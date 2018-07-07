# Running Scripts from GitHub and Amazon S3<a name="integration-remote-scripts"></a>

This section describes how to use the `AWS-RunRemoteScript` pre\-defined SSM document to download scripts from GitHub and Amazon S3, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. By using this document, you no longer need to manually port scripts into Amazon EC2 or wrap them in SSM documents\. Systems Manager integration with GitHub and Amazon S3 promotes *infrastructure as code*, which reduces the time it takes to manage instances while standardizing configurations across your fleet\. 

You can also create custom SSM documents that enable you to download and run scripts or other SSM documents from remote locations\. For more information, see [Creating Composite Documents](composite-docs.md)\.

**Topics**
+ [Running Scripts from GitHub](#integration-github)
+ [Running Scripts from Amazon S3](#integration-s3)

## Running Scripts from GitHub<a name="integration-github"></a>

This section describes how to download and run scripts from a private or public GitHub repository\. You can run different types of scripts, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. 

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts \(as long as the referenced scripts are included in the directory\)\.

Note the following important details about running scripts from GitHub\.
+ Systems Manager does not check to see if your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ You are responsible for ensuring that all GitHub requirements are met\. This includes refreshing your access token, as needed\. You must also ensure that you don't surpass the number of authenticated or unauthenticated requests\. For more information, see the GitHub documentation\.

**Topics**
+ [Run Ansible Playbooks from GitHub](#integration-github-ansible)
+ [Run Python Scripts from GitHub](#integration-github-python)

### Run Ansible Playbooks from GitHub<a name="integration-github-ansible"></a>

This section includes procedures to help you run Ansible Playbooks from GitHub by using either the console or the AWS CLI\.

**Before You Begin**  
If you plan to run a script that is stored in a private GitHub repository, then you must create a Systems Manager `SecureString` parameter for your GitHub security access token\. You can't access a script in a private GitHub repository by manually passing your token over SSH\. The access token must be passed as a Systems Manager `SecureString` parameter\. For more information about creating a `SecureString` parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

#### Run an Ansible Playbook from GitHub \(Console\)<a name="integration-github-ansible-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Run an Ansible Playbook from GitHub \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *GitHub*\. 
   + In the **Source Info** box, type the required information to access the source in the following format:

     ```
     {"owner":"owner_name", "repository": "repository_name", "path": "path_to_scripts_or_directory", "tokenInfo":"{{ssm-secure:SecureString_parameter_name}}" }
     ```

     For example:

     ```
     {"owner":"TestUser1", "repository": "GitHubPrivateTest", "path": "scripts/webserver.yml", "tokenInfo":"{{ssm-secure:mySecureStringParameter}}" }
     ```

     This example downloads a directory of scripts named *complex\-script*\. 
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     ansible-playbook -i “localhost,” --check -c local webserver.yml
     ```
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

**Run an Ansible Playbook from GitHub \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run a command**\.

1. In the **Document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Select Targets by** section, choose an option and select the instances where you want to download and run the script\.

1. \(Optional\) In the **Execute on** field, specify a number of **Targets** that can run the AWS\-RunRemoteScript document concurrently \(for example, 10\)\. Or, specify a percentage of the number of targets that can run the document concurrently \(for example, 10%\)\.
**Note**  
If you selected targets by choosing EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document by specifying a percentage\.

1. \(Optional\) In the **Stop after** field, specify the maximum number of errors allowed before the system stops sending the command to other instances\. For example, if you specify 3, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Source Type** list, choose **GitHub**

1. In the **Source** text box, type the required information to access the source in the following format:

   ```
   {"owner":"owner_name", "repository": "repository_name", "path": "path_to_scripts_or_directory", "tokenInfo":"{{ssm-secure:SecureString_parameter_name}}" }
   ```

   For example:

   ```
   {"owner":"TestUser1", "repository": "GitHubPrivateTest", "path": "scripts/webserver.yml", "tokenInfo":"{{ssm-secure:mySecureStringParameter}}" }
   ```

1. In the **Command Line** field, type parameters for the script execution\. Here is an example\.

   ```
   ansible-playbook -i “localhost,” --check -c local webserver.yml
   ```

1. In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.

1. In the **Comments** field, type information about this command\.

1. In the **Advanced Options** section, choose **Write to S3** to store command output in an Amazon S3 bucket\. Type the bucket and prefix names in the text boxes\.

1. Choose **Enable SNS notifications** to receive notifications and status about the command execution\. For more information about configuring SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

#### Run an Ansible Playbook from GitHub by Using the AWS CLI<a name="integration-github-ansible-cli"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to download and run a script from GitHub\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"repository_name\", \"path\": \"path_to_file_or_directory\", \"tokenInfo\":\"{{ssm-secure:name_of_your_SecureString_parameter}}\" }"],"commandLine":["commands_to_run"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-1234abcd" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"TestUser1\", \"repository\": \"GitHubPrivateTest\", \"path\": \"scripts/webserver.yml\", \"tokenInfo\":\"{{ssm-secure:mySecureStringParameter}}\" }"],"commandLine":["ansible-playbook -i “localhost,” --check -c local webserver.yml"]}'
   ```

### Run Python Scripts from GitHub<a name="integration-github-python"></a>

This section includes procedures to help you run Python scripts from GitHub by using either the console or the AWS CLI\. 

#### Run a Python Script from GitHub \(Console\)<a name="integration-github-python-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Run a Python Script from GitHub \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In **Command parameters**, do the following:
   + In **Source Type**, select *GitHub*\. 
   + In the **Source Info** text box, type the required information to access the source in the following format:

     ```
     {"owner":"owner_name", "repository": "repository_name", "path": "path_to_scripts_or_directory", "tokenInfo":"{{ssm-secure:SecureString_parameter_name}}" }
     ```

     For example:

     ```
     {"owner":"TestUser1", "repository":"GitHubPrivateTest", "path": "scripts/python/complex-script","tokenInfo":"{{ssm-secure:mySecureStringParameter}}"}
     ```

     This example downloads a directory of scripts named *complex\-script*\.
   + In the **Command Line** field, type parameters for the script execution\. Here is an example\.

     ```
     mainFile.py argument-1 argument-2
     ```

     This example runs *mainFile\.py*, which can then run other scripts in the *complex\-script* directory\.
   + \(Optional\) In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.
   + \(Optional\) In **Execution Timeout**, specify the number of seconds for the system to wait before failing the script command execution\. 

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

**Run a Python Script from GitHub \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run a command**\.

1. In the **Document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Select Targets by** section, choose an option and select the instances where you want to download and run the script\.

1. \(Optional\) In the **Execute on** field, specify a number of **Targets** that can run the AWS\-RunRemoteScript document concurrently \(for example, 10\)\. Or, specify a percentage of the number of targets that can run the document concurrently \(for example, 10%\)\.
**Note**  
If you selected targets by choosing EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document by specifying a percentage\.

1. \(Optional\) In the **Stop after** field, specify the maximum number of errors allowed before the system stops sending the command to other instances\. For example, if you specify 3, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Source Type** list, choose **GitHub**

1. In the **Source** text box, type the required information to access the source in the following format:

   ```
   {"owner":"owner_name", "repository": "repository_name", "path": "path_to_scripts_or_directory", "tokenInfo":"{{ssm-secure:SecureString_parameter_name}}" }
   ```

   For example:

   ```
   {"owner":"TestUser1", "repository":"GitHubPrivateTest", "path": "scripts/python/complex-script","tokenInfo":"{{ssm-secure:mySecureStringParameter}}"}
   ```

   This example downloads a directory of scripts named complex\-script\.

1. In the **Command Line** field, type parameters for the script execution\. Here is an example\.

   ```
   mainFile.py argument-1 argument-2
   ```

   This example runs mainFile\.py, which can then run other scripts in the complex\-script directory\.

1. In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.

1. In the **Comments** field, type information about this command\.

1. In the **Advanced Options** section, choose **Write to S3** to store command output in an Amazon S3 bucket\. Type the bucket and prefix names in the text boxes\.

1. Choose **Enable SNS notifications** to receive notifications and status about the command execution\. For more information about configuring SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

#### Run a Python Script from GitHub by Using the AWS CLI<a name="integration-github-python-cli"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to download and run a script from GitHub\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\":\"repository_name\", \"path\": \"path_to_script_or_directory"}"],"commandLine":["commands_to_run"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-abcd1234" --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"TestUser1\", \"repository\":\"GitHubTestPublic\", \"path\": \"scripts/python/complex-script\"}"],"commandLine":["mainFile.py argument-1 argument-2 "]}'
   ```

   This example downloads a directory of scripts name complex\-script\. The `commandLine` entry runs mainFile\.py, which can then run other scripts in the complex\-script directory\.

## Running Scripts from Amazon S3<a name="integration-s3"></a>

This section describes how to download and run scripts from Amazon S3\. You can run different types of scripts, including Ansible Playbooks, Python, Ruby, and PowerShell\. 

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts \(as long as the referenced scripts are included in the directory\)\.

Note the following important details about running scripts from Amazon S3\.
+ Systems Manager does not check to see if your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ Verify that your AWS Identity and Access Management \(IAM\) user account, role, or group has permission to read from the S3 bucket\.

**Topics**
+ [Run Ruby Scripts from Amazon S3](#integration-s3-ruby)
+ [Run PowerShell Script from Amazon S3](#integration-S3-PowerShell)

### Run Ruby Scripts from Amazon S3<a name="integration-s3-ruby"></a>

This section includes procedures to help you run Ruby scripts from Amazon S3 by using either the EC2 console or the AWS CLI\.

#### Run a Ruby Script from Amazon S3 \(Console\)<a name="integration-s3-ruby-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Run a Ruby Script from Amazon S3 \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

**Run a Ruby Script from Amazon S3 \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run a command**\.

1. In the **Document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Select Targets by** section, choose an option and select the instances where you want to download and run the script\.

1. \(Optional\) In the **Execute on** field, specify a number of **Targets** that can run the AWS\-RunRemoteScript document concurrently \(for example, 10\)\. Or, specify a percentage of the number of targets that can run the document concurrently \(for example, 10%\)\.
**Note**  
If you selected targets by choosing EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document by specifying a percentage\.

1. \(Optional\) In the **Stop after** field, specify the maximum number of errors allowed before the system stops sending the command to other instances\. For example, if you specify 3, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Source Type** list, choose **S3**

1. In the **Source** text box, type the required information to access the source in the following format:

   ```
   {"path":"https://s3.amazonaws.com/path_to_script"}
   ```

   For example:

   ```
   {"path":"https://s3.amazonaws.com/rubytest/scripts/ruby/helloWorld.rb"}
   ```

1. In the **Command Line** field, type parameters for the script execution\. Here is an example\.

   ```
   helloWorld.rb argument-1 argument-2
   ```

1. In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.

1. In the **Comments** field, type information about this command\.

1. In the **Advanced Options** section, choose **Write to S3** to store command output in an Amazon S3 bucket\. Type the bucket and prefix names in the text boxes\.

1. Choose **Enable SNS notifications** to receive notifications and status about the command execution\. For more information about configuring SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

#### Run a Ruby Script from S3 by using the AWS CLI<a name="integration-s3-ruby-cli"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to download and run a script from Amazon S3\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters'{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}' 
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-abcd1234" --parameters'{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/RubyTest/scripts/ruby/helloWorld.rb\"}"],"commandLine":["helloWorld.rb argument-1 argument-2"]}' 
   ```

### Run PowerShell Script from Amazon S3<a name="integration-S3-PowerShell"></a>

This section includes procedures to help you run PowerShell scripts from Amazon S3 by using either the EC2 console or the AWS CLI\.

#### Run a PowerShell Script from Amazon S3 \(Console\)<a name="integration-S3-PowerShell-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Run a PowerShell Script from Amazon S3 \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

**Run a PowerShell Script from Amazon S3 \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run a command**\.

1. In the **Document** list, choose **AWS\-RunRemoteScript**\.

1. In the **Select Targets by** section, choose an option and select the instances where you want to download and run the script\.

1. \(Optional\) In the **Execute on** field, specify a number of **Targets** that can run the AWS\-RunRemoteScript document concurrently \(for example, 10\)\. Or, specify a percentage of the number of targets that can run the document concurrently \(for example, 10%\)\.
**Note**  
If you selected targets by choosing EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document by specifying a percentage\.

1. \(Optional\) In the **Stop after** field, specify the maximum number of errors allowed before the system stops sending the command to other instances\. For example, if you specify 3, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Source Type** list, choose **S3**

1. In the **Source** text box, type the required information to access the source in the following format:

   ```
   {"path": "https://s3.amazonaws.com/path_to_script"}
   ```

   For example:

   ```
   {"path": "https://s3.amazonaws.com/PowerShellTest/powershell/helloPowershell.ps1"}
   ```

1. In the **Command Line** field, type parameters for the script execution\. Here is an example\.

   ```
   helloPowershell.ps1 argument-1 argument-2
   ```

1. In the **Working Directory** field, type the name of a directory on the instance where you want to download and run the script\.

1. In the **Comments** field, type information about this command\.

1. In the **Advanced Options** section, choose **Write to S3** to store command output in an Amazon S3 bucket\. Type the bucket and prefix names in the text boxes\.

1. Choose **Enable SNS notifications** to receive notifications and status about the command execution\. For more information about configuring SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

#### Run a PowerShell Script from S3 by Using the AWS CLI<a name="integration-s3-powershell-cli"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to download and run a script from Amazon S3\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "instance-IDs" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\": \"https://s3.amazonaws.com/path_to_script\"}"],"commandLine":["script_name_and_arguments"]}' 
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunRemoteScript" --instance-ids "i-1234abcd" --parameters '{"sourceType":["S3"],"sourceInfo":["{\"path\": \"https://s3.amazonaws.com/TestPowershell/powershell/helloPowershell.ps1\"}"],"commandLine":["helloPowershell.ps1 argument-1 argument-2"]}' 
   ```