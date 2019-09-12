# Using Targets and Rate Controls with State Manager Associations<a name="systems-manager-state-manager-targets-and-rate-controls"></a>

AWS Systems Manager enables you to create State Manager associations on a fleet of managed instances by using targets\. Additionally, you can control the execution of these associations across your fleet by specifying a concurrency value and an error threshold\. The concurrency value specifies how many resources are allowed to run the association simultaneously\. An error threshold specifies how many association executions can fail before Systems Manager sends a command to each instance configured with that association\. The command stops the association from running until the next scheduled execution\. The concurrency and error threshold features are collectively called *rate controls*\. 

**Concurrency**  
Concurrency helps to limit the impact on your fleet by allowing you to specify that only a certain number of instances can process an association at one time\. You can specify either an absolute number of instances, for example 20, or a percentage of the target set of instances, for example 10%\.

State Manager concurrency has the following restrictions and limitations:
+ If you choose to create an association by using targets, but you don't specify a concurrency value, then State Manager automatically enforces a maximum concurrency of 50 instances\.
+ If new instances that match the target criteria come online while an association that uses concurrency is running, then the new instances run the association if the concurrency value is not exceeded\. If the concurrency value is exceeded, then the instances are ignored during the current association execution interval\. The instances run the association during the next scheduled interval while conforming to the concurrency requirements\.
+ If you update an association that uses concurrency, and one or more instances are processing that association when it is updated, then any instance that is running the association is allowed to complete\. Those associations that haven't started are stopped\. After running associations complete, all target instances immediately run the association again because it was updated\. When the association runs again, the concurrency value is enforced\. 

**Error Thresholds**  
An error threshold specifies how many association executions are allowed to fail before Systems Manager sends a command to each instance configured with that association\. The command stops the association from running until the next scheduled execution\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\.

If you specify an absolute number of three errors, for example, State Manager sends the stop command when the fourth error is returned\. If you specify 0, then State Manager sends the stop command after the first error result is returned\.

If you specify an error threshold of 10% for 50 associations, then State Manager sends the stop command when the sixth error is returned\. Associations that are already running when an error threshold is reached are allowed to complete, but some of these associations might fail\. To ensure that there aren't more errors than the number specified for the error threshold, set the **Concurrency** value to 1 so that associations proceed one at a time\. 

State Manager error thresholds have the following restrictions and limitations:
+ Error thresholds are enforced for the current interval\.
+ Information about each error, including step\-level details, is recorded in the association history\.
+ If you choose to create an association by using targets, but you don't specify an error threshold, then State Manager automatically enforces a threshold of 100% failures\.

**Targets**  
You can create associations on tens, hundreds, or thousands of instances by using the `targets` parameter\. The `targets` parameter accepts a `Key,Value` combination based on resource tags that you specified for your instances\. When you run the request to create the association, the system locates and attempts to create the association on all instances that match the specified criteria\. After the association is created and assigned to the instance or to a target set of instances, then State Manager immediately runs the association\.

**Note**  
When you create an association, you specify when the schedule runs\. You must specify the schedule by using a cron or rate expression\. There are many tools on the internet to help you create these expressions\. For more information about cron and rate expressions, see [Cron and Rate Expressions for Associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.

## Create an Association That Uses Targets and Rate Controls \(Console\)<a name="sysman-state-targets-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association that uses targets and rate controls\.

**Important**  
The following procedure is intended for creating an association with a `Command` or `Policy` document\. For information on creating an association that uses an `Automation` document, see [Running Automation Workflows with Triggers Using State Manager](automation-sm-target.md)\.

**To create a State Manager association that uses targets and rate controls**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\. A name helps you remember the purpose of the association when you created it\. For example, you could specify **Automatically\_update\_AWSPVDrivers\_on\_us\-west\-2\_instances** for an association with that purpose\. Spaces aren't allowed in the name\.

1. In the **Document** list, choose the option next to a document name\. You can use the numbers to the right of the Search bar to view more documents\.

1. For **Parameters**, specify the required input parameters\.

1. In the **Targets** section, choose either **Selecting all managed instances in this region under this account** or **Specifying tags**\. If you choose to target tags, then enter a tag key and a tag value\.
**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, then use the buttons provided to create a cron or rate schedule for the association\. 

1. In the **Advanced options** section:
   + In **Compliance severity**, choose a severity level for the association\. Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you indicate here\. For more information, see [About State Manager Association Compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, configure options to run State Manager associations across a fleet of managed instances\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. In the **Output options** section, choose **Enable writing output to S3** if you want to write the output of the command to create the associations to an Amazon S3 bucket\.

1. Choose **Create Association**\.

## Create an Association That Uses Targets and Rate Controls \(Command Line\)<a name="sysman-state-targets-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a State Manager association that uses targets and rate controls\.

**To create an association with targets and rate controls**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md) or [Install or Upgrade the AWS Tools for PowerShell](getting-started-ps.md)\.

1. Use the following format to create a command that creates a State Manager association that uses targets and rate controls\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --targets Key=tag:TagKey,Values=TagValue \
     --name document_name \
     --schedule "cron_or_rate_expression" \
     --parameters (if any) \
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set \
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --targets Key=tag:TagKey,Values=TagValue ^
     --name document_name ^
     --schedule "cron_or_rate_expression" ^
     --parameters (if any) ^
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set ^
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName document_name `
     -Target Targets `
     -ScheduleExpression "cron_or_rate_expression" `
     -Parameters (if any) `
     -MaxConcurrency a_number_of_instances_or_a_percentage_of_target_set `
     -MaxError  a_number_of_errors_or_a_percentage_of_target_set
   ```

------

   The following example creates an association on instances tagged with `"Environment,Linux"`\. The association uses the AWS\-UpdateSSMAgent document to update SSM Agent on the targeted instances at 2:00 every Sunday morning\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --targets Key=tag:Environment,Values=Linux \
     --name AWS-UpdateSSMAgent  \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --targets Key=tag:Environment,Values=Linux ^
     --name AWS-UpdateSSMAgent  ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)" ^
     --max-errors "5" ^
     --max-concurrency "10"
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
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM
   ```

------

   The following example targets instance IDs by specifying a wildcard value \(\*\)\. This enables Systems Manager to create an association on *all* instances in the current account and AWS Region\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\.

------
#### [ Linux ]

   ```
   aws ssm create-association \ 
     --association-name Update_SSM_Agent_Linux \
     --name "AWS-UpdateSSMAgent" \
     --targets "Key=instanceids,Values=*" \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^ 
     --association-name Update_SSM_Agent_Linux ^
     --name "AWS-UpdateSSMAgent" ^
     --targets "Key=instanceids,Values=*" ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)" ^
     --max-errors "5" ^
     --max-concurrency "10"
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
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM
   ```

------
**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 