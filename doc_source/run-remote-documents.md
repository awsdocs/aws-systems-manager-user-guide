# Running Documents from Remote Locations<a name="run-remote-documents"></a>

You can run SSM documents from remote locations by using the `AWS-RunDocument` pre\-defined SSM document\. This document currently supports the following remote locations:
+ GitHub repositories \(public and private\)
+ Amazon S3
+ Documents saved in Systems Manager

The following procedure describes how to run remote SSM documents by using the console\. This procedure shows how to run the remote document by using Run Command, but you can also run remote documents by using State Manager or Automation\.

**Before You Begin**  
Before you run a remote document, you must complete the following tasks\.
+ Create an SSM document and save it in a remote location\. For more information, see [Creating Systems Manager Documents](create-ssm-doc.md)
+ If you plan to run a remote document that is stored in a private GitHub repository, then you must create a Systems Manager `SecureString` parameter for your GitHub security access token\. You can't access a remote document in a private GitHub repository by manually passing your token over SSH\. The access token must be passed as a Systems Manager `SecureString` parameter\. For more information about creating a `SecureString` parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

## Run a Remote Document \(Console\)<a name="run-remote-documents-console"></a>

**To run a remote document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Document** list, choose **AWS\-RunDocument**\.

1. In **Command parameters**, for **Source Type**, choose an option\. 
   + If you choose **GitHub**, specify **Source Info** information in the following format:

     ```
     {
         "owner": "owner_name",
         "repository": "repository_name",
         "branch": "branch_name",
         "path": "path_to_document",
         "tokenInfo": "{{ssm-secure:SecureString_parameter_name}}"
     }
     ```

     For example:

     ```
     {
         "owner": "TestUser1",
         "repository": "SSMTestDocsRepo",
         "path": "SSMDocs/mySSMdoc.yml",
         "branch": "myBranch",
         "tokenInfo": "{{ssm-secure:myAccessTokenParam}}"
     }
     ```
**Note**  
`"branch"` is required only if your SSM document is stored in a branch other than `master`\.  
To use the version of your SSM document in a particular *commit* in your repository, use `commitID` with `getOptions` instead of `branch`\. For example:  

     ```
     "getOptions": "commitID:bbc1ddb94...b76d3bEXAMPLE",
     ```
   + If you choose **S3**, specify **Source Info** information in the following format:

     ```
     {"path":"URL_to_document_in_S3"}
     ```

     For example:

     ```
     {"path":"https://s3.amazonaws.com/aws-executecommand-test/scripts/ruby/mySSMdoc.json"}
     ```
   + If you choose **SSMDocument**, specify **Source Info** information in the following format:

     ```
     {"name": "document_name"}
     ```

     For example:

     ```
     {"name": "mySSMdoc"}
     ```

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. In the **Document Parameters** field, type parameters for the remote SSM document\. For example, if you run the AWS\-RunPowerShell document, you could specify:

   ```
   {"commands": ["date", "echo \"Hello World\""]}
   ```

   If you run the AWS\-ConfigureAWSPack document, you could specify:

   ```
   {
      "action":"Install",
      "name":"AWSPVDriver"
   }
   ```

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an Amazon S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager Status Changes Using Amazon SNS Notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting Managed Instance from Scripts](send-commands-reboot.md)\.