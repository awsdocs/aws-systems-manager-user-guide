# Run Ruby scripts from Amazon S3<a name="integration-s3-ruby"></a>

This section includes procedures to help you run Ruby scripts from Amazon Simple Storage Service \(Amazon S3\) by using either the AWS Systems Manager console or the AWS Command Line Interface \(AWS CLI\)\.

## Run a Ruby script from Amazon S3 \(console\)<a name="integration-s3-ruby-console"></a>

**Run a Ruby script from Amazon S3**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **`AWS-RunRemoteScript`**\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select **S3**\. 
   + In the **Source Info** text box, enter the required information to access the source in the following format\.

     ```
     {"path":"https://s3.amazonaws.com/path_to_script"}
     ```

     Following is an example\.

     ```
     {"path":"https://s3.amazonaws.com/doc-example-bucket/scripts/ruby/helloWorld.rb"}
     ```
   + In the **Command Line** field, enter parameters for the script execution\. Here is an example\.

     ```
     helloWorld.rb argument-1 argument-2
     ```
   + \(Optional\) In the **Working Directory** field, enter the name of a directory on the node where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, enter information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

## Run a Ruby script from Amazon S3 by using the AWS CLI<a name="integration-s3-ruby-cli"></a>

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command\.

------
#### [ Linux & macOS ]

   ```
   aws ssm send-command \
       --document-name "AWS-RunRemoteScript" \
       --targets "Key=instanceids,Values=instance-IDs" \
       --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command \
       --document-name "AWS-RunRemoteScript" \
       --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" \
       --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/doc-example-bucket/scripts/ruby/helloWorld.rb\"}"],"commandLine":["helloWorld.rb argument-1 argument-2"]}'
   ```

------
#### [ Windows ]

   ```
   aws ssm send-command ^    
       --document-name "AWS-RunRemoteScript" ^
       --targets "Key=instanceids,Values=instance-IDs" ^
       --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/path_to_script\"}',"commandLine"="script_name_and_arguments"
   ```

   Here is an example\.

   ```
   aws ssm send-command ^
       --document-name "AWS-RunRemoteScript" ^
       --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" ^
       --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/doc-example-bucket/scripts/ruby/helloWorld.rb\"}',"commandLine"="helloWorld.rb argument-1 argument-2"
   ```

------