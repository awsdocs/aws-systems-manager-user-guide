# Walkthrough: Use the AWS CLI with Run Command<a name="walkthrough-cli"></a>

The following sample walkthrough shows you how to use the AWS Command Line Interface \(AWS CLI\) to view information about commands and command parameters, how to run commands, and how to view the status of those commands\. 

**Important**  
Only trusted administrators should be allowed to use AWS Systems Manager pre\-configured documents shown in this topic\. The commands or scripts specified in Systems Manager documents run with administrative permissions on your managed nodes\. If a user has permission to run any of the pre\-defined Systems Manager documents \(any document that begins with `AWS-`\), then that user also has administrator access to the node\. For all other users, you should create restrictive documents and share them with specific users\.

**Topics**
+ [Step 1: Getting started](#walkthrough-cli-settings)
+ [Step 2: Run shell scripts to view resource details](#walkthrough-cli-run-scripts)
+ [Step 3: Send simple commands using the `AWS-RunShellScript` document](#walkthrough-cli-example-1)
+ [Step 4: Run a simple Python script using Run Command](#walkthrough-cli-example-2)
+ [Step 5: Run a Bash script using Run Command](#walkthrough-cli-example-3)

## Step 1: Getting started<a name="walkthrough-cli-settings"></a>

You must either have administrator permissions on the managed node you want to configure or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\. Also note, this example uses the US East \(Ohio\) Region \(us\-east\-2\)\. Run Command is available in the AWS Regions listed in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

**To run commands using the AWS CLI**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. List all available documents\.

   This command lists all of the documents available for your account based on IAM permissions\. 

   ```
   aws ssm list-documents
   ```

1. Verify that an managed node is ready to receive commands\.

   The output of the following command shows if managed nodes are online\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-instance-information \
       --output text --query "InstanceInformationList[*]"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-information ^
       --output text --query "InstanceInformationList[*]"
   ```

------

1. Run the following command to view details about a particular managed node\.
**Note**  
To run the commands in this walkthrough, replace the instance and command IDs\. For managed AWS IoT Greengrass core devices, use the mi\-*ID\_number* for instance ID\. The command ID is returned as a response to send\-command\. Instance IDs are available from Fleet Manager, a capability of AWS Systems Manager\.\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-instance-information \
       --instance-information-filter-list key=InstanceIds,valueSet=instance-ID
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-information ^
       --instance-information-filter-list key=InstanceIds,valueSet=instance-ID
   ```

------

## Step 2: Run shell scripts to view resource details<a name="walkthrough-cli-run-scripts"></a>

Using Run Command and the `AWS-RunShellScript` document, you can run any command or script on a managed node as if you were logged on locally\.

**View the description and available parameters**

Run the following command to view a description of the Systems Manager JSON document\.

------
#### [ Linux & macOS ]

```
aws ssm describe-document \
    --name "AWS-RunShellScript" \
    --query "[Document.Name,Document.Description]"
```

------
#### [ Windows ]

```
aws ssm describe-document ^
    --name "AWS-RunShellScript" ^
    --query "[Document.Name,Document.Description]"
```

------

Run the following command to view the available parameters and details about those parameters\.

------
#### [ Linux & macOS ]

```
aws ssm describe-document \
    --name "AWS-RunShellScript" \
    --query "Document.Parameters[*]"
```

------
#### [ Windows ]

```
aws ssm describe-document ^
    --name "AWS-RunShellScript" ^
    --query "Document.Parameters[*]"
```

------

## Step 3: Send simple commands using the `AWS-RunShellScript` document<a name="walkthrough-cli-example-1"></a>

Run the following command to get IP information for a Linux managed node\.

If you're targeting a Windows Server managed node, change the `document-name` to `AWS-RunPowerShellScript` and change the `command` from `ifconfig` to `ipconfig`\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --instance-ids "instance-ID" \
    --document-name "AWS-RunShellScript" \
    --comment "IP config" \
    --parameters commands=ifconfig \
    --output text
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --instance-ids "instance-ID" ^
    --document-name "AWS-RunShellScript" ^
    --comment "IP config" ^
    --parameters commands=ifconfig ^
    --output text
```

------

**Get command information with response data**  
The following command uses the Command ID that was returned from the previous command to get the details and response data of the command execution\. The system returns the response data if the command completed\. If the command execution shows `"Pending"` or `"InProgress"` you run this command again to see the response data\.

------
#### [ Linux & macOS ]

```
aws ssm list-command-invocations \
    --command-id $sh-command-id \
    --details
```

------
#### [ Windows ]

```
aws ssm list-command-invocations ^
    --command-id $sh-command-id ^
    --details
```

------

**Identify user account**

The following command displays the default user account running the commands\. 

------
#### [ Linux & macOS ]

```
sh_command_id=$(aws ssm send-command \
    --instance-ids "instance-ID" \
    --document-name "AWS-RunShellScript" \
    --comment "Demo run shell script on Linux managed node" \
    --parameters commands=whoami \
    --output text \
    --query "Command.CommandId")
```

------

**Get command status**  
The following command uses the Command ID to get the status of the command execution on the managed node\. This example uses the Command ID that was returned in the previous command\. 

------
#### [ Linux & macOS ]

```
aws ssm list-commands \
    --command-id "command-ID"
```

------
#### [ Windows ]

```
aws ssm list-commands ^
    --command-id "command-ID"
```

------

**Get command details**  
The following command uses the Command ID from the previous command to get the status of the command execution on a per managed node basis\.

------
#### [ Linux & macOS ]

```
aws ssm list-command-invocations \
    --command-id "command-ID" \
    --details
```

------
#### [ Windows ]

```
aws ssm list-command-invocations ^
    --command-id "command-ID" ^
    --details
```

------

**Get command information with response data for a specific managed node**  
The following command returns the output of the original `aws ssm send-command` request for a specific managed node\. 

------
#### [ Linux & macOS ]

```
aws ssm list-command-invocations \
    --instance-id instance-ID \
    --command-id "command-ID" \
    --details
```

------
#### [ Windows ]

```
aws ssm list-command-invocations ^
    --instance-id instance-ID ^
    --command-id "command-ID" ^
    --details
```

------

**Display Python version**

The following command returns the version of Python running on a node\.

------
#### [ Linux & macOS ]

```
sh_command_id=$(aws ssm send-command \
    --instance-ids "instance-ID" \
    --document-name "AWS-RunShellScript" \
    --comment "Demo run shell script on Linux Instances" \
    --parameters commands='python -V' \
    --output text --query "Command.CommandId") sh -c 'aws ssm list-command-invocations \
    --command-id "$sh_command_id" \
    --details \
    --query "CommandInvocations[].CommandPlugins[].{Status:Status,Output:Output}"'
```

------

## Step 4: Run a simple Python script using Run Command<a name="walkthrough-cli-example-2"></a>

The following command runs a simple Python "Hello World" script using Run Command\.

------
#### [ Linux & macOS ]

```
sh_command_id=$(aws ssm send-command \
    --instance-ids "instance-ID" \
    --document-name "AWS-RunShellScript" \
    --comment "Demo run shell script on Linux Instances" \
    --parameters '{"commands":["#!/usr/bin/python","print \"Hello World from python\""]}' \
    --output text \
    --query "Command.CommandId") sh -c 'aws ssm list-command-invocations \
    --command-id "$sh_command_id" \
    --details \
    --query "CommandInvocations[].CommandPlugins[].{Status:Status,Output:Output}"'
```

------

## Step 5: Run a Bash script using Run Command<a name="walkthrough-cli-example-3"></a>

The examples in this section demonstrate how to run the following bash script using Run Command\.

For examples of using Run Command to run scripts stored in remote locations, see [Running scripts from Amazon S3](integration-s3.md) and [Running scripts from GitHub](integration-remote-scripts.md)\.

```
#!/bin/bash
yum -y update
yum install -y ruby
cd /home/ec2-user
curl -O https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/install
chmod +x ./install
./install auto
```

This script installs the AWS CodeDeploy agent on Amazon Linux and Red Hat Enterprise Linux \(RHEL\) instances, as described in [Create an Amazon EC2 instance for CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-create.html) in the *AWS CodeDeploy User Guide*\.

The script installs the CodeDeploy agent from an AWS managed Amazon S3 bucket in thee US East \(Ohio\) Region \(us\-east\-2\), `aws-codedeploy-us-east-2`\.

**Run a bash script in an AWS CLI command**

The following sample demonstrates how to include the bash script in a CLI command using the `--parameters` option\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name "AWS-RunShellScript" \
    --targets '[{"Key":"InstanceIds","Values":["instance-id"]}]' \
    --parameters '{"commands":["#!/bin/bash","yum -y update","yum install -y ruby","cd /home/ec2-user","curl -O https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/install","chmod +x ./install","./install auto"]}'
```

------

**Run a bash script in a JSON file**

In the following example, the content of the bash script is stored in a JSON file, and the file is included in the command using the `--cli-input-json` option\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name "AWS-RunShellScript" \
    --targets "Key=InstanceIds,Values=instance-id" \
    --cli-input-json file://installCodeDeployAgent.json
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name "AWS-RunShellScript" ^
    --targets "Key=InstanceIds,Values=instance-id" ^
    --cli-input-json file://installCodeDeployAgent.json
```

------

The contents of the referenced `installCodeDeployAgent.json` file is shown in the following example\.

```
{
    "Parameters": {
        "commands": [
            "#!/bin/bash",
            "yum -y update",
            "yum install -y ruby",
            "cd /home/ec2-user",
            "curl -O https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/install",
            "chmod +x ./install",
            "./install auto"
        ]
    }
}
```