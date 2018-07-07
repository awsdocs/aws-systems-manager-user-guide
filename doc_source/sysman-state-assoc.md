# Create an Association \(Console\)<a name="sysman-state-assoc"></a>

This section describes how to create a State Manager association by using the Amazon EC2 console\. The example in this section shows you how to create an association based on a custom SSM document\. If this is your first time creating an association, we recommend that you perform this procedure in a test environment\. For an example of creating an association using the AWS CLI, see [Walkthrough: Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.

**Before You Begin**  
Before you complete the following procedure, verify that you have at least one instance running that is configured for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a State Manager association \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create document**\.

1. In the **Name** field, type a descriptive name that identifies this document as a test document for State Manager\.

1. In the **Document type** list, choose **Command document**\.

1. In the **Content** area:
   + Select the button next to **JSON**\.
   + Delete the pre\-populated brackets \{\} in the **Content **, and then copy and paste the following sample document into the **Content** field\. 

     This document includes one step that invokes the **aws:runPowerShellScript** plugin to return the instance host name\. This document can be run on Windows instances\.

     ```
     {
        "schemaVersion":"2.0",
        "description":"Sample document",
        "mainSteps":[
           {
              "action":"aws:runPowerShellScript",
              "name":"runPowerShellScript",
              "inputs":{
                 "runCommand":[
                    "hostname"
                 ]
              }
           }
        ]
     }
     ```

1. In the navigation pane, choose **State Manager**\. 

1. Choose **Create association**\.

1. In the **Name** box, type a descriptive name for this association\. For example, name the association **TestHostnameAssociation**\.

1. In the **Command document** list, choose the document you just created\.

1. In the **Document version** list, leave the default value\.

1. Disregard the **Parameters** section, as the test document does not take parameters\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. Disregard the **Output options** section\. Enabling the storage of command output in an S3 bucket is described in the next procedure, [Edit and Create a New Version of an Association \(Console\)](sysman-state-assoc-version.md)\.

1. Choose **Create association**\. The system attempts to create the association on the instance and immediately apply the state\. In this case, after creating the association, the system attempts to return the host name\. The association status shows **Pending**\.

1. Choose your browser's Refresh button\. The status changes to **Success**\.

**To create a State Manager association \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Documents**\.

1. Choose **Create Document**\.

1. In the **Name** field, type a descriptive name that identifies this document as a test document for State Manager\.

1. In the **Document type** list, choose **Command**\.

1. Delete the pre\-populated brackets \{\} in the **Content field**, and then copy and paste the following sample document into the **Content** field\. 

   This document includes one step that invokes the aws:runPowerShellScript plugin to return the instance host name\. This document can be run on Windows instances\.

   ```
   {
      "schemaVersion":"2.0",
      "description":"Sample document",
      "mainSteps":[
         {
            "action":"aws:runPowerShellScript",
            "name":"runPowerShellScript",
            "inputs":{
               "runCommand":[
                  "hostname"
               ]
            }
         }
      ]
   }
   ```

1. Choose **Create document**\. After the system creates the document, choose **Close**\.

1. In the EC2 console navigation pane, expand **Systems Manager Services**, and then choose **State Manager**\. 

1. Choose **Create Association**\.

1. In the **Association Name**, specify a descriptive name for this association\. For example, specify TestHostnameAssociation\.

1. In the **Select Document** section, choose the document you just created\.

1. In the **Document Version** list, leave the default value\.

1. In the **Select Targets by** section, choose an option\.

1. In the **Schedule** section, choose an option\.

1. Disregard the **Parameters** section, as the test document does not take parameters\. Also, disregard the **Write to S3** option, because using that option is described in the next procedure\.

1. Choose **Create Association**\. The system attempts to create the association on the instance and immediately apply the state\. In this case, after creating the association, the system attempts to return the host name\. The association status shows **Pending**\.

1. In the right corner of the **Association** page, choose the refresh button\. The status changes to **Success**\.

You can't view the output from this procedure \(the instance name\) because the it written to Amazon S3\. 

For information about editing an association, writing the output to an Amazon S3 bucket, and viewing the instance name, see the next procedure, [Edit and Create a New Version of an Association \(Console\)](sysman-state-assoc-version.md)\.