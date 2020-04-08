# Getting started with Change Calendar<a name="systems-manager-change-calendar-prereqs"></a>

Complete the following before using Change Calendar\.

## Install latest command line tools<a name="change-calendar-prereqs-tools"></a>

Install the latest command line tools to get state information about calendars\.


| Requirement | Description | 
| --- | --- | 
|  AWS CLI  |  \(Optional\) To use the AWS CLI to get state information about calendars, install the newest release of the AWS CLI on your local computer\. For more information about how to install or upgrade the CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.  | 
|  AWS Tools for PowerShell  |  \(Optional\) To use the Tools for PowerShell to get state information about calendars, install the newest release of Tools for PowerShell on your local computer\. For more information about how to install or upgrade the Tools for PowerShell, see [Setting Up the AWS Tools for Windows PowerShell or AWS Tools for PowerShell Core](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for PowerShell User Guide*\.  | 

## Set up permissions<a name="change-calendar-prereqs-permissions"></a>

To create, update, or delete a Change Calendar entry, including adding and removing events from the entry, a policy attached to your AWS Identity and Access Management \(IAM\) user or service role must allow the following actions\.
+ `ssm:CreateDocument`
+ `ssm:DescribeDocument`
+ `ssm:UpdateDocument`
+ `ssm:DeleteDocument`

To get information about the current or upcoming state of the calendar, a policy attached to your AWS Identity and Access Management \(IAM\) user or service role must allow the following action\.
+ `ssm:GetCalendarState`

If your IAM user account, group, or role is assigned administrator permissions, then you have access to Change Calendar\. If you don't have administrator permissions, then an administrator must give you permission by assigning the `AmazonSSMFullAccess` managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

Change Calendar entries that are owned by \(that is, created by\) accounts other than yours are read\-only, even if they are shared with your account\.