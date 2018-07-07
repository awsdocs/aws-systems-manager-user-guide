# Control Access to Maintenance Windows \(Console\)<a name="sysman-maintenance-perm-console"></a>

The following procedures describe how to use the AWS Systems Manager console to create the required roles and permissions for Maintenance Windows\.

**Topics**
+ [Task 1: Create a Custom Service Role for Maintenance Windows](#sysman-maintenance-role)
+ [Task 2: Assign the IAM PassRole Policy to an IAM User or Group](#sysman-maintenance-passrole)

## Task 1: Create a Custom Service Role for Maintenance Windows<a name="sysman-maintenance-role"></a>

Use the following procedure to create a custom service role for Maintenance Windows so that Systems Manager can run tasks on your behalf\.

**To create a custom service role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Mark the following selections:

   1. ** Select type of trusted entity** area: **AWS service**

   1. **Choose the service that will use this role** area: **EC2**

   1. **Select your use case** area: **EC2**

1. Choose **Next: Permissions**\.

1. In the list of policies, select the box next to **AmazonSSMMaintenanceWindowRole**, and then choose **Next: Review**\.

1. In **Role name**, enter a name that identifies this role as a Maintenance Windows role; for example **my\-maintenance\-window\-role\.**

1. Optional: Change the default role description to reflect the purpose of this role\. For example: "Performs Maintenance Window tasks on your behalf\." 

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose the name of the role you just created\.

1. Choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. Delete the current policy, and then copy and paste the following policy into the **Policy Document** field:

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"",
            "Effect":"Allow",
            "Principal":{
               "Service":[
                  "ssm.amazonaws.com",
                  "sns.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```
**Note**  
`"sns.amazonaws.com"` is required only if you will use Amazon SNS to send notifications related to Maintenance Window tasks run through Run Command\. See step 13 below for more information\.

1. Choose **Update Trust Policy**, and then copy or make a note of the role name and the **Role ARN** value on the **Summary** page\. You will specify this information when you create your Maintenance Window\.

1. If you will configure a Maintenance Window to send notifications about command statuses using Amazon SNS, when run through a Run Command command task, do the following:

   1. Choose the **Permissions** tab\.

   1. Choose **Add inline policy**, and then choose the **JSON** tab\.

   1. In **Policy Document**, paste the following:

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "sns-access-role-arn"
          }
        ]
      }
      ```

      *sns\-access\-role\-arn* represents the ARN of the existing IAM role to be for sending SNS notifications related to the Maintenance Window, in the format of `arn:aws:iam::account-id:role/role-name.` For example: `arn:aws:iam::111222333444:role/my-sns-access-role`\. 
**Note**  
In the Systems Manager console, this ARN is selected in the **IAM Role** list on the **Register run command task** page\. For information, see [Assign Tasks to a Maintenance Window \(Console\)](sysman-maintenance-assign-tasks.md)\. In the Systems Manager API, this ARN is entered as the value of [ServiceRoleArn](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html#EC2-SendCommand-request-ServiceRoleArn) in the [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) request\.

   1. Choose **Review policy**\.

   1. In the **Name** box, type a name to identify this as a policy to allow sending Amazon SNS notifications\.

1. Choose **Create policy**\.

## Task 2: Assign the IAM PassRole Policy to an IAM User or Group<a name="sysman-maintenance-passrole"></a>

When you register a task with a Maintenance Window, you specify a custom service role to run the actual task operations\. This is the role that the service will assume when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the Maintenance Window, the role that should be used when running tasks\.

Depending on whether you are assigning the `iam: Passrole` permission to an individual user or a group, use one of the following procedures to provide the minimum permissions required to register tasks with a Maintenance Window\.

**To assign the IAM PassRole policy to an IAM user account**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, and then choose the name of the user account you want to update\.

1. On the **Permissions** tabs, in the policies list, verify that the `AmazonSSMFullAccess` policy is listed, or that there is a comparable policy that gives the IAM user permission to call the Systems Manager API\. Add the permission if it is not included already\. For information, see [Attaching IAM Policies \(Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#attach-managed-policy-console) in the *IAM User Guide*\. 

1. Choose **Add inline policy**\.

1. On the **Create policy** page, on the **Visual edtior** tab, in the **Select a service** area, choose **IAM**\.

1. In the **Actions** area, choose **PassRole**\.
**Tip**  
Type **passr** in the filter box to quickly locate **PassRole**\.

1. Choose the **Resources** line, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, paste the role ARN you created in the previous procedure, and then choose **Save changes**\.

1. Choose **Review policy**\.

1. On the **Review policy** page, type a name in the **Name** box to identify this PassRole policy, and then choose **Create policy**\.

**To assign the IAM PassRole policy to an IAM group**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Groups**\.

1. In the list of groups, select the name of the group you want to assign the `iam:PassRole` permission to\. 

1. In the **Inline Policies** area, do one of the following:
   + If no inline policies have been added yet, choose **click here**\.
   + If one or more inline policies have been added, choose **Create Group Policy**\.

1. Select **Policy Generator**, and then choose **Select**\.

1. Make the following selections:

   1. **Effect**: Allow

   1. **AWS Service**: Identity and Access Management

   1. **Actions**: PassRole

   1. **Amazon Resource Name \(ARN\)**: Enter the ARN of the Maintenance Window role you created in [Task 1: Create a Custom Service Role for Maintenance Windows](#sysman-maintenance-role)

1. Choose **Add Statement**, and then choose **Next Step**\.

1. Choose **Apply Policy**\.