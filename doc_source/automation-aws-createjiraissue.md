# AWS\-CreateJiraIssue<a name="automation-aws-createjiraissue"></a>

**Description**

Create an issue in Jira\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateJiraIssue)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AssigneeName

  Type: String

  Description: \(Optional\) The username of the person the issue should be assigned to\.
+ DueDate

  Type: String

  Description: \(Optional\) The due date for the issue in yyyy\-mm\-dd format\.
+ IssueDescription

  Type: String

  Description: \(Required\) A detailed description of the issue\.
+ IssueSummary

  Type: String

  Description: \(Required\) A brief summary of the issue\.
+ IssueTypeName

  Type: String

  Description: \(Required\) The name of the type of issue you want to create \(for example, Task, Sub\-task, Bug, etc\.\)\.
+ JiraURL

  Type: String

  Description: \(Required\) The url of the Jira instance\.
+ JiraUsername

  Type: String

  Description: \(Required\) The name of the user the issue will be created with\.
+ PriorityName

  Type: String

  Description: \(Optional\) The name of the priority of the issue\.
+ ProjectKey

  Type: String

  Description: \(Required\) The key of the project the issue should be created in\.
+ SSMParameterName

  Type: String

  Description: \(Required\) The name of an encrypted SSM Parameter containing the API key or password for the Jira user\.

**Document Steps**

aws:createStack \- Create CloudFormation stack to create Lambda IAM role and function\.

aws:invokeLambdaFunction \- Invoke Lambda function to create the Jira issue

aws:deleteStack \- Delete the CloudFormation stack created\.

**Outputs**

IssueId: ID of the newly created Jira issue