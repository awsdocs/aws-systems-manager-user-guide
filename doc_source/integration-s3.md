# Running scripts from Amazon S3<a name="integration-s3"></a>

This section describes how to download and run scripts from Amazon S3\. You can run different types of scripts, including Ansible Playbooks, Python, Ruby, Shell, and PowerShell\. 

You can also download a directory that includes multiple scripts\. When you run the primary script in the directory, Systems Manager also runs any referenced scripts that are included in the directory\. 

Note the following important details about running scripts from Amazon S3\.
+ Systems Manager does not verify that your script is capable of running on an instance\. Before you download and run the script, you must verify that the required software is installed on the instance\. Or, you can create a composite document that installs the software by using either Run Command or State Manager, and then downloads and runs the script\.
+ Verify that your AWS Identity and Access Management \(IAM\) user account, role, or group has permission to read from the S3 bucket\.
+ Ensure that the instance profile on your EC2 instances has `s3:ListBucket` and `s3:GetObject` permissions\. If the instance profile doesn't have these permissions, the system fails to download your script from the S3 bucket\. For more information, see [Using instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the *IAM User Guide*\. 

**Topics**
+ [Run Ruby scripts from Amazon S3](integration-s3-ruby.md)
+ [Run shell scripts from Amazon S3](integration-s3-shell.md)
+ [Run a PowerShell script from Amazon S3](integration-S3-PowerShell.md)