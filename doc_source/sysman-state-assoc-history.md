# Viewing association histories<a name="sysman-state-assoc-history"></a>

You can view all executions for a specific association ID by using the [DescribeAssociationExecutions](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAssociationExecutions.html) API action\. This action allows you to quickly see the status, detailed status, results, last execution time, and more information for a State Manager association\. This API action also includes filters to help you quickly locate associations according to the criteria you specify\. For example, you can specify an exact date and time, and use a GREATER\_THAN filter to view only those executions that were processed after the specified date and time\.

If, for example, an association execution failed, you can drill down into the details of a specific execution by using the [DescribeAssociationExecutionTargets](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAssociationExecutionTargets.html) API action\. This action shows you the resources, such as instance IDs, where the association ran and the various association statuses\. You can then quickly see which resource or instance failed to run an association\. With the resource ID you can then view the command execution details to see exactly which step in a command failed\.

The examples in this section also include information about how to use the [StartAssociationsOnce](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAssociationsOnce.html) API action to run an association immediately and only one time\. You can use this API action when you investigate failed association executions\. If you see that an association failed, you can make a change on the resource, and then immediately run the association to see if the change on the resource allows the association to run successfully\.

## Viewing association histories \(console\)<a name="sysman-state-assoc-history-console"></a>

Use the following procedure to view the execution history for a specific association ID and then view execution details for one or more resources\. 

**To view execution history for a specific association ID**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. Choose **State Manager**\.

1. In the **Association id** field, choose an association for which you want to view the history\.

1. Choose the **View details** button\.

1. Choose the **Execution history** tab\.

1. Choose an association for which you want to view resource\-level execution details\. For example, choose an association that shows a status of **Failed**\. You can then view the execution details for the instances that failed to run the association\.
**Note**  
Use the search box filters to locate the execution for which you want to view details\.  

