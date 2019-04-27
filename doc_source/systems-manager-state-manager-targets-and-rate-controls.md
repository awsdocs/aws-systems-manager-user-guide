# Using Targets and Rate Controls with State Manager Associations<a name="systems-manager-state-manager-targets-and-rate-controls"></a>

AWS Systems Manager enables you to create State Manager associations on a fleet of managed instances by using targets\. Additionally, you can control the execution of these associations across your fleet by specifying a concurrency value and an error threshold\. The concurrency value specifies how many resources are allowed to run the association simultaneously\. An error threshold specifies how many association executions are allowed to fail before Systems Manager sends a command to each instance configured with that association to stop running the association until the next scheduled execution\. The concurrency and error threshold features are collectively called *rate controls*\. 

**Concurrency**  
Concurrency helps to limit the impact on your fleet by allowing you to specify that only a certain number of instances can process an association at one time\. You can specify either an absolute number of instances, for example 20, or a percentage of the target set of instances, for example 10%\.

State Manager concurrency has the following restrictions and limitations:
+ If you choose to create an association by using targets, but you don't specify a concurrency value, then State Manager automatically enforces a maximum concurrency of 50 instances\.
+ If new instances that match the target criteria come online while an association that uses concurrency is running, then the new instances run the association if the concurrency value is not exceeded\. If the concurrency value is exceeded, then they are ignored during the current association execution interval\. They will run the association during the next scheduled interval while conforming to the concurrency requirements\.
+ If you update an association that uses concurrency, and one or more instances are processing that association when it is updated, then any instance that is running the association is allowed to be completed\. Those associations that haven't started are aborted\. After running associations are completed, all target instances immediately run the association again because it was updated\. When the association runs again, the concurrency value is enforced\. 

**Error Thresholds**  
An error threshold specifies how many association executions are allowed to fail before Systems Manager sends a command to each instance configured with that association to stop running the association until the next scheduled execution\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\.

If you specify an absolute number of three errors, for example, State Manager sends the stop command when the fourth error is received\. If you specify 0, then State Manager sends the stop command after the first error result is returned\.

If you specify an error threshold of 10% for 50 associations, then State Manager sends the stop command when the sixth error is received\. Associations that are already running when an error threshold is reached are allowed to be completed, but some of these associations might fail as well\. If you need to ensure that there won’t be more errors than the number specified for the error threshold, then set the **Concurrency** value to 1 so that associations proceed one at a time\. 

State Manager error thresholds have the following restrictions and limitations:
+ Error thresholds are enforced for the current interval\.
+ Information about each error, including step\-level details, are recorded in the association history\.
+ If you choose to create an association by using targets, but you don't specify an error threshold, then State Manager automatically enforces a threshold of 100% failures\.

## Create an Association that Uses Targets and Rate Controls \(CLI\)<a name="sysman-state-targets"></a>

You can create associations on tens, hundreds, or thousands of instances by using the `targets` parameter\. The `targets` parameter accepts a `Key,Value` combination based on Amazon EC2 tags that you specified for your instances\. When you run the request to create the association, the system locates and attempts to create the association on all instances that match the specified criteria\. After the association is created and assigned to the instance or to a target set of instances, then State Manager immediately runs the association\.

**Note**  
When you create an association, you specify when the schedule runs\. You must specify the schedule by using a cron or rate expression\. There are many tools on the internet to help you create these expressions\. For more information about cron and rate expressions, see [Cron and Rate Expressions for Associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.

Use the following format to create an AWS CLI command that uses targets to create a State Manager association\. 

```
aws ssm create-association --targets Key=tag:TagKey,Values=TagValue --name document_name --schedule "cron_or_rate_expression" --parameters (if any) --max-concurrency (Optional) a_number_of_instances_or_a_percentage_of_target_set --max-errors (Optional) a_number_of_errors_or_a_percentage_of_target_set
```

The following example creates an association on instances tagged with "Environment,Linux"\. The association uses the AWS\-UpdateSSMAgent document to updates SSM Agent on the targeted instances at 2:00 every Sunday morning\. For compliance reporting, this association is assigned a severity level of Medium\.

```
aws ssm create-association --association-name Update_SSM_Agent_Linux --targets Key=tag:Environment,Values=Linux --name AWS-UpdateSSMAgent  --compliance-severity "MEDIUM" --schedule "cron(0 2 ? * SUN *)"
```

**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is dissociated from the State Manager document\. 