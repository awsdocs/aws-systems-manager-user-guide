# Setting Up Parameter Store<a name="sysman-paramstore-settingup"></a>

To set up parameters in Systems Manager Parameter Store, you first configure AWS Identity and Access Management \(IAM\) policies that provide users in your account with permission to perform the actions you specify\. 

This section includes information about how to manually configure these policies using the IAM console, and how to assign them to users and user groups\. You can also create and assign policies to control which parameter actions can be run on an instance\. 

This section also include information about how to create Amazon CloudWatch Events rules that let you receive notifications about changes to Systems Manager parameters\. You can also use CloudWatch Events rules to trigger other actions in AWS based on changes in Parameter Store\.

**Topics**
+ [Control Access to Systems Manager Parameters](sysman-paramstore-access.md)
+ [Set Up Notifications or Trigger Actions Based on Parameter Store Events](sysman-paramstore-cwe.md)