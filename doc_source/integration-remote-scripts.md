# Running scripts from GitHub<a name="integration-remote-scripts"></a>

This topic describes how to use the pre\-defined Systems Manager document \(SSM document\) `AWS-RunRemoteScript` to download scripts from GitHub, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. By using this SSM document, you no longer need to manually port scripts into Amazon Elastic Compute Cloud \(Amazon EC2\) or wrap them in SSM documents\. AWS Systems Manager integration with GitHub promotes *infrastructure as code*, which reduces the time it takes to manage nodes while standardizing configurations across your fleet\. 

You can also create custom SSM documents that allow you to download and run scripts or other SSM documents from remote locations\. For more information, see [Creating composite documents](documents-creating-content.md#documents-creating-composite)\.

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts that are included in the directory\. 

Note the following important details about running scripts from GitHub\.
+ Systems Manager doesn't verify that your script is capable of running on a node\. Before you download and run the script, verify that the required software is installed on the node\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, capabilities of AWS Systems Manager, and then downloads and runs the script\.
+ You're responsible for ensuring that all GitHub requirements are met\. This includes refreshing your access token, as needed\. Ensure that you don't surpass the number of authenticated or unauthenticated requests\. For more information, see the GitHub documentation\.
+ GitHub Enterprise repositories are not supported\.

**Topics**
+ [Run Ansible Playbooks from GitHub](integration-github-ansible.md)
+ [Run Python scripts from GitHub](integration-github-python.md)