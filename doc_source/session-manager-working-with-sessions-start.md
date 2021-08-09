# Start a session<a name="session-manager-working-with-sessions-start"></a>

You can use the AWS Systems Manager console, the Amazon Elastic Compute Cloud \(Amazon EC2\) console, the AWS Command Line Interface \(AWS CLI\), or SSH to start a session\.

**Topics**
+ [Starting a session \(Systems Manager console\)](#start-sys-console)
+ [Starting a session \(Amazon EC2 console\)](#start-ec2-console)
+ [Starting a session \(AWS CLI\)](#sessions-start-cli)
+ [Starting a session \(SSH\)](#sessions-start-ssh)
+ [Starting a session \(port forwarding\)](#sessions-start-port-forwarding)
+ [Starting a session \(interactive and noninteractive commands\)](#sessions-start-interactive-commands)

## Starting a session \(Systems Manager console\)<a name="start-sys-console"></a>

You can use the AWS Systems Manager console to start a session with an instance in your account\.

**Note**  
Before you start a session, make sure that you have completed the setup steps for Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.

**To start a session \( Systems Manager console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Session Manager** in the navigation pane\.

1. Choose **Start session**\.

1. For **Target instances**, choose the option button to the left of the instance you want to connect to\.

   If an instance you want to connect to isn't in the list, or is listed but an error message reports, "The instance you selected isn't configured to use Session Manager," see [Instance not available or not configured for Session Manager](session-manager-troubleshooting.md#session-manager-troubleshooting-instances) for troubleshooting steps\.

1. Choose **Start session**\.

After the connection is made, you can run bash commands \(Linux and macOS\) or PowerShell commands \(Windows\) as you would through any other connection type\.

## Starting a session \(Amazon EC2 console\)<a name="start-ec2-console"></a>

You can use the Amazon Elastic Compute Cloud \(Amazon EC2\) console to start a session with an instance in your account\.

**Note**  
If you receive an error that you aren't authorized to perform one or more Systems Manager actions \(`ssm:command-name`, then you must contact your administrator for assistance\. Your administrator is the person that provided you with your user name and password\. Ask that person to update your policies to allow you to start sessions from the Amazon EC2 console\. If you're an administrator, see [Quickstart default IAM policies for Session Manager](getting-started-restrict-access-quickstart.md) for more information\.

**To start a session \(Amazon EC2 console\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**\.

1. Select the instance and choose **Connect**\.

1. For **Connection method**, choose **Session Manager**\.

1. Choose **Connect**\.

After the connection is made, you can run bash commands \(Linux and macOS\) or PowerShell commands \(Windows\) as you would through any other connection type\.

## Starting a session \(AWS CLI\)<a name="sessions-start-cli"></a>

Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

To start a session using the AWS CLI, run the following command\.

**Note**  
Before you start a session, make sure that you have completed the setup steps for Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

```
aws ssm start-session \
    --target instance-id
```

 *instance\-id* represents the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability, such as `i-02573cafcfEXAMPLE`\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

## Starting a session \(SSH\)<a name="sessions-start-ssh"></a>

To start a Session Manager SSH session, version 2\.3\.672\.0 or later of SSM Agent must be installed on the managed instance\.

**SSH connection requirements**  
Take note of the following requirements and limitations for session connections using SSH:
+ Your target instance must be configured to support SSH connections\. For more information, see [\(Optional\) Allow SSH connections through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.
+ You must use the user on the instance associated with the Privacy Enhanced Mail \(PEM\) certificate, not the `ssm-user` account that is used for other types of session connections\. For example, on EC2 instances for Linux and macOS, the default user is `ec2-user`\. For information about identifying the default user for each instance type, see [Get Information About Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-get-info-about-instance) in the *Amazon EC2 User Guide for Linux Instances*\.
+ Logging isn't available for Session Manager sessions that connect through port forwarding or SSH\. This is because SSH encrypts all session data, and Session Manager only serves as a tunnel for SSH connections\.

**Note**  
Before you start a session, make sure that you have completed the setup steps for Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.

To start a session using SSH, run the following command\.

```
ssh -i /path/my-key-pair.pem username@instance-id
```

 */path/my\-key\-pair\.pem* represents the path to the PEM certificate that is associated with the instance\. For example, for an EC2 instance, the key pair file you created or selected when you created the instance\.

 *username@instance\-id* represents the default user name for your instance type, and the instance ID, such as `ec2-user@i-02573cafcfEXAMPLE`\.

**Tip**  
When you start a session using SSH, you can copy local files to the target instance using the following command format\.  

```
scp -i /path/my-key-pair.pem /path/SampleFile.txt username@instance-id:~
```

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

## Starting a session \(port forwarding\)<a name="sessions-start-port-forwarding"></a>

To start a Session Manager port forwarding session, version 2\.3\.672\.0 or later of SSM Agent must be installed on the managed instance\.

**Note**  
Before you start a session, make sure that you have completed the setup steps for Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.  
To use the AWS CLI to run session commands, you must install the Session Manager plugin on your local machine\. For information, see [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.  
Depending on your operating system and command line tool, the placement of quotation marks can differ and escape characters might be required\.

To start a port forwarding session, run the following command from the CLI\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name AWS-StartPortForwardingSession \
    --parameters '{"portNumber":["80"], "localPortNumber":["56789"]}'
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name AWS-StartPortForwardingSession ^
    --parameters portNumber="3389",localPortNumber="56789"
```

------

 *instance\-id* represents the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability, such as `i-02573cafcfEXAMPLE`\.

*portNumber* represents the remote port on the instance where traffic should be redirected to, such as `3389` for connecting to a Windows instance using the Remote Desktop Protocol \(RDP\)\. If this parameter isn't specified, Session Manager assumes `80` as the default remote port\. 

*localPortNumber* represents the local port on the client where traffic should be redirected to, such as `56789`\. This value is what you enter when connecting to an instance using a client\. For example, **localhost:56789**\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

For more information about port forwarding sessions, see [Port Forwarding Using AWS Systems Manager Session Manager](http://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/) in the *AWS News Blog*\.

## Starting a session \(interactive and noninteractive commands\)<a name="sessions-start-interactive-commands"></a>

To start an interactive command session, run the following command:

**Note**  
Before you start a session, make sure that you have completed the setup steps for Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name CustomCommandSessionDocument \
    --parameters '{"logpath":["/var/log/amazon/ssm/amazon-ssm-agent.log"]}'
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name CustomCommandSessionDocument ^
    --parameters logpath="/var/log/amazon/ssm/amazon-ssm-agent.log"
```

------

 *instance\-id* represents the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability, such as `i-02573cafcfEXAMPLE`\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

**Related content**  
[Port Forwarding Using AWS Systems Manager Session Manager](http://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/) on the *AWS News Blog*\.