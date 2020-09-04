# Running Systems Manager command documents from remote locations<a name="run-remote-documents"></a>

You can run Systems Manager documents \(SSM documents\) from remote locations by using the `AWS-RunDocument` pre\-defined SSM document\. This document currently supports running SSM documents stored in the following locations:
+ GitHub repositories \(public and private\)
+ Amazon S3 buckets
+ Systems Manager

While you can also run remote documents by using State Manager or Automation, the following procedure describes only how to run remote SSM documents by using Run Command in the AWS Systems Manager console\. 

**Note**  
`AWS-RunDocument` can be used to run only command\-type SSM documents, not other types such as Automation documents\. The `AWS-RunDocument` uses the `aws:downloadContent` plugin\. For more information about the `aws:downloadContent` plugin, see [aws:downloadContent](ssm-plugins.md#aws-downloadContent)\.

**Before you begin**  
Before you run a remote document, you must complete the following tasks\.
+ Create an SSM command document and save it in a remote location\. For more information, see [Creating Systems Manager documents](create-ssm-doc.md)
+ If you plan to run a remote document that is stored in a private GitHub repository, then you must create a Systems Manager `SecureString` parameter for your GitHub security access token\. You can't access a remote document in a private GitHub repository by manually passing your token over SSH\. The access token must be passed as a Systems Manager `SecureString` parameter\. For more information about creating a `SecureString` parameter, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

## Run a remote document \(console\)<a name="run-remote-documents-console"></a>

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
         "path": "path_to_document",
         "getOptions":"branch:branch_name",
         "tokenInfo": "{{ssm-secure:secure-string-token}}"
     }
     ```

     For example:

     ```
     {
         "owner":"TestUser",
         "repository":"GitHubTestExamples",
         "path":"scripts/python/test-script",
         "getOptions":"branch:exampleBranch",
         "tokenInfo":"{{ssm-secure:my-secure-string-token}}"
     }
     ```
**Note**  
`getOptions` are extra options to retrieve content from a branch other than master, or from a specific commit in the repository\. `getOptions` can be omitted if you are using the latest commit in the master branch\. The `branch` parameter is required only if your SSM document is stored in a branch other than `master`\.  
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
     {"path":"https://s3.amazonaws.com/DOC-EXAMPLE-BUCKET/scripts/ruby/mySSMdoc.json"}
     ```
   + If you choose **SSMDocument**, specify **Source Info** information in the following format:

     ```
     {"name": "document_name"}
     ```

     For example:

     ```
     {"name": "mySSMdoc"}
     ```

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

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting managed instance from scripts](send-commands-reboot.md)\.