# Walkthrough: Use the AWS CLI with Run Command<a name="walkthrough-cli"></a>

The following sample walkthrough shows you how to use the AWS CLI to view information about commands and command parameters, how to run commands, and how to view the status of those commands\. 

**Important**  
Only trusted administrators should be allowed to use Systems Manager pre\-configured documents shown in this topic\. The commands or scripts specified in Systems Manager documents run with administrative privilege on your instances\. If a user has permission to run any of the pre\-defined Systems Manager documents \(any document that begins with AWS\), then that user also has administrator access to the instance\. For all other users, you should create restrictive documents and share them with specific users\. For more information about restricting access to Run Command, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

**Topics**
+ [Step 1: Getting Started](#walkthrough-cli-settings)
+ [Step 2: Run Shell Scripts](#walkthrough-cli-run-scripts)
+ [Step 3: Send a Command Using the AWS\-RunShellScript document \- Example 1](#walkthrough-cli-example-1)
+ [Step 4: Send a Command Using the AWS\-RunShellScript document \- Example 2](#walkthrough-cli-example-2)
+ [Additional Examples](#walkthrough-cli-examples)

## Step 1: Getting Started<a name="walkthrough-cli-settings"></a>

You must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permission in IAM\. Also note, this example uses the US East \(Ohio\) Region \(us\-east\-2\)\. Run Command is currently available in the AWS Regions listed in [AWS Systems Manager](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *Amazon Web Services General Reference*\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

**To run commands using the AWS CLI**

1. Run the following command to specify your credentials and the region\.

   ```
   aws configure
   ```

1. The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region-id
   Default output format [None]: ENTER
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\. The command returns a list of Linux and Windows documents\.

   ```
   aws ssm list-documents
   ```

1. Verify that an instance is ready to receive commands

   The output of the following command shows if instances are online\.

   ```
   aws ssm describe-instance-information --output text --query "InstanceInformationList[*]"
   ```

1. Use the following command to view details about a particular instance\.
**Note**  
To run the commands in this walkthrough, you must replace the instance and command IDs\. The command ID is returned as a response of the send\-command\. The instance ID is available from the Amazon EC2 console\.

   ```
   aws ssm describe-instance-information --instance-information-filter-list key=InstanceIds,valueSet=instance ID
   ```

## Step 2: Run Shell Scripts<a name="walkthrough-cli-run-scripts"></a>

Using Run Command and the AWS\-RunShellScript document, you can run any command or script on an EC2 instance as if you were logged on locally\.

**To view the description and available parameters**
+ Use the following command to view a description of the Systems Manager JSON document\.

  ```
  aws ssm describe-document --name "AWS-RunShellScript" --query "[Document.Name,Document.Description]"
  ```
+ Use the following command to view the available parameters and details about those parameters\.

  ```
  aws ssm describe-document --name "AWS-RunShellScript" --query "Document.Parameters[*]"
  ```

## Step 3: Send a Command Using the AWS\-RunShellScript document \- Example 1<a name="walkthrough-cli-example-1"></a>

Use the following command to get IP information for an instance\.

```
aws ssm send-command --instance-ids "instance ID" --document-name "AWS-RunShellScript" --comment "IP config" --parameters commands=ifconfig --output text
```

**Get command information with response data**  
The following command uses the Command ID that was returned from the previous command to get the details and response data of the command execution\. The system returns the response data if the command completed\. If the command execution shows "Pending" you will need to run this command again to see the response data\.

```
aws ssm list-command-invocations --command-id $sh_command_id --details
```

## Step 4: Send a Command Using the AWS\-RunShellScript document \- Example 2<a name="walkthrough-cli-example-2"></a>

The following command displays the default user account running the commands\. 

```
sh_command_id=$(aws ssm send-command --instance-ids "instance ID" --document-name "AWS-RunShellScript" --comment "Demo run shell script on Linux Instance" --parameters commands=whoami --output text --query "Command.CommandId")
```

**Get command status**  
The following command uses the Command ID to get the status of the command execution on the instance\. This example uses the Command ID that was returned in the previous command\. 

```
aws ssm list-commands  --command-id "command_ID"
```

**Get command details**  
The following command uses the Command ID from the previous command to get the status of the command execution on a per instance basis\.

```
aws ssm list-command-invocations --command-id "command_ID" --details
```

**Get command information with response data for a specific instance**  
The following command returns the output of the original aws ssm send\-command for a specific instance\. 

```
aws ssm list-command-invocations --instance-id instance ID --command-id "command_ID" --details
```

## Additional Examples<a name="walkthrough-cli-examples"></a>

The following command returns the version of Python running on an instance\.

```
sh_command_id=$(aws ssm send-command --instance-ids "instance ID" --document-name "AWS-RunShellScript" --comment "Demo run shell script on Linux Instances" --parameters commands='python -V' --output text --query "Command.CommandId") sh -c 'aws ssm list-command-invocations --command-id "$sh_command_id" --details --query "CommandInvocations[].CommandPlugins[].{Status:Status,Output:Output}"' 
```

The following command runs a Python script using Run Command\.

```
sh_command_id=$(aws ssm send-command --instance-ids "instance ID" --document-name "AWS-RunShellScript" --comment "Demo run shell script on Linux Instances" --parameters '{"commands":["#!/usr/bin/python","print \"Hello world from python\""]}' --output text --query "Command.CommandId") sh -c 'aws ssm list-command-invocations --command-id "$sh_command_id" --details --query "CommandInvocations[].CommandPlugins[].{Status:Status,Output:Output}"'
```