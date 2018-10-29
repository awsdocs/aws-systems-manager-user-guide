# AWSSupport-SetupIPMonitoringFromVPC

## Description

AWSSupport-SetupIPMonitoringFromVPC creates an EC2 instance in the specified subnet and monitors selected target IPs (IPv4 or IPv6) by continuously running ping, MTR, traceroute and tracetcp tests. The results are stored in CloudWatch logs, and metric filters are applied to quickly visualize latency and packet loss statistics in a CloudWatch dashboard.

### Additional Information

The CloudWatch logs data can be used for network troubleshooting and analysis of pattern/trends. Additionally, you can configure CloudWatch alarms with SNS notifications when packet loss and/or latency reach a threshold. The data can also be used when opening a Premium Support case, to help isolate an issue quickly and reduce time to resolution when investigating a network issue.

## Document Type

Automation

## Owner

Amazon

## Parameters

### SubnetId

- **Type**: String
- **Description**: (Required) The subnet ID for the monitor instance. NOTE: If you specify a private subnet, make sure there is Internet access to allow the monitor instance to setup the test (i.e. install the CloudWatch Logs agent, interact with AWS Systems Manager and Amazon CloudWatch).

### TargetIPs

- **Type**: String
- **Description**: (Required) Comma separated list of IPv4s and/or IPv6s to monitor. No spaces allowed. Maximum size is 255 characters. NOTE: If you provide an invalid IP, the automation will fail and rollback the test setup.

### CloudWatchLogGroupNamePrefix

- **Type**: String
- **Default**: /AWSSupport-SetupIPMonitoringFromVPC
- **Description**: (Optional) Prefix used for each CloudWatch log group created for the test results.

### CloudWatchLogGroupRetentionInDays

- **Type**: String
- **Allowed values**: 1,3,5,7,14,30,60,90,120,150,180,365,400,545,731,1827,3653
- **Default**: 7
- **Description**: (Optional) Number of days you want to keep the network monitoring results for.

### InstanceType

- **Type**: String
- **Allowed values**: t2.micro, t2.small,t2.medium,t2.large
- **Default**: t2.micro
- **Description**: (Optional) The EC2 instance type for the EC2Rescue instance. Recommended size: t2.micro.

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The IAM role for this execution. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that executes this document.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWSSupport-SetupIPMonitoringFromVPC --parameters 'SubnetId=SUBNETID,TargetIPs="IPV41,IPV42,IPV61"'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended the user who executes the automation have the **AmazonSSMAutomationRole** IAM managed policy attached.
In addition to that policy, the user must have:

```json
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Action": [
            "iam:CreateRole",
            "iam:CreateInstanceProfile",
            "iam:GetRole",
            "iam:GetInstanceProfile",
            "iam:DetachRolePolicy",
            "iam:AttachRolePolicy",
            "iam:PassRole",
            "iam:AddRoleToInstanceProfile",
            "iam:RemoveRoleFromInstanceProfile",
            "iam:DeleteRole",
            "iam:DeleteInstanceProfile",
            "iam:PutRolePolicy",
            "iam:DeleteRolePolicy"
         ],
         "Resource": [
            "arn:aws:iam::An-AWS-Account-ID:role/AWSSupport/SetupIPMonitoringFromVPC_*",
            "arn:aws:iam::An-AWS-Account-ID:instance-profile/AWSSupport/SetupIPMonitoringFromVPC_*"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "iam:DetachRolePolicy",
            "iam:AttachRolePolicy"
         ],
         "Resource": [
            "arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "cloudwatch:DeleteDashboards"
         ],
         "Resource": [
            "*"
         ],
         "Effect": "Allow"
      }
   ]
}
```

## Document Steps

1. **aws:executeAwsApi** - describe the provided subnet.
1. **aws:branch** - evaluate the TargetIPs input.
    1. (IPv6) If TargetIPs contains an IPv6:
        1. **aws:assertAwsResourceProperty** - check the provided subnet has an IPv6 pool associated
1. **aws:executeAwsApi** - get the latest Amazon Linux 2 AMI from Parameter Store.
1. **aws:executeAwsApi** - create a security group for the test in the subnet's VPC.
    1. (Cleanup) If the security group creation fails:
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - allow all outbound traffic in the test security group
    1. (Cleanup) If the security group egress rule creation fails:
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - create an IAM role for the test EC2 instance
    1. (Cleanup) If the role creation fails:
        1. **aws:executeAwsApi** - delete the IAM role created by the automation, if it exists.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - attach the AmazonEC2RoleForSSM managed policy
    1. (Cleanup) If the policy attachment fails:
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation, if attached.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - attach an inline policy to allow setting CloudWatch log group retentions and creating a CloudWatch dashboard
    1. (Cleanup) If the inline policy attachment fails:
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation, if created.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - create an IAM instance profile
    1. (Cleanup) If the instance profile creation fails:
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation, if it exists.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:executeAwsApi** - associate the IAM instance profile to the IAM role
    1. (Cleanup) If the instance profile and role association fails:
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role, if associated.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:sleep** - wait for the instance profile to become available
1. **aws:runInstances** - create the test instance in the specified subnet, and with the instance profile created earlier attached.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:branch** - evaluate the TargetIPs input
    1. (IPv6) If TargetIPs contains an IPv6:
        1. **aws:executeAwsApi** - assign an IPv6 to the test instance
1. **aws:waitForAwsResourceProperty** - wait for the test instance to become a managed instance.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - install test pre-requisites:
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - validate the provided IPs are syntactically correct IPv4 and/or IPv6 addresses:
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - define the MTR test for each of the provided IPs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - define the first ping test for each of the provided IPs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - define the second ping test for each of the provided IPs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - define the tracepath test for each of the provided IPs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - define the traceroute test for each of the provided IPs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - configure CloudWatch logs.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - schedule cronjobs to run each test every minute.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:sleep** - wait for the tests to generate some data.
1. **aws:runCommand** - set the desired CloudWatch log group retentions.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - set the CloudWatch log group metric filters.
    1. (Cleanup) If the step fails:
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.
1. **aws:runCommand** - create the CloudWatch dashboard.
    1. (Cleanup) If the step fails:
        1. **aws:executeAwsApi** - delete the CloudWatch dashboard, if it exists.
        1. **aws:changeInstanceState** - terminate the test instance.
        1. **aws:executeAwsApi** - remove the IAM instance profile from the role.
        1. **aws:executeAwsApi** - delete the IAM instance profile created by the automation.
        1. **aws:executeAwsApi** - delete the CloudWatch inline policy from the role created by the automation.
        1. **aws:executeAwsApi** - detach the AmazonEC2RoleForSSM managed policy from the role created by the automation.
        1. **aws:executeAwsApi** - delete the IAM role created by the automation.
        1. **aws:executeAwsApi** - delete the security group created by the automation, if it exists.

## Outputs

createCloudWatchDashboards.Output - the URL of the CloudWatch dashboard

createManagedInstance.InstanceIds - the test instance ID

