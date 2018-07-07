# Using Automation with Jenkins<a name="automation-jenkins"></a>

If your organization uses Jenkins software in a CI/CD pipeline, you can add Automation as a post\-build step to pre\-install application releases into Amazon Machine Images \(AMIs\)\. You can also use the Jenkins scheduling feature to call Automation and create your own operating system \(OS\) patching cadence\.

The example below shows how to invoke Automation from a Jenkins server that is running either on\-premises or in Amazon EC2\. For authentication, the Jenkins server uses AWS credentials based on an AWS Identity and Access Management \(IAM\) user that you create in the example\. If your Jenkins server is running in Amazon EC2, you can also authenticate it using an IAM instance profile role\.

**Note**  
Be sure to follow Jenkins security best\-practices when configuring your instance\.

**Before You Begin**  
Complete the following tasks before you configure Automation with Jenkins\.
+ Complete the [Simplify AMI Patching Using Automation, Lambda, and Parameter Store](automation-simpatch.md) example\. The following example uses the **UpdateMyLatestWindowsAmi** automation document created in that example\.
+ Configure IAM roles for Automation\. Systems Manager requires an instance profile role and a service role ARN to process Automation workflows\. For more information, see [Setting Up Automation](automation-setup.md)\.
+ After you configure IAM roles for Automation, use the following procedure to create an IAM user account for your Jenkins server\. The Automation workflow uses the IAM user account's Access key and Secret key to authenticate the Jenkins server during execution\.

**To create a user account for the Jenkins server**

1. From the **Users** page on the [IAM console](https://console.aws.amazon.com/iam/home#users), choose **Add User**\.

1. In the **Set user details** section, specify a user name \(for example, *Jenkins*\)\.

1. In the **Select AWS access type** section, choose **Programmatic Access**\.

1. Choose **Next:Permissions**\.

1. In the **Set permissions for** section, choose **Attach existing policies directly**\.

1. In the filter field, type **AmazonSSMFullAccess**\.

1. Choose the checkbox beside the policy, and then choose **Next:Review**\.

1. Verify the details, and then choose **Create**\.

1. Copy the Access and Secret keys to a text file\. You will specify these credentials in the next procedure\.

Use the following procedure to configure the AWS CLI on your Jenkins server\.

**To configure the Jenkins server for Automation**

1. If it's not already installed, download the AWS CLI to your Jenkins server\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1. In a terminal window on your Jenkins server, run the following commands to configure the AWS CLI\.

   ```
   sudo su – jenkins
   aws configure
   ```

1. When prompted, enter the AWS Access key and Secret key you received when you created the Jenkins user in IAM\. Specify a default region\. For more information about configuring the AWS CLI see [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

Use the following procedure to configure your Jenkins project to invoke Automation\.

**To configure your Jenkins server to invoke Automation**

1. Open the Jenkins console in a web browser\.

1. Choose the project that you want to configure with Automation, and then choose **Configure**\.

1. On the **Build** tab, choose **Add Build Step**\.

1. Choose **Execute shell** or **Execute Windows batch command** \(depending on your operating system\)\.

1. In the **Command** box, run an AWS CLI command like the following:

   ```
   aws --region the region of your source AMI ssm start-automation-execution --document-name your document name --parameters parameters for the document
   ```

   The following example command uses the **UpdateMyLatestWindowsAmi** document and the Systems Manager Parameter `latestAmi` created in [Simplify AMI Patching Using Automation, Lambda, and Parameter Store](automation-simpatch.md):

   ```
   aws --region region-id ssm start-automation-execution \
       --document-name UpdateMyLatestWindowsAmi \
       --parameters \
           "sourceAMIid='{{ssm:latestAmi}}'"
   ```

   In Jenkins, the command looks like the example in the following screenshot\.  
![\[Jenkins information\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-ami-jenkins2.png)

1. In the Jenkins project, choose **Build Now**\. Jenkins returns output similar to the following example\.  
![\[Jenkins information\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-ami-jenkins.png)