# Tutorial: Update application dependencies, patch a managed node, and perform an application\-specific health check<a name="aws-runpatchbaselinewithhooks-tutorial"></a>

In many cases, a managed node must be rebooted after it has been patched with the latest software update\. However, rebooting a node in production without safeguards in place can cause several problems, such as invoking alarms, recording incorrect metric data, and interrupting data synchronizations\.

This tutorial demonstrates how to avoid problems like these by using the AWS Systems Manager document \(SSM document\) `AWS-RunPatchBaselineWithHooks` to achieve a complex, multi\-step patching operation that accomplishes the following:

1. Prevent new connections to the application

1. Install operating system updates

1. Update the package dependencies of the application

1. Restart the system

1. Perform an application\-specific health check

For this example, we have set up our infrastructure this way:
+ The virtual machines targeted are registered as managed nodes with Systems Manager\.
+ `Iptables` is used as a local firewall\.
+ The application hosted on the managed nodes is running on port 443\.
+ The application hosted on the managed nodes is a `nodeJS` application\.
+ The application hosted on the managed nodes is managed by the pm2 process manager\.
+ The application already has a specified health check endpoint\.
+ The application’s health check endpoint requires no end user authentication\. The endpoint allows for a health check that meets the organization’s requirements in establishing availability\. \(In your environments, it might be enough to simply ascertain that the `nodeJS` application is running and able to listen for requests\. In other cases, you might want to also verify that a connection to the caching layer or database layer has already been established\.\)

The examples in this tutorial are for demonstration purposes only and not meant to be implemented as\-is into production environments\. Also, keep in mind that the lifecycle hooks feature of Patch Manager, a capability of Systems Manager, with the `AWS-RunPatchBaselineWithHooks` document can support numerous other scenarios\. Here are several examples\.
+ Stop a metrics reporting agent before patching and restarting it after the managed node reboots\.
+ Detach the managed node from a CRM or PCS cluster before patching and reattach after the node reboots\.
+ Update third\-party software \(for example, Java, Tomcat, Adobe applications, and so on\) on Windows Server machines after operating system \(OS\) updates are applied, but before the managed node reboots\.

**To update application dependencies, patch a managed node, and perform an application\-specific health check**

1. Create an SSM document for your preinstallation script with the following contents and name it `NodeJSAppPrePatch`\. Replace *your\_application* with the name of your application\.

   This script immediately blocks new incoming requests and provides five seconds for already active ones to complete before beginning the patching operation\. For the `sleep` option, specify a number of seconds greater than it usually takes for incoming requests to complete\.

   ```
   # exit on error
   set -e
   # set up rule to block incoming traffic
   iptables -I INPUT -j DROP -p tcp --syn --destination-port 443 || exit 1
   # wait for current connections to end. Set timeout appropriate to your application's latency
   sleep 5 
   # Stop your application
   pm2 stop your_application
   ```

   For information about creating SSM documents, see [Creating SSM document content](documents-creating-content.md)\.

1. Create another SSM document with the following content for your postinstall script to update your application dependencies and name it `NodeJSAppPostPatch`\. Replace */your/application/path* with the path to your application\.

   ```
   cd /your/application/path
   npm update 
   # you can use npm-check-updates if you want to upgrade major versions
   ```

1. Create another SSM document with the following content for your `onExit` script to bring your application back up and perform a health check\. Name this SSM document `NodeJSAppOnExitPatch`\. Replace *your\_application* with the name of your application\.

   ```
   # exit on error
   set -e
   # restart nodeJs application
   pm2 start your_application
   # sleep while your application starts and to allow for a crash
   sleep 10
   # check with pm2 to see if your application is running
   pm2 pid your_application
   # re-enable incoming connections
   iptables -D INPUT -j DROP -p tcp --syn --destination-port 
   # perform health check
   /usr/bin/curl -m 10 -vk -A "" http://localhost:443/health-check || exit 1
   ```

1. Create an association in State Manager, a capability of AWS Systems Manager, to issue the operation by performing the following steps:

   1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

   1. For **Name**, provide a name to help identify the purpose of the association\.

   1. In the **Document** list, choose `AWS-RunPatchBaselineWithHooks`\.

   1. For **Operation**, choose **Install**\.

   1. \(Optional\) For **Snapshot Id**, provide a GUID that you generate to help speed up the operation and ensure consistency\. The GUID value can be as simple as `00000000-0000-0000-0000-111122223333`\.

   1. For **Pre Install Hook Doc Name**, enter `NodeJSAppPrePatch`\. 

   1. For **Post Install Hook Doc Name**, enter `NodeJSAppPostPatch`\. 

   1. For **On ExitHook Doc Name**,enter `NodeJSAppOnExitPatch`\. 

1. For **Targets**, identify your managed nodes by specifying tags, choosing nodes manually, choosing a resource group, or choosing all managed nodes\.

1. For **Specify schedule**, specify how often to run the association\. For managed node patching, once per week is a common cadence\.

1. In the **Rate control** section, choose options to control how the association runs on multiple managed nodes\. Ensure that only a portion of managed nodes are updated at a time\. Otherwise, all or most of your fleet could be taken offline at once\. For more information about using rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the managed node, not those of the IAM user performing this task\. For more information, see [Configure instance permissions for Systems Manager](setup-instance-permissions.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, verify that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. Choose **Create Association**\.