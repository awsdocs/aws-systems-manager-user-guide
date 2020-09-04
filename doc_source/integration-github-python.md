# Run Python scripts from GitHub<a name="integration-github-python"></a>

This section includes procedures to help you run Python scripts from GitHub by using either the console or the AWS CLI\. 

## Run a Python script from GitHub \(console\)<a name="integration-github-python-console"></a>

**Run a Python script from GitHub**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. For **Command parameters**, do the following:
   + In **Source Type**, select **GitHub**\. 
   + In the **Source Info** box, type the required information to access the source in the following format:

     ```
     {
         "owner": "owner_name",
         "repository": "repository_name",
         "branch": "branch_name",
         "path": "path_to_document",
         "tokenInfo": "{{ssm-secure:SecureString_parameter_name}}"
     }
     ```

     The following example downloads a directory of scripts named *complex\-script*\.

     ```
     {
         "owner": "TestUser1",
         "repository": "SSMTestDocsRepo",
         "branch": "myBranch",
         "path": "scripts/python/complex-script",
         "tokenInfo": "{{ssm-secure:myAccessTokenParam}}"
     }
     ```
**Note**  
`"branch"` is required only if your scripts are stored in a branch other than `master`\.  
To use the version of your scripts that are in a particular *commit* in your repository, use `commitID` with `getOptions` instead of `branch`\. For example:  
`"getOptions": "commitID:bbc1ddb94...b76d3bEXAMPLE",`
   + For **Command Line**, type parameters for the script execution\. Here is an example\.

     ```
     mainFile.py argument-1 argument-2
     ```

     This example runs `mainFile.py`, which can then run other scripts in the `complex-script` directory\.
   + \(Optional\) For **Working Directory**, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) For **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

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

## Run a Python script from GitHub by using the AWS CLI<a name="integration-github-python-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to download and run a script from GitHub\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\":\"repository_name\", \"path\": \"path_to_script_or_directory"}"],"commandLine":["commands_to_run"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-02573cafcfEXAMPLE" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"TestUser1\", \"repository\":\"GitHubTestPublic\", \"path\": \"scripts/python/complex-script\"}"],"commandLine":["mainFile.py argument-1 argument-2 "]}'
   ```

   This example downloads a directory of scripts called `complex-script`\. The `commandLine` entry runs `mainFile.py`, which can then run other scripts in the `complex-script` directory\.