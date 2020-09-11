# AWSSupport\-SetupIPMonitoringFromVPC<a name="automation-awssupport-setupipmonitoringfromvpc"></a>

**Description**

AWSSupport\-SetupIPMonitoringFromVPC creates an EC2 instance in the specified subnet and monitors selected target IPs \(IPv4 or IPv6\) by continuously running ping, MTR, traceroute and tracetcp tests\. The results are stored in Amazon CloudWatch Logs logs, and metric filters are applied to quickly visualize latency and packet loss statistics in a CloudWatch dashboard\.

**Additional Information**

The CloudWatch Logs data can be used for network troubleshooting and analysis of pattern/trends\. Additionally, you can configure CloudWatch alarms with Amazon SNS notifications when packet loss and/or latency reach a threshold\. The data can also be used when opening a Premium Support case, to help isolate an issue quickly and reduce time to resolution when investigating a network issue\.

**Note**  
To clean up resources created by AWSSupport\-SetupIPMonitoringFromVPC, you can run the Automation document `AWSSupport-TerminateIPMonitoringFromVPC`\. For more information, see [AWSSupport\-TerminateIPMonitoringFromVPC](automation-awssupport-terminateipmonitoringfromvpc.md)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-SetupIPMonitoringFromVPC)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ CloudWatchLogGroupNamePrefix

  Type: String

  Default: /AWSSupport\-SetupIPMonitoringFromVPC

  Description: \(Optional\) Prefix used for each CloudWatch log group created for the test results\.
+ CloudWatchLogGroupRetentionInDays

  Type: String

  Valid values: 1 \| 3 \| 5 \| 7 \| 14 \| 30 \| 60 \| 90 \| 120 \| 150 \| 180 \| 365 \| 400 \| 545 \| 731 \| 1827 \| 3653

  Default: 7

  Description: \(Optional\) Number of days you want to keep the network monitoring results for\.
+ InstanceType

  Type: String

  Valid values: t2\.micro \| t2\.small \| t2\.medium \| t2\.large

  Default: t2\.micro

  Description: \(Optional\) The EC2 instance type for the EC2Rescue instance\. Recommended size: t2\.micro\.
+ SubnetId

  Type: String

  Description: \(Required\) The subnet ID for the monitor instance\. Be aware that if you specify a private subnet, then you must make sure there is Internet access to allow the monitor instance to setup the test \(meaning, install the CloudWatch Logs agent, interact with Systems Manager and CloudWatch\)\.
+ TargetIPs

  Type: String

  Description: \(Required\) Comma separated list of IPv4s and/or IPv6s to monitor\. No spaces allowed\. Maximum size is 255 characters\. Be aware that if you provide an invalid IP, then the automation will fail and rollback the test setup\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

It is recommended that the user who runs the automation have the **AmazonSSMAutomationRole** IAM managed policy attached\. In addition, the user must have the following policy attached to their user account, group, or role:

```
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
                "arn:aws:iam::AWS_account_ID:role/AWSSupport/SetupIPMonitoringFromVPC_*",
                "arn:aws:iam::AWS_account_ID:instance-profile/AWSSupport/SetupIPMonitoringFromVPC_*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:DetachRolePolicy",
                "iam:AttachRolePolicy"
            ],
            "Resource": [
                "arn:aws:iam::aws:policy/service-role/AmazonSSMManagedInstanceCore"
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
        },
        {
            "Action": [
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:CreateSecurityGroup",
                "ec2:DeleteSecurityGroup",
                "ec2:DescribeSubnets"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

**Document Steps**

1. **aws:executeAwsApi** \- describe the provided subnet\.

1. **aws:branch** \- evaluate the TargetIPs input\. 

   \(IPv6\) If TargetIPs contains an IPv6: 

   **aws:assertAwsResourceProperty** \- check the provided subnet has an IPv6 pool associated

1. **aws:executeAwsApi** \- get the latest Amazon Linux 2 AMI from Parameter Store\.

1. **aws:executeAwsApi** \- create a security group for the test in the subnet's VPC\. 

   \(Cleanup\) If the security group creation fails: 

   **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- allow all outbound traffic in the test security group\.

   \(Cleanup\) If the security group egress rule creation fails: 

   **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- create an IAM role for the test EC2 instance 

   \(Cleanup\) If the role creation fails: 

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation, if it exists\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- attach the AmazonSSMManagedInstanceCore managed policy 

   \(Cleanup\) If the policy attachment fails: 

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation, if attached\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- attach an inline policy to allow setting CloudWatch log group retentions and creating a CloudWatch dashboard 

   \(Cleanup\) If the inline policy attachment fails: 

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation, if created\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- create an IAM instance profile\. 

   \(Cleanup\) If the instance profile creation fails: 

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation, if it exists\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:executeAwsApi** \- associate the IAM instance profile to the IAM role\. 

   \(Cleanup\) If the instance profile and role association fails: 

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role, if associated\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:sleep** \- wait for the instance profile to become available\.

1. **aws:runInstances** \- create the test instance in the specified subnet, and with the instance profile created earlier attached\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:branch** \- evaluate the TargetIPs input\. 

   \(IPv6\) If TargetIPs contains an IPv6: 

   **aws:executeAwsApi** \- assign an IPv6 to the test instance\.

1. **aws:waitForAwsResourceProperty** \- wait for the test instance to become a managed instance\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- install test pre\-requisites: 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- validate the provided IPs are syntactically correct IPv4 and/or IPv6 addresses: 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- define the MTR test for each of the provided IPs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- define the first ping test for each of the provided IPs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- define the second ping test for each of the provided IPs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- define the tracepath test for each of the provided IPs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- define the traceroute test for each of the provided IPs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- configure CloudWatch logs\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- schedule cronjobs to run each test every minute\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:sleep** \- wait for the tests to generate some data\.

1. **aws:runCommand** \- set the desired CloudWatch log group retentions\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- set the CloudWatch log group metric filters\. 

   \(Cleanup\) If the step fails: 

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

1. **aws:runCommand** \- create the CloudWatch dashboard\. 

   \(Cleanup\) If the step fails: 

   1. **aws:executeAwsApi** \- delete the CloudWatch dashboard, if it exists\.

   1. **aws:changeInstanceState** \- terminate the test instance\.

   1. **aws:executeAwsApi** \- remove the IAM instance profile from the role\.

   1. **aws:executeAwsApi** \- delete the IAM instance profile created by the automation\.

   1. **aws:executeAwsApi** \- delete the CloudWatch inline policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- detach the AmazonSSMManagedInstanceCore managed policy from the role created by the automation\.

   1. **aws:executeAwsApi** \- delete the IAM role created by the automation\.

   1. **aws:executeAwsApi** \- delete the security group created by the automation, if it exists\.

**Outputs**

createCloudWatchDashboards\.Output \- the URL of the CloudWatch dashboard\.

createManagedInstance\.InstanceIds \- the test instance ID\.