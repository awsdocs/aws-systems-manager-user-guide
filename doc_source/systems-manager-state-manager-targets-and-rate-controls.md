# About targets and rate controls in State Manager associations<a name="systems-manager-state-manager-targets-and-rate-controls"></a>

This topic describes State Manager features that help you deploy an association to dozens or hundreds of instances while controlling how many instances run the association at the scheduled time\.

## Targets<a name="systems-manager-state-manager-targets-and-rate-controls-about-targets"></a>

When you create a State Manager association, you choose which instances to configure with the association in the **Targets** section of the Systems Manager console, as shown here\.

![\[Different options for targeting instances when creating a State Manager association\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-targets.png)

If you create an association by using a command line tool such as the AWS CLI, then you specify the `targets` parameter\. Targeting instances lets you configure tens, hundreds, or thousands of instances with an association without having to specify or choose individual instance IDs\. 

**Note**  
You can only target Systems Manager managed instances\. This means you must set up and configure your instances for Systems Manager before you create an association\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\. 

State Manager includes the following target options when creating an association\.

**Specify instance tags**  
Use this option to specify a tag key and \(optionally\) a tag value that are currently assigned to your instances\. When you run the request, the system locates and attempts to create the association on all instances that match the specified tag key and value\. When the system initially creates the association, it runs the association\. After this initial run, the system runs the association according to the schedule you specified\.

If you create new instances and assign the specified tag key and value to those instances, the system automatically applies the association, runs it immediately, and then runs it according to the schedule\. If you delete the specified tags from an instance, the system no longer runs the association on those instances\.

For information about assigning tags to your instances, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide*\.

**Choose instances manually**  
Use this option to manually select the instances where you want to create the association\. The **Instances** pane displays all Systems Manager managed instances in the current AWS account and Region\. You can manually select as many instances as you want\. If you don't see instances in the list for which you want to create an association, verify that the instances are running and are configured for Systems Manager\. When the system initially creates the association, it runs the association\. After this initial run, the system runs the association according to the schedule you specified\.

**Choose a resource group**  
Use this option to create an association on all instances returned by an AWS Resource Groups tag\-based or AWS CloudFormation stack\-based query\. 

Note the following details about targeting resource groups for an association\.
+ If you add new instances to a group, the system automatically maps the instances to the association that targets the resource group\. The system applies the association to the instances when it discovers the change\. After this initial run, the system runs the association according to the schedule you specified\.
+ If you delete a resource group, all instances in that group no longer run the association\. As a best practice, you should delete associations targeting the group\.
+ You can only target a single resource group for an association\. Multiple or nested groups are not supported\.
+ After you create an association, State Manager periodically updates the association with information about resources in the Resource Group\. If you add new resources to a Resource Group, the schedule for when the system applies the association to the new resources depends on several factors\. You can check the status of the association in the State Manager page of the Systems Manager console\.

**Warning**  
An AWS Identity and Access Management \(IAM\) user, group, or role with permission to create an association that targets a resource group of Amazon EC2 instances automatically has root\-level control of all instances in the group\. Only trusted administrators should be permitted to create associations\. 

For more information about Resource Groups, see [What Is AWS Resource Groups?](https://docs.aws.amazon.com/ARG/latest/userguide/) in the *AWS Resource Groups User Guide*\.

**Choose all instances**  
Use this option to target all instances in the current AWS account and Region\. When you run the request, the system locates and attempts to create the association on all instances in the current AWS account and Region\. When the system initially creates the association, it runs the association\. After this initial run, the system runs the association according to the schedule you specified\. If you create new instances, the system automatically applies the association, runs it immediately, and then runs it according to the schedule\.

## Rate controls<a name="systems-manager-state-manager-targets-and-rate-controls-about-controls"></a>

You can control the execution of an association on your instances by specifying a concurrency value and an error threshold\. The concurrency value specifies how many instances are allowed to run the association simultaneously\. An error threshold specifies how many association executions can fail before Systems Manager sends a command to each instance configured with that association to stop running the association\. The command stops the association from running until the next scheduled execution\. The concurrency and error threshold features are collectively called *rate controls*\. 

![\[Different rate control options when creating a State Manager association\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-rate-controls.png)

**Concurrency**  
Concurrency helps to limit the impact on your instances by allowing you to specify that only a certain number of instances can process an association at one time\. You can specify either an absolute number of instances, for example 20, or a percentage of the target set of instances, for example 10%\.

State Manager concurrency has the following restrictions and limitations:
+ If you choose to create an association by using targets, but you don't specify a concurrency value, then State Manager automatically enforces a maximum concurrency of 50 instances\.
+ If new instances that match the target criteria come online while an association that uses concurrency is running, then the new instances run the association if the concurrency value is not exceeded\. If the concurrency value is exceeded, then the instances are ignored during the current association execution interval\. The instances run the association during the next scheduled interval while conforming to the concurrency requirements\.
+ If you update an association that uses concurrency, and one or more instances are processing that association when it is updated, then any instance that is running the association is allowed to complete\. Those associations that haven't started are stopped\. After running associations complete, all target instances immediately run the association again because it was updated\. When the association runs again, the concurrency value is enforced\. 

**Error thresholds**  
An error threshold specifies how many association executions are allowed to fail before Systems Manager sends a command to each instance configured with that association\. The command stops the association from running until the next scheduled execution\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\.

If you specify an absolute number of three errors, for example, State Manager sends the stop command when the fourth error is returned\. If you specify 0, then State Manager sends the stop command after the first error result is returned\.

If you specify an error threshold of 10% for 50 associations, then State Manager sends the stop command when the sixth error is returned\. Associations that are already running when an error threshold is reached are allowed to complete, but some of these associations might fail\. To ensure that there aren't more errors than the number specified for the error threshold, set the **Concurrency** value to 1 so that associations proceed one at a time\. 

State Manager error thresholds have the following restrictions and limitations:
+ Error thresholds are enforced for the current interval\.
+ Information about each error, including step\-level details, is recorded in the association history\.
+ If you choose to create an association by using targets, but you don't specify an error threshold, then State Manager automatically enforces a threshold of 100% failures\.