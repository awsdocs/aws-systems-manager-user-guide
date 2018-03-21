# Control Access to Maintenance Windows \(Console\)<a name="sysman-maintenance-perm-console"></a>

The following procedures describe how to create the required roles and permissions for Maintenance Windows by using the AWS console\.


+ [Create an IAM Role for Systems Manager](#sysman-maintenance-role)
+ [Assign the IAM PassRole Policy to an IAM User Account](#sysman-maintenance-passrole)

## Create an IAM Role for Systems Manager<a name="sysman-maintenance-role"></a>

Use the following procedure to create a role so that Systems Manager can run tasks in Maintenance Windows on your behalf\.

**To create an IAM role for Maintenance Windows**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under** Select type of trusted entity**, choose **AWS service**\. Under **Choose the service that will use this role**, choose **EC2**\. Under **Select your use case**, choose **EC2**, and then choose **Next: Permissions**\.

1. In the list of policies, select the box next to **AmazonSSMMaintenanceWindowRole**, and then choose **Next: Review**\.

1. In **Role name**, enter a name that identifies this role as a Maintenance Windows role\.

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
                  "ec2.amazonaws.com",
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
`"sns.amazonaws.com"` is required only if you will use Amazon SNS to send notifications related to Maintenance Window tasks run through Run Command\. See step 11 below for more information\.

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

      *sns\-access\-role\-arn* represents the ARN of the IAM role to be for sending SNS notifications related to the Maintenance Window, in the format of `arn:aws:iam::account-id:role/role-name.` For example: `arn:aws:iam::111222333444:role/SNS-Access-role`\. 
**Note**  
In the Systems Manager console, this ARN is selected in the **IAM Role** list on the **Register run command task** page\. For information, see [Assign Tasks to a Maintenance Window](sysman-maintenance-assign-tasks.md)\. In the Systems Manager API, this ARN is entered as the value of [ServiceRoleArn](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html#EC2-SendCommand-request-ServiceRoleArn) in the [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) request\.

   1. Choose **Review policy**\.

   1. In the **Name** box, type a name to identify this as a policy to allow sending Amazon SNS notifications\.

1. Choose **Create policy**\.

## Assign the IAM PassRole Policy to an IAM User Account<a name="sysman-maintenance-passrole"></a>

When you register a task with a Maintenance Window, you specify the role you created in the previous procedure\. This is the role that the service will assume when it runs tasks on your behalf\. In order to register the task, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to your IAM user account\. The policy in the following procedure provides the minimum permissions required to register tasks with a Maintenance Window\.

**To assign the IAM PassRole policy to an IAM user account**

1. In the IAM console navigation pane, choose **Users**, and then choose the name of the user account you want to update\.

1. On the **Permissions** tabs, in the policies list, verify that the **AmazonSSMFullAccess** policy is listed, or that there is a comparable policy that gives the IAM user permission to call the Systems Manager API\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, in the **Select a service** area, choose **Choose a service**, and then choose **IAM**\.

1. Choose **Select actions**, and then choose **PassRole**\.
**Tip**  
Type **passr** in the filter box to quickly locate **PassRole**\.

1. Choose the **Resources** line, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, paste the role ARN you created in the previous procedure, and then choose **Save changes**\.

1. Choose **Review policy**\.

1. On the **Review Policy** page, type a name in the **Name** box, and then choose **Create policy**\.