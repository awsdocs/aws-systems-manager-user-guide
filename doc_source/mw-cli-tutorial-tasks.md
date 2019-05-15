# Step 3: Register Tasks with the Maintenance Window<a name="mw-cli-tutorial-tasks"></a>

In this step, you use the AWS CLI to register one of the four types of tasks that are supported by maintenance windows:
+ Systems Manager Run Command commands
+ Systems Manager Automation workflows
+ AWS Lambda functions
+ AWS Step Functions tasks

If you want to practice registering more than one type of task, we recommend repeating this tutorial for each type\.

## Before You Begin<a name="mw-cli-tutorial-tasks-before"></a>

Before your try registering a task with your maintenance window, review the following information\.

**Resource requirements**  
Ensure that you have created the resources you need for each task type\. For example, you should already have created one or more maintenance windows and the targets \(instances\) you want to run the tasks on\. You should have created or identified the other resources you plan to include in the tasks\. For example, for a Run Command task, you should know the name of the S3 bucket you can save command output to\. For a Lambda task, you should have already created a Lambda function; and so on\.

**About registration task options**  
To register a task with a maintenance window, use the AWS CLI [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) command\. This command supports a number of options, some required and some optional, that you can use to register a task with a maintenance window\. Of these, the `--task-invocation-parameters` option is used to specify the parameters that are unique to each task type\.

Before you try the task registration steps, we recommend that you first review the topic [About 'register\-task\-with\-maintenance\-window' Options and Values](register-tasks-options.md)\. This topic describes the option and parameter values you use to register tasks using the register\-task\-with\-maintenance\-window command\. 

**Deprecated options**  
The register\-task\-with\-maintenance\-window also supports two deprecated options that are not included in this tutorial, `--task-parameters` and `--logging-info`\. To specify parameters to pass to a task when it runs, use the `Parameters` option in the `--task-invocation-parameters` structure instead of `TaskParameters`\. To specify an Amazon S3 bucket to contain logs, use the `OutputS3BucketName` and `OutputS3KeyPrefix` options in the `--task-invocation-parameters` structure instead of `LoggingInfo`\. 

**Topics**
+ [Before You Begin](#mw-cli-tutorial-tasks-before)
+ [About 'register\-task\-with\-maintenance\-window' Options and Values](register-tasks-options.md)
+ [Create a Maintenance Window Task \(AWS CLI\)](register-tasks-tutorial.md)