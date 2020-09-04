# Running scripts from GitHub<a name="integration-remote-scripts"></a>

This topic describes how to use the `AWS-RunRemoteScript` pre\-defined SSM document to download scripts from GitHub, including Ansible Playbooks, Python, Ruby, and PowerShell scripts\. By using this document, you no longer need to manually port scripts into Amazon EC2 or wrap them in SSM documents\. Systems Manager integration with GitHub promotes *infrastructure as code*, which reduces the time it takes to manage instances while standardizing configurations across your fleet\. 

You can also create custom SSM documents that enable you to download and run scripts or other SSM documents from remote locations\. For more information, see [Creating composite documents](composite-docs.md)\.

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts that are included in the directory\. 

Note the following important details about running scripts from GitHub\.
+ Systems Manager does not verify that your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ You are responsible for ensuring that all GitHub requirements are met\. This includes refreshing your access token, as needed\. You must also ensure that you don't surpass the number of authenticated or unauthenticated requests\. For more information, see the GitHub documentation\.

**Topics**
+ [Run Ansible Playbooks from GitHub](integration-github-ansible.md)
+ [Run Python scripts from GitHub](integration-github-python.md)