![\[Filtering the list of State Manager association executions.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-state-executions-filter.png)

1. Choose an execution ID\. The **Association execution targets** page opens\. This page shows all of the resources that ran the association\.

1. Choose a resource ID to view specific information about that resource\.
**Note**  
Use the search box filters to locate the resource for which you want to view details\.  

![\[Filtering the list of State Manager association executions targets.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-state-executions-targets-filter.png)

1. If you are investigating an association that failed to run, you can use the **Apply association now** button to run an association immediately and only one time\. After you made changes on the resource where the association failed to run, choose the **Association ID** link in the navigation breadcrumb\.

1. Choose the **Apply association now** button\. After the execution is complete, verify that the association execution succeeded\.

## Viewing association histories \(command line\)<a name="sysman-state-assoc-history-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to view the execution history for a specific association ID\. Following this, the procedure describes how to view execution details for one or more resources\.

**To view execution history for a specific association ID**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to view a list of executions for a specific association ID\.

------
#### [ Linux ]

   ```
   aws ssm describe-association-executions \
     --association-id ID \
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN
   ```

**Note**  
This command includes a filter to limit the results to only those executions that occurred after a specific date and time\. If you want to view all executions for a specific association ID, remove the `--filters` parameter and ` Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN` value\.

------
#### [ Windows ]

   ```
   aws ssm describe-association-executions ^
     --association-id ID ^
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN
   ```

**Note**  
This command includes a filter to limit the results to only those executions that occurred after a specific date and time\. If you want to view all executions for a specific association ID, remove the `--filters` parameter and ` Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN` value\.

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecution `
     -AssociationId ID `
     -Filter @{"Key"="CreatedTime";"Value"="2019-06-01T19:15:38.372Z";"Type"="GREATER_THAN"}
   ```

**Note**  
This command includes a filter to limit the results to only those executions that occurred after a specific date and time\. If you want to view all executions for a specific association ID, remove the `-Filter` parameter and ` @{"Key"="CreatedTime";"Value"="2019-06-01T19:15:38.372Z";"Type"="GREATER_THAN"}` value\.

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "AssociationExecutions":[
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"76a5a04f-caf6-490c-b448-92c02EXAMPLE",
            "CreatedTime":1523986028.219,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"791b72e0-f0da-4021-8b35-f95dfEXAMPLE",
            "CreatedTime":1523984226.074,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"ecec60fa-6bb0-4d26-98c7-140308EXAMPLE",
            "CreatedTime":1523982404.013,
            "AssociationVersion":"1"
         }
      ]
   }
   ```

------
#### [ Windows ]

   ```
   {
      "AssociationExecutions":[
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"76a5a04f-caf6-490c-b448-92c02EXAMPLE",
            "CreatedTime":1523986028.219,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"791b72e0-f0da-4021-8b35-f95dfEXAMPLE",
            "CreatedTime":1523984226.074,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
            "ExecutionId":"ecec60fa-6bb0-4d26-98c7-140308EXAMPLE",
            "CreatedTime":1523982404.013,
            "AssociationVersion":"1"
         }
      ]
   }
   ```

------
#### [ PowerShell ]

   ```
   AssociationId         : c336d2ab-09de-44ba-8f6a-6136cEXAMPLE
   AssociationVersion    : 1
   CreatedTime           : 8/18/2019 2:00:50 AM
   DetailedStatus        : Success
   ExecutionId           : 76a5a04f-caf6-490c-b448-92c02EXAMPLE
   LastExecutionDate     : 1/1/0001 12:00:00 AM
   ResourceCountByStatus : {Success=1}
   Status                : Success
   
   AssociationId         : c336d2ab-09de-44ba-8f6a-6136cEXAMPLE
   AssociationVersion    : 1
   CreatedTime           : 8/11/2019 2:00:54 AM
   DetailedStatus        : Success
   ExecutionId           : 791b72e0-f0da-4021-8b35-f95dfEXAMPLE
   LastExecutionDate     : 1/1/0001 12:00:00 AM
   ResourceCountByStatus : {Success=1}
   Status                : Success
   
   AssociationId         : c336d2ab-09de-44ba-8f6a-6136cEXAMPLE
   AssociationVersion    : 1
   CreatedTime           : 8/4/2019 2:01:00 AM
   DetailedStatus        : Success
   ExecutionId           : ecec60fa-6bb0-4d26-98c7-140308EXAMPLE
   LastExecutionDate     : 1/1/0001 12:00:00 AM
   ResourceCountByStatus : {Success=1}
   Status                : Success
   ```

------

   You can limit the results by using one or more filters\. The following example returns all associations that were run before a specific date and time\. 

------
#### [ Linux ]

   ```
   aws ssm describe-association-executions \
     --association-id ID \
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=LESS_THAN
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association-executions ^
     --association-id ID ^
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=LESS_THAN
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecution `
     -AssociationId 14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE `
     -Filter @{"Key"="CreatedTime";"Value"="2019-06-01T19:15:38.372Z";"Type"="LESS_THAN"}
   ```

------

   The following returns all associations that were *successfully* run after a specific date and time\.

------
#### [ Linux ]

   ```
   aws ssm describe-association-executions \
     --association-id ID \
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN Key=Status,Value=Success,Type=EQUAL
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association-executions ^
     --association-id ID ^
     --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN Key=Status,Value=Success,Type=EQUAL
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecution `
     -AssociationId 14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE `
     -Filter @{
         "Key"="CreatedTime";
         "Value"="2019-06-01T19:15:38.372Z";
         "Type"="GREATER_THAN"
       },
       @{
         "Key"="Status";
         "Value"="Success";
         "Type"="EQUAL"
       }
   ```

------

1. Run the following command to view all targets where the specific execution ran\.

------
#### [ Linux ]

   ```
   aws ssm describe-association-execution-targets \
     --association-id ID \
     --execution-id ID
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association-execution-targets ^
     --association-id ID ^
     --execution-id ID
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecutionTarget `
     -AssociationId 14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE `
     -ExecutionId 76a5a04f-caf6-490c-b448-92c02EXAMPLE
   ```

------

   You can limit the results by using one or more filters\. The following example returns information about all targets where the specific association failed to run\.

------
#### [ Linux ]

   ```
   aws ssm describe-association-execution-targets \
     --association-id ID \
     --execution-id ID \
     --filters Key=Status,Value="Failed"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association-execution-targets ^
     --association-id ID ^
     --execution-id ID ^
     --filters Key=Status,Value="Failed"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecutionTarget `
     -AssociationId 14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE `
     -ExecutionId 76a5a04f-caf6-490c-b448-92c02EXAMPLE `
     -Filter @{
         "Key"="Status";
         "Value"="Failed"
       }
   ```

------

   The following example returns information about a specific managed instance where an association failed to run\.

------
#### [ Linux ]

   ```
   aws ssm describe-association-execution-targets \
     --association-id ID \
     --execution-id ID \
     --filters Key=Status,Value=Failed Key=ResourceId,Value="i-02573cafcfEXAMPLE" Key=ResourceType,Value=ManagedInstance
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association-execution-targets ^
     --association-id ID ^
     --execution-id ID ^
     --filters Key=Status,Value=Failed Key=ResourceId,Value="i-02573cafcfEXAMPLE" Key=ResourceType,Value=ManagedInstance
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationExecutionTarget `
     -AssociationId 14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE `
     -ExecutionId 76a5a04f-caf6-490c-b448-92c02EXAMPLE `
     -Filter @{
         "Key"="Status";
         "Value"="Success"
       },
       @{
         "Key"="ResourceId";
         "Value"="i-02573cafcfEXAMPLE"
       },
       @{
         "Key"="ResourceType";
         "Value"="ManagedInstance"
       }
   ```

------

1. If you are investigating an association that failed to run, you can use the [StartAssociationsOnce](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAssociationsOnce.html) API action to run an association immediately and only one time\. After you change the resource where the association failed to run, run the following command to run the association immediately and only one time\.

------
#### [ Linux ]

   ```
   aws ssm start-associations-once \
     --association-id ID
   ```

------
#### [ Windows ]

   ```
   aws ssm start-associations-once ^
     --association-id ID
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAssociationsOnce `
     -AssociationId ID
   ```

------