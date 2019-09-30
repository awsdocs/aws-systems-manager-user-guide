# Start a Session<a name="session-manager-working-with-sessions-start"></a>

You can use the AWS Systems Manager console, the AWS CLI, or SSH to start a session\.

**Topics**
+ [Starting a Session \(Console\)](#start-sys-console)
+ [Starting a Session \(AWS CLI\)](#sessions-start-cli)
+ [Starting a Session \(SSH\)](#sessions-start-ssh)
+ [Starting a Session \(Port Forwarding\)](#sessions-start-port-forwarding)
+ [Starting a Session \(Interactive Commands\)](#sessions-start-interactive-commands)

## Starting a Session \(Console\)<a name="start-sys-console"></a>

You can use the AWS Systems Manager console to start a session with an instance in your account\.

**To start a session \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Session Manager** in the navigation pane\.

1. Choose **Start session**\.

1. For **Target instances**, choose the option button to the left of the instance you want to connect to\.

   If an instance you want to connect to is not in the list, or is listed but an error message reports, "The instance you selected is not configured to use Session Manager," see [Instance Not Available or Not Configured for Session Manager](session-manager-troubleshooting.md#session-manager-troubleshooting-instances) for troubleshooting steps\.

1. Choose **Start session**\.

After the connection is made, you can run bash commands \(Linux\) or PowerShell commands \(Windows\) as you would through any other connection type\.

## Starting a Session \(AWS CLI\)<a name="sessions-start-cli"></a>

To start a session using the AWS CLI, run the following command:

**Note**  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

```
aws ssm start-session --target instance-id
```

 *instance\-id* represents of the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability\. For example: `i-02573cafcfEXAMPLE`\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

## Starting a Session \(SSH\)<a name="sessions-start-ssh"></a>

To start a session using SSH, run the following command:

**Note**  
To start a session using SSH, your target instance must be configured to support SSH connections\. For more information, see [\(Optional\) Enable SSH Connections Through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.

```
ssh -i /path/my-key-pair.pem username@instance-id
```

 */path/my\-key\-pair\.pem* represents the path to your Privacy Enhanced Mail \(PEM\) certificate\.

 *username@instance\-id* represents the user name you use to connect to the instance, and the instance ID\. For example: `JaneDoe@i-02573cafcfEXAMPLE`\.

**Tip**  
When you start a session using SSH, you can copy local files to the target instance using the following command format\.  

```
scp -i /path/my-key-pair.pem /path/SampleFile.txt username@instance-id:~
```

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

## Starting a Session \(Port Forwarding\)<a name="sessions-start-port-forwarding"></a>

To start a port forwarding session, run the following command from the CLI:

**Note**  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

```
aws ssm start-session --target instance-id --document-name AWS-StartPortForwardingSession --parameters '{"portNumber":["80"], "localPortNumber":["56789"]}'
```

 *instance\-id* represents of the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability\. For example: `i-02573cafcfEXAMPLE`\.

*portNumber* represents the remote port on the instance where traffic should be redirected to\. \. For example: 3389\. If this parameter is not specified, Session Manager assumes 80 as the default remote port\. 

*localPortNumber* represents the local port on the client where traffic should be redirected to\. For example: 56789\. 

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

## Starting a Session \(Interactive Commands\)<a name="sessions-start-interactive-commands"></a>

To start an Interactive Command session, run the following command::

**Note**  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

```
aws ssm start-session --target instance-id --document-name TestInteractiveCommandSessionDocument --parameters '{"logpath":["/var/log/amazon/ssm/amazon-ssm-agent.log"]}'
```

 *instance\-id* represents of the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability\. For example: `i-02573cafcfEXAMPLE`\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.

**Related Content**  
[Port Forwarding Using AWS Systems Manager Session Manager](http://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/) on the *AWS News Blog*\.