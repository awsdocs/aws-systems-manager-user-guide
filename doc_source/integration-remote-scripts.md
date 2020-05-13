# Running scripts from GitHub and Amazon S3<a name="integration-remote-scripts"></a>

This section describes how to use the `AWS-RunRemoteScript` pre\-defined SSM document to download scripts from GitHub and Amazon S3, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. By using this document, you no longer need to manually port scripts into Amazon EC2 or wrap them in SSM documents\. Systems Manager integration with GitHub and Amazon S3 promotes *infrastructure as code*, which reduces the time it takes to manage instances while standardizing configurations across your fleet\. 

You can also create custom SSM documents that enable you to download and run scripts or other SSM documents from remote locations\. For more information, see [Creating composite documents](composite-docs.md)\.

**Topics**
+ [Running scripts from GitHub](#integration-github)
+ [Running scripts from Amazon S3](#integration-s3)

## Running scripts from GitHub<a name="integration-github"></a>

This section describes how to download and run scripts from a private or public GitHub repository\. You can run different types of scripts, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. 

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts \(as long as the referenced scripts are included in the directory\)\.

Note the following important details about running scripts from GitHub\.
+ Systems Manager does not check to see if your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ You are responsible for ensuring that all GitHub requirements are met\. This includes refreshing your access token, as needed\. You must also ensure that you don't surpass the number of authenticated or unauthenticated requests\. For more information, see the GitHub documentation\.

**Topics**
+ [Run Ansible Playbooks from GitHub](#integration-github-ansible)
+ [Run Python scripts from GitHub](#integration-github-python)

### Run Ansible Playbooks from GitHub<a name="integration-github-ansible"></a>

This section includes procedures to help you run Ansible Playbooks from GitHub by using either the console or the AWS CLI\.

**Before You Begin**  
If you plan to run a script that is stored in a private GitHub repository, then you must create a Systems Manager `SecureString` parameter for your GitHub security access token\. You can't access a script in a private GitHub repository by manually passing your token over SSH\. The access token must be passed as a Systems Manager `SecureString` parameter\. For more information about creating a `SecureString` parameter, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

#### Run an Ansible Playbook from GitHub \(console\)<a name="integration-github-ansible-console"></a>

**Run an Ansible Playbook from GitHub**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *GitHub*\. 
   + In the **Source Info** box, type the required information to access the source in the following format:

     ```
     {
         "owner": "owner_name",
         "repository": "repository_name",
         "branch": "branch_name",
         "path": "path_to_scripts_or_directory",
         "tokenInfo": "{{ssm-secure:SecureString_parameter_name}}"
     }
     ```

     This example downloads a file named `webserver.yml`\. 

     ```
     {
         "owner": "TestUser1",
         "repository": "GitHubPrivateTest",
         "branch": "myBranch",
         "path": "scripts/webserver.yml",
         "tokenInfo": "{{ssm-secure:mySecureStringParameter}}"
     }
     ```
**Note**  
`"branch"` is required only if your SSM document is stored in a branch other than `master`\.  
To use the version of your scripts that are in a particular *commit* in your repository, use `commitID` with `getOptions` instead of `branch`\. For example:  

     ```
     "getOptions": "commitID:bbc1ddb94...b76d3bEXAMPLE",
     ```
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     ansible-playbook -i “localhost,” --check -c local webserver.yml
     ```
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where are my instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

#### Run an Ansible Playbook from GitHub by using the AWS CLI<a name="integration-github-ansible-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to download and run a script from GitHub\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"repository_name\", \"path\": \"path_to_file_or_directory\", \"tokenInfo\":\"{{ssm-secure:name_of_your_SecureString_parameter}}\" }"],"commandLine":["commands_to_run"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-1234abcd" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"TestUser1\", \"repository\": \"GitHubPrivateTest\", \"path\": \"scripts/webserver.yml\", \"tokenInfo\":\"{{ssm-secure:mySecureStringParameter}}\" }"],"commandLine":["ansible-playbook -i “localhost,” --check -c local webserver.yml"]}'
   ```

### Run Python scripts from GitHub<a name="integration-github-python"></a>

This section includes procedures to help you run Python scripts from GitHub by using either the console or the AWS CLI\. 

#### Run a Python script from GitHub \(console\)<a name="integration-github-python-console"></a>

**Run a Python script from GitHub**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. For **Command parameters**, do the following:
   + In **Source Type**, select *GitHub*\. 
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

     For example, the following downloads a directory of scripts named *complex\-script*\.:

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

     ```
     "getOptions": "commitID:bbc1ddb94...b76d3bEXAMPLE",
     ```
   + For **Command Line**, type parameters for the script execution\. Here is an example\.

     ```
     mainFile.py argument-1 argument-2
     ```

     This example runs *mainFile\.py*, which can then run other scripts in the *complex\-script* directory\.
   + \(Optional\) For **Working Directory**, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) For **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where are my instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

#### Run a Python script from GitHub by using the AWS CLI<a name="integration-github-python-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to download and run a script from GitHub\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\":\"repository_name\", \"path\": \"path_to_script_or_directory"}"],"commandLine":["commands_to_run"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-abcd1234" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"TestUser1\", \"repository\":\"GitHubTestPublic\", \"path\": \"scripts/python/complex-script\"}"],"commandLine":["mainFile.py argument-1 argument-2 "]}'
   ```

   This example downloads a directory of scripts name complex\-script\. The `commandLine` entry runs mainFile\.py, which can then run other scripts in the complex\-script directory\.

## Running scripts from Amazon S3<a name="integration-s3"></a>

This section describes how to download and run scripts from Amazon S3\. You can run different types of scripts, including Ansible Playbooks, Python, Ruby, Shell, and PowerShell\. 

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts \(as long as the referenced scripts are included in the directory\)\.

Note the following important details about running scripts from Amazon S3\.
+ Systems Manager does not check to see if your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ Verify that your AWS Identity and Access Management \(IAM\) user account, role, or group has permission to read from the S3 bucket\.
+ Ensure that the instance profile on your EC2 instances has `s3:ListBucket` and `s3:GetObject` permissions\. If the instance profile doesn't have these permissions, the system fails to download your script from the S3 bucket\. For more information, see [Using instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the *IAM User Guide*\. 

**Topics**
+ [Run Ruby scripts from Amazon S3](#integration-s3-ruby)
+ [Run shell scripts from Amazon S3](#integration-s3-shell)
+ [Run a PowerShell script from Amazon S3](#integration-S3-PowerShell)

### Run Ruby scripts from Amazon S3<a name="integration-s3-ruby"></a>

This section includes procedures to help you run Ruby scripts from Amazon S3 by using either the Systems Manager console or the AWS CLI\.

#### Run a Ruby script from Amazon S3 \(console\)<a name="integration-s3-ruby-console"></a>

**Run a Ruby script from Amazon S3**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *S3*\. 
   + In the **Source Info** text box, type the required information to access the source in the following format:

     ```
     {"path":"https://s3.amazonaws.com/path_to_script"}
     ```

     For example:

     ```
     {"path":"https://s3.amazonaws.com/rubytest/scripts/ruby/helloWorld.rb"}
     ```
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     helloWorld.rb argument-1 argument-2
     ```
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where are my instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

#### Run a Ruby script from Amazon S3 by using the AWS CLI<a name="integration-s3-ruby-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Depending on the operating system type on your local machine, run one of the following commands to download and run a script from Amazon S3 \(the Windows version includes the escape characters \("/"\) you need to run the command from your command line tool\):

   **Windows** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/path_to_script\"}',"commandLine"="script_name_and_arguments"
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/RubyTest/scripts/ruby/helloWorld.rb\"}',"commandLine"="helloWorld.rb argument-1 argument-2"
   ```

   **Linux** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/RubyTest/scripts/ruby/helloWorld.rb\"}"],"commandLine":["helloWorld.rb argument-1 argument-2"]}'
   ```

### Run shell scripts from Amazon S3<a name="integration-s3-shell"></a>

This section includes procedures to help you run Shell scripts from Amazon S3 by using either the Systems Manager console or the AWS CLI\.

#### Run a shell script from Amazon S3 \(console\)<a name="integration-s3-ruby-console"></a>

**Run a shell script from Amazon S3**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *S3*\. 
   + In the **Source Info** text box, type the required information to access the source in the following format:

     ```
     {"path":"https://s3.amazonaws.com/path_to_script"}
     ```

     For example:

     ```
     {"path":"https://s3.amazonaws.com/shelltest/scripts/shell/helloWorld.sh"}
     ```
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     helloWorld.sh argument-1 argument-2
     ```
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where are my instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

#### Run a shell script from Amazon S3 by using the AWS CLI<a name="integration-s3-shell-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Depending on the operating system type on your local machine, run one of the following commands to download and run a script from Amazon S3 \(the Windows version includes the escape characters \("\\"\) you need to run the command from your command line tool\):

   **Windows** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/path_to_script\"}',"commandLine"="script_name_and_arguments"
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/ShellTest/scripts/shell/helloWorld.sh\"}',"commandLine"="helloWorld.sh argument-1 argument-2"
   ```

   **Linux** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/ShellTest/scripts/shell/helloWorld.sh\"}"],"commandLine":["helloWorld.sh argument-1 argument-2"]}'
   ```

### Run a PowerShell script from Amazon S3<a name="integration-S3-PowerShell"></a>

This section includes procedures to help you run PowerShell scripts from Amazon S3 by using either the EC2 console or the AWS CLI\.

#### Run a PowerShell script from Amazon S3 \(console\)<a name="integration-S3-PowerShell-console"></a>

**Run a PowerShell script from Amazon S3**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *S3*\. 
   + In the **Source Info** text box, type the required information to access the source in the following format:

     ```
     {"path": "https://s3.amazonaws.com/path_to_script"}
     ```

     For example:

     ```
     {"path": "https://s3.amazonaws.com/PowerShellTest/powershell/helloPowershell.ps1"}
     ```
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     helloPowershell.ps1 argument-1 argument-2
     ```
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where are my instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

#### Run a PowerShell script from S3 by using the AWS CLI<a name="integration-s3-powershell-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Depending on the operating system type on your local machine, run one of the following commands to download and run a script from Amazon S3 \(the Windows version includes the escape characters \("\\"\) you need to run the command from your command line tool\):

   **Windows** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/path_to_script\"}',"commandLine"="script_name_and_arguments"
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters "sourceType"="S3",sourceInfo='{\"path\":\"https://s3.amazonaws.com/PowerShellTest/scripts/powershell/helloWorld.ps1\"}',"commandLine"="helloWorld.ps1 argument-1 argument-2"
   ```

   **Linux** local machine:

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=instance-IDs" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --targets "Key=instanceids,Values=i-1234567890abcdef0" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/PowerShellTest/scripts/powershell/helloWorld.ps1\"}"],"commandLine":["helloWorld.ps1 argument-1 argument-2"]}'
   ```