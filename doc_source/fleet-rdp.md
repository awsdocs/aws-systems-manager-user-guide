# Connect using Remote Desktop<a name="fleet-rdp"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to connect to your Windows Server 2012 RTM and later instances using the Remote Desktop Protocol \(RDP\)\. These Remote Desktop sessions powered by NICE DCV provide secure connections to your instances directly from your browser\. With Fleet Manager, you can connect a maximum of four instances per browser window\. Currently, Fleet Manager supports only English language inputs\. Though you can connect to instances with Fleet Manager using RDP without opening any inbound ports, Fleet Manager only supports operating system configurations that use the default RDP port 3389 at this time\. If you've changed the value of the listening port for RDP on your instance, Fleet Manager fails to establish the connection\. When connecting to your instance, you can use Windows credentials or the Amazon EC2 key pair \(\.pem file\) associated with the instance for authentication\. For information about Amazon EC2 key pairs, see [Amazon EC2 key pairs and Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) and [Amazon EC2 key pairs and Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances* and *Amazon EC2 User Guide for Windows Instances*\.

Alternatively, if you're authenticated to the AWS Management Console using AWS IAM Identity Center \(successor to AWS Single Sign\-On\), Fleet Manager integrates with IAM Identity Center so you can connect to your instances without providing additional credentials\. Fleet Manager supports IAM Identity Center authenticated RDP connections in the same AWS Region where you enabled IAM Identity Center and user names can be a maximum of 16 characters\. For IAM Identity Center authenticated RDP connections, Fleet Manager creates a local user that is added to the Local Administrators group on the instance that persists after the connection ends\. IAM Identity Center authenticated RDP connections are not supported for nodes that are Microsoft Active Directory domain controllers\.

**Important**  
Note the following important details\.  
To use Fleet Manager with RDP, you must have SSM Agent version 3\.0\.222\.0 or higher running on your instances\. For information about how to determine the version number running on an instance, see [Checking the SSM Agent version number](ssm-agent-get-version.md)\. For information about manually installing or automatically updating SSM Agent, see [Working with SSM Agent](ssm-agent.md)\.
Fleet Manager RDP connections have a maximum session duration of 60 minutes\. When that duration is reached, Fleet Manager disconnects the session\. You can reconnect to the same session by using your credentials\. Alternatively, you can select **Renew session** before the session ends to restart the duration timer\.
Fleet Manager RDP connections have an idle session timeout of 10 minutes\. When that duration is reached, Fleet Manager disconnects the session\. You can reconnect to the same session by using your credentials\.
By default, you can have a maximum of 5 concurrent Fleet Manager RDP connections in the same AWS account and AWS Region\. You can request a service quota increase using the Service Quotas console for up to 25 concurrent RDP connections\. For more information, see [Requesting a quota increase ](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.
If you're connecting to a Windows Server 2022 instance, you might need to install version 2\.2\.2 or later of the `PSReadLine` module to ensure keyboard functionality while using PowerShell\. The following is an example command\.  

  ```
  Install-Module `
      -Name PSReadLine `
      -Repository PSGallery `
      -MinimumVersion 2.2.2
  ```

Because Fleet Manager uses Session Manager to connect to Windows instances using RDP, you must complete the prerequisites for Session Manager before using this feature\. Session Manager is a capability of AWS Systems Manager\. Session preferences in the AWS account and AWS Region are applied when connecting to your instances using RDP\. For information about setting up Session Manager, see [Setting up Session Manager](session-manager-getting-started.md)\.

**Note**  
If you log your Session Manager operations using Amazon Simple Storage Service \(Amazon S3\), you will encounter the following error in the `bucket_name/Port/stderr` directory\. This error, which is generated for each Remote Desktop connection, is expected behavior and can be safely ignored\.  

```
Setting up data channel with id SESSION_ID failed: failed to create websocket for datachannel with error: CreateDataChannel failed with no output or error: createDataChannel request failed: unexpected response from the service <BadRequest>
<ClientErrorMessage>Session is already terminated</ClientErrorMessage>
</BadRequest>
```

In addition to the required AWS Identity and Access Management \(IAM\) permissions for Systems Manager and Session Manager, the user or role you use to access the console must allow the following actions:
+ `ssm-guiconnect:CancelConnection`
+ `ssm-guiconnect:GetConnection`
+ `ssm-guiconnect:StartConnection`

The following are example IAM policies for connecting to instances using RDP with Fleet Manager\. Replace each *example resource placeholder* with your own information\.

## Standard policy<a name="standard-policy"></a>

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

## Policy for connecting to instances with specific tags<a name="tag-policy"></a>

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

## Policy for AWS IAM Identity Center \(successor to AWS Single Sign\-On\) users<a name="sso-policy"></a>

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

**To connect to instances using RDP with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance that you want to connect to using RDP\.

1. In the **Node actions** menu, choose **Connect with Remote Desktop**\.

1. Choose your preferred **Authentication type**\. If you choose **User credentials**, enter the user name and password for the Windows user account that you want to use when connecting to the instance\. If you choose **Key pair**, choose the **Browse local machine** option to browse your local machine and choose the PEM key associated with your instance, or copy and paste the contents into the empty field after choosing the **Paste key pair content** option\.

1. Select **Connect**\.