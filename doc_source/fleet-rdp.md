# Connect to a managed node using Remote Desktop<a name="fleet-rdp"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to connect to your Amazon Elastic Compute Cloud \(Amazon EC2\) instances using the Remote Desktop Protocol \(RDP\)\. Fleet Manager Remote Desktop, which is powered by NICE DCV, provides you with secure connectivity to your Windows Server instances directly from the Systems Manager console\. You can have up to four simultaneous connections in a single browser window\.

For more information about configuring AWS Identity and Access Management \(IAM\) permissions to allow your instances to interact with Systems Manager, see [Configure instance permissions for Systems Manager](setup-instance-permissions.md)\.

**Note**  
You can only use Remote Desktop with instances that are running Windows Server 2012 RTM or higher\. Remote Desktop supports only English language inputs\.

**Topics**
+ [Setting up your environment](#fleet-rdp-prerequisites)
+ [Configuring IAM permissions for Remote Desktop](#fleet-rdp-iam-policy-examples)
+ [Authenticating Remote Desktop connections](#fleet-rdp-authentication)
+ [Remote connection duration and concurrency](#fleet-rdp-duration-concurrency)
+ [Connect to a managed node using Remote Desktop](#fleet-rdp-connect-to-node)

## Setting up your environment<a name="fleet-rdp-prerequisites"></a>

Before using Remote Desktop, verify that your environment meets the following requirements:
+ **Managed node configuration**

  Make sure that your Amazon EC2 instances are configured as [managed nodes](managed_instances.md) in Systems Manager\.
+ **SSM Agent minimum version**

  Verify that nodes are running SSM Agent version 3\.0\.222\.0 or higher\. For information about how to check which agent version is running on a node, see [Checking the SSM Agent version number](ssm-agent-get-version.md)\. For information about installing or updating SSM Agent, see [Working with SSM Agent](ssm-agent.md)\.
+ **RDP port configuration**

  To accept remote connections, the Remote Desktop Services service on your Windows Server nodes must use default RDP port 3389\. This is the default configuration on Amazon Machine Images \(AMIs\) provided by AWS\. You are not explicitly required to open any inbound ports to use Remote Desktop\.
+ **PSReadLine module version for keyboard functionality**

  To ensure that your keyboard functions properly in PowerShell, verify that nodes running Windows Server 2022 have PSReadLine module version 2\.2\.2 or higher installed\. If they are running an older version, you can install the required version using the following command\.

  ```
  Install-Module `
      -Name PSReadLine `
      -Repository PSGallery `-MinimumVersion 2.2.2
  ```
+ **Session Manager configuration**

  Before you can use Remote Desktop, you must complete the prerequisites for Session Manager setup\. When you connect to an instance using Remote Desktop, any session preferences defined for your AWS account and AWS Region are applied\. For more information, see [Setting up Session Manager](session-manager-getting-started.md)\.
**Note**  
If you log Session Manager activity using Amazon Simple Storage Service \(Amazon S3\), then your Remote Desktop connections will generate the following error in `bucket_name/Port/stderr`\. This error is expected behavior and can be safely ignored\.  

  ```
  Setting up data channel with id SESSION_ID failed: failed to create websocket for datachannel with error: CreateDataChannel failed with no output or error: createDataChannel request failed: unexpected response from the service <BadRequest>
  <ClientErrorMessage>Session is already terminated</ClientErrorMessage>
  </BadRequest>
  ```

## Configuring IAM permissions for Remote Desktop<a name="fleet-rdp-iam-policy-examples"></a>

In addition to the required IAM permissions for Systems Manager and Session Manager, the user or role you use to access the console must allow the following actions:
+ `ssm-guiconnect:CancelConnection`
+ `ssm-guiconnect:GetConnection`
+ `ssm-guiconnect:StartConnection`

The following are example IAM policies that you can attach to a user or role to allow different types of interaction with Remote Desktop\. Replace each *example resource placeholder* with your own information\.

### Standard policy<a name="standard-policy"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:GetPasswordData"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SSM",
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeInstanceProperties",
                "ssm:GetCommandInvocation",
                "ssm:GetInventorySchema"
            ],
            "Resource": "*"
        },
        {
            "Sid": "TerminateSession",
            "Effect": "Allow",
            "Action": [
                "ssm:TerminateSession"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/aws:ssmmessages:session-id": [
                        "${aws:userid}"
                    ]
                }
            }
        },
        {
            "Sid": "SSMStartSession",
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ec2:*:account-id:instance/*",
                "arn:aws:ssm:*:account-id:managed-instance/*",
                "arn:aws:ssm:*::document/AWS-StartPortForwardingSession"
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true"
                }
            }
        },
        {
            "Sid": "GuiConnect",
            "Effect": "Allow",
            "Action": [
                "ssm-guiconnect:CancelConnection",
                "ssm-guiconnect:GetConnection",
                "ssm-guiconnect:StartConnection"
            ],
            "Resource": "*"
        }
    ]
}
```

### Policy for connecting to instances with specific tags<a name="tag-policy"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:GetPasswordData"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SSM",
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeInstanceProperties",
                "ssm:GetCommandInvocation",
                "ssm:GetInventorySchema"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SSMStartSession",
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ssm:*::document/AWS-StartPortForwardingSession"
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true"
                }
            }
        },
        {
            "Sid": "AccessTaggedInstances",
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ec2:*:account-id:instance/*",
                "arn:aws:ssm:*:account-id:managed-instance/*"
            ],
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/tag key": [
                        "tag value"
                    ]
                }
            }
        },
        {
            "Sid": "GuiConnect",
            "Effect": "Allow",
            "Action": [
                "ssm-guiconnect:CancelConnection",
                "ssm-guiconnect:GetConnection",
                "ssm-guiconnect:StartConnection"
            ],
            "Resource": "*"
        }
    ]
}
```

### Policy for AWS IAM Identity Center \(successor to AWS Single Sign\-On\) users<a name="sso-policy"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SSO",
            "Effect": "Allow",
            "Action": [
                "sso:ListDirectoryAssociations*",
                "identitystore:DescribeUser"
            ],
            "Resource": "*"
        },
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:GetPasswordData"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SSM",
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeInstanceProperties",
                "ssm:GetCommandInvocation",
                "ssm:GetInventorySchema"
            ],
            "Resource": "*"
        },
        {
            "Sid": "TerminateSession",
            "Effect": "Allow",
            "Action": [
                "ssm:TerminateSession"
            ],
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/aws:ssmmessages:session-id": [
                        "${aws:userName}"
                    ]
                }
            }
        },
        {
            "Sid": "SSMStartSession",
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/*",
                "arn:aws:ssm:*:*:managed-instance/*",
                "arn:aws:ssm:*:*:document/AWS-StartPortForwardingSession"
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true"
                }
            }
        },
        {
            "Sid": "SSMSendCommand",
            "Effect": "Allow",
            "Action": [
                "ssm:SendCommand"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/*",
                "arn:aws:ssm:*:*:managed-instance/*",
                "arn:aws:ssm:*:*:document/AWSSSO-CreateSSOUser"
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true"
                }
            }
        },
        {
            "Sid": "GuiConnect",
            "Effect": "Allow",
            "Action": [
                "ssm-guiconnect:CancelConnection",
                "ssm-guiconnect:GetConnection",
                "ssm-guiconnect:StartConnection"
            ],
            "Resource": "*"
        }
    ]
}
```

## Authenticating Remote Desktop connections<a name="fleet-rdp-authentication"></a>

When establishing a remote connection, you can authenticate using Windows credentials or the Amazon EC2 key pair \(\.pem file\) that is associated with the instance\. For information about using key pairs, see [Amazon EC2 key pairs and Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Windows Instances*\.

Alternatively, if you're authenticated to the AWS Management Console using AWS IAM Identity Center \(successor to AWS Single Sign\-On\), you can connect to your instances without providing additional credentials\. For an example of a policy to allow remote connection authentication using IAM Identity Center, see [Configuring IAM permissions for Remote Desktop](#fleet-rdp-iam-policy-examples)\. 

**Note**  
Note the following conditions for using IAM Identity Center authentication:  
Remote Desktop supports IAM Identity Center authentication for nodes in the same AWS Region where you enabled IAM Identity Center\.
Remote Desktop supports IAM Identity Center user names of up to 16 characters\. 
When a connection is authenticated using IAM Identity Center, Remote Desktop creates a local Windows user in the instance’s Local Administrators group\. This user persists after the remote connection has ended\. 
Remote Desktop does not allow IAM Identity Center authentication for nodes that are Microsoft Active Directory domain controllers\.
Although Remote Desktop allows you to use IAM Identity Center authentication for nodes *joined* to an Active Directory domain, we do not recommend doing so\. This authentication method grants administrative permissions to users which might override more restrictive permissions granted by the domain\.

## Remote connection duration and concurrency<a name="fleet-rdp-duration-concurrency"></a>

The following conditions apply to active Remote Desktop connections:
+ **Connection duration**

  By default, a Remote Desktop connection is disconnected after 60 minutes\. To prevent a connection from being disconnected, you can choose **Renew session** before being disconnected to reset the duration timer\.
+ **Connection timeout**

  A Remote Desktop connection disconnects after it has been idle for more than 10 minutes\.
+ **Concurrent connections**

  By default, you can have a maximum of 5 active Remote Desktop connections at one time for the same AWS account and AWS Region\. To request a service quota increase of up to 25 concurrent connections, see [Requesting a quota increase ](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html)in the *Service Quotas User Guide*\.

## Connect to a managed node using Remote Desktop<a name="fleet-rdp-connect-to-node"></a>

**To connect to a managed node using Fleet Manager Remote Desktop**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the node that you want to connect to\. You can select either the check box or the node name\.

1. On the **Node actions** menu, choose **Connect with Remote Desktop**\.

1. Choose your preferred **Authentication type**\. If you choose **User credentials**, enter the user name and password for a Windows user account on the node that you're connecting to\. If you choose **Key pair**, you can provide authentication using one of the following methods:

   1. Choose **Browse local machine** if you want to select the PEM key associated with your instance from your local file system\.

      \- or \-

   1. Choose **Paste key pair content** if you want to copy the contents of the PEM file and paste them in to the provided field\.

1. Select **Connect**\.