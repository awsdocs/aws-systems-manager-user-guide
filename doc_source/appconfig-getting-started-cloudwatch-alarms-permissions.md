# \(Optional\) Configuring permissions for rollback based on CloudWatch alarms<a name="appconfig-getting-started-cloudwatch-alarms-permissions"></a>

You can configure AWS AppConfig to roll back to a previous version of a configuration in response to one or more Amazon CloudWatch alarms\. When you configure a deployment to respond to CloudWatch alarms, you specify an AWS Identity and Access Management \(IAM\) role\. AppConfig requires this role so that it can monitor CloudWatch alarms even if those alarms weren't created in the current AWS account\.

Use the following procedures to create an IAM role that enables AppConfig to rollback based on CloudWatch alarms\. This section includes the following procedures\.

1. [Step 1: Create the permission policy for rollback based on CloudWatch alarms](#appconfig-getting-started-cloudwatch-alarms-permissions-policy)

1. [Step 2: Create the IAM role for rollback based on CloudWatch alarms](#appconfig-getting-started-cloudwatch-alarms-permissions-role)

1. [Step 3: Add a trust relationship](#appconfig-getting-started-cloudwatch-alarms-permissions-trust)

## Step 1: Create the permission policy for rollback based on CloudWatch alarms<a name="appconfig-getting-started-cloudwatch-alarms-permissions-policy"></a>

Use the following procedure to create an IAM policy that gives AppConfig permission to call the `DescribeAlarms` API action\. 

**To create an IAM permission policy for rollback based on CloudWatch alarms**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. Replace the default content on the JSON tab with the following permission policy, and then choose **Review**\.

   ```
   {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Effect": "Allow",
                   "Action": [
                       "cloudwatch:DescribeAlarms"
                   ],
                   "Resource": "*"
               }
           ]
       }
   ```

1. On the **Review** page, enter **SSMCloudWatchAlarmDiscoveryRole** in the **Role name** field\. 

1. Choose **Create policy**\. The system returns you to the **Policies** page\.

## Step 2: Create the IAM role for rollback based on CloudWatch alarms<a name="appconfig-getting-started-cloudwatch-alarms-permissions-role"></a>

Use the following procedure to create an IAM role and assign the policy you created in the previous procedure to it\. 

**To create an IAM role for rollback based on CloudWatch alarms**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Immediately under **Choose the service that will use this role**, choose **EC2**, and then choose **Next: Permissions**\.  
![\[Choosing the EC2 service in the IAM console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/setup-instance-profile.png)

1. On the **Attached permissions policy** page, search for **SSMCloudWatchAlarmDiscoveryRole**\. 

1. Choose this policy and then choose **Next: Tags**\.

1. Enter tags for this role, and then choose **Next: Review**\.

1. On the **Create role** page, enter a name in the **Role name** field, and then choose **Create role**\.

1. On the **Roles** page, choose the role you just created\. The **Summary** page opens\. 

## Step 3: Add a trust relationship<a name="appconfig-getting-started-cloudwatch-alarms-permissions-trust"></a>

Use the following procedure to configure the role you just created to trust AppConfig\.

**To add a trust relationship for AppConfig**

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. Edit the policy to include only "`appconfig.amazonaws.com`", as shown in the following example:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "appconfig.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.