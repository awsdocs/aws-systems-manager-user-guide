# Create an Association<a name="sysman-state-assoc"></a>

The following procedures describe how to create a State Manager association by using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for Windows PowerShell\.

**Important**  
The following procedures are intended for creating an association with a `Command` or `Policy` document\. For information on creating an association that uses an `Automation` document, see [Running Automation Workflows with Triggers Using State Manager](automation-sm-target.md)\.

When a State Manager association is created, the association immediately runs on the specified instances or targets\. After the initial execution, the association runs in intervals according to the schedule that you defined and according to the following rules:
+ Associations are only run on instances that are online when the interval starts\. Offline instances are skipped\.
+ State Manager attempts to run the association on all configured instances during an interval\.
+ If an association is not run during an interval \(because, for example, a concurrency value limited the number of instances that could process the association at one time\), then State Manager attempts to run the association during the next interval\.
+ State Manager records history for all skipped intervals\. You can view the history on the **Execution History** tab\.

## Create an Association \(Console\)<a name="sysman-state-assoc-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association\.

**To create a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\. A name helps you remember the purpose of the association\. For example, you could specify **Automatically\_update\_AWSPVDrivers\_on\_us\-west\-2\_instances** for an association with that purpose\. Spaces aren't allowed in the name\.

1. In the **Document** list, choose the option next to a document name\. You can use the numbers to the right of the Search bar to view more documents\.

1. For **Parameters**, specify the required input parameters\.

1. For **Targets**, choose an option\. For information about using targets, see [Using Targets and Rate Controls with State Manager Associations](systems-manager-state-manager-targets-and-rate-controls.md)\.
**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, then use the buttons provided to create a cron or rate schedule for the association\. 

1. In the **Advanced options** section:
   + In **Compliance severity**, choose a severity level for the association\. Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you indicate here\. For more information, see [About State Manager Association Compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, configure options to run State Manager associations across a fleet of managed instances\. For information about using rate controls, see [Using Targets and Rate Controls with State Manager Associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. In the **Output options** section, choose **Enable writing output to S3** if you want to write the output of the command to create the associations to an Amazon S3 bucket\.

1. Choose **Create Association**\.

## Create an Association \(Command Line\)<a name="sysman-state-assoc-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create an association\.

**To create a State Manager association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md) or [Install or Upgrade the AWS Tools for PowerShell](getting-started-ps.md)\.

1. Use the following format to create a command that creates a State Manager association\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --targets Key=tag:TagKey,Values=TagValue \
     --name document_name \
     --schedule "cron_or_rate_expression" \
     --parameters (if any)
   ```

**Note**  
If you create an association by using the AWS CLI, use the `--Targets` parameter to target instances for the association\. Don't use the `--InstanceID` parameter\. The `--InstanceID` parameter is a legacy parameter\. 

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --targets Key=tag:TagKey,Values=TagValue ^
     --name document_name ^
     --schedule "cron_or_rate_expression" ^
     --parameters (if any)
   ```

**Note**  
If you create an association by using the AWS CLI, use the `--Targets` parameter to target instances for the association\. Don't use the `--InstanceID` parameter\. The `--InstanceID` parameter is a legacy parameter\. 

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName document_name `
     -Target Targets `
     -ScheduleExpression "cron_or_rate_expression" `
     -Parameters (if any)
   ```

**Note**  
If you create an association by using AWS Tools for Windows PowerShell, use the `-Target` parameter to target instances for the association\. Don't use the `-InstanceID` parameter\. The `-InstanceID` parameter is a legacy parameter\. 

------

   The following example creates an association on instances tagged with `"Environment,Linux"`\. The association uses the `AWS-UpdateSSMAgent` document to update SSM Agent on the targeted instances at 2:00 every Sunday morning\. For compliance reporting, this association is assigned a severity level of Medium\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --targets Key=tag:Environment,Values=Linux \
     --name AWS-UpdateSSMAgent  \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --targets Key=tag:Environment,Values=Linux ^
     --name AWS-UpdateSSMAgent  ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName Update_SSM_Agent_Linux `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="tag:Environment"
         "Values"="Linux"
       } `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -ComplianceSeverity MEDIUM
   ```

------

   The following example targets instance IDs by specifying a wildcard value \(\*\)\. This enables Systems Manager to create an association on *all* instances in the current account and AWS Region\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --name "AWS-UpdateSSMAgent" \
     --targets "Key=instanceids,Values=*" \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --name "AWS-UpdateSSMAgent" ^
     --targets "Key=instanceids,Values=*" ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName Update_SSM_Agent_All `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="InstanceIds"
         "Values"="*"
       } `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -ComplianceSeverity MEDIUM
   ```

------
**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 