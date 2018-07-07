# Viewing Association Histories<a name="sysman-state-assoc-history"></a>

You can view all executions for a specific association ID by using the [DescribeAssociationExecutions](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAssociationExecutions.html) API action\. This action allows you to quickly see the status, detailed status, results, last execution time, and more information for a State Manager association\. This API action also includes filters to help you quickly locate associations according to the criteria you specify\. For example, you can specify an exact date and time, and use a GREATER\_THAN filter to view only those executions that were processed after the specified date and time\.

If, for example, an association execution failed, you can drill down into the details of a specific execution by using the [DescribeAssociationExecutionTargets](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAssociationExecutionTargets.html) API action\. This action shows you the resources, such as instance IDs, where the association ran and the various association statuses\. You can then quickly see which resource or instance failed to run an association\. With the resource ID you can then view the command execution details to see exactly which step in a command failed\.

The examples in this section also include information about how to use the [StartAssociationsOnce](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAssociationsOnce.html) API action to run an association immediately and only one time\. You can use this API action when you investigate failed association executions\. If you see that an association failed, you can make a change on the resource, and then immediately run the association to see if the change on the resource allows the association to run successfully\.

## Viewing Association Histories by Using the Systems Manager Console<a name="sysman-state-assoc-history-console"></a>

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

## Viewing Association Histories by Using the AWS CLI<a name="sysman-state-assoc-history-cli"></a>

Use the following procedure to view the execution history for a specific association ID and then view execution details for one or more resources\. 

**To view execution history for a specific association ID**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to view a list of executions for a specific association ID\. This command includes a filter to limit the results to only those executions that occurred after a specific date and time\. If you want to view all executions for a specific association ID, remove the `--filter parameter.`

   ```
   aws ssm describe-association-executions --association-id ID --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN
   ```

   The system returns information like the following\.

   ```
   {
      "AssociationExecutions":[
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"12345-abcdef-6789-ghij",
            "ExecutionId":"abcde-12345-fghi-6789",
            "CreatedTime":1523986028.219,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"12345-abcdef-6789-ghij",
            "ExecutionId":"zzzz-333-xxxx-4444",
            "CreatedTime":1523984226.074,
            "AssociationVersion":"1"
         },
         {
            "Status":"Success",
            "DetailedStatus":"Success",
            "AssociationId":"12345-abcdef-6789-ghij",
            "ExecutionId":"4545-a4a4a4-3636",
            "CreatedTime":1523982404.013,
            "AssociationVersion":"1"
         }
      ]
   }
   ```

   You can limit the results by using one or more filters\. The following example returns all associations that were run before a specific date and time\. 

   ```
   aws ssm describe-association-executions --association-id ID --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=LESS_THAN 
   ```

   The following returns all associations that were *successfully* run after a specific date and time\.

   ```
   aws ssm describe-association-executions --association-id ID --filters Key=CreatedTime,Value="2018-04-10T19:15:38.372Z",Type=GREATER_THAN Key=Status,Value=Success,Type=EQUAL
   ```

1. Execute the following command to view all targets where the specific execution ran\.

   ```
   aws ssm describe-association-execution-targets --association-id ID --execution-id ID
   ```

   You can limit the results by using one or more filters\. The following example returns information about all targets where the specific association failed to run\.

   ```
   aws ssm describe-association-execution-targets --association-id ID --execution-id ID --filters Key=Status,Value="Failed"
   ```

   The following example returns information about a specific managed instance where an association failed to run\.

   ```
   aws ssm describe-association-execution-targets --association-id ID --execution-id ID --filters Key=Status,Value=Failed  Key=ResourceId,Value="instance ID" Key=ResourceType,Value=ManagedInstance
   ```

1. If you are investigating an association that failed to run, you can use the [StartAssociationsOnce](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAssociationsOnce.html) API action to run an association immediately and only one time\. After you make changes on the resource where the association failed to run, run the following command to run the association immediately and only one time\.

   ```
   aws ssm start-associations-once --association-id ID 
   ```