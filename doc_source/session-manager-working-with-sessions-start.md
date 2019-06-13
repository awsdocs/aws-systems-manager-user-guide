# Start a Session<a name="session-manager-working-with-sessions-start"></a>

You can use the AWS Systems Manager console or the AWS CLI to start a session\.

**Topics**
+ [Starting a Session \(Console\)](#start-sys-console)
+ [Starting a Session \(AWS CLI\)](#sessions-start-cli)

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

*instance\-id* represents of the ID of an instance configured for use with AWS Systems Manager and its Session Manager capability\.

For information about other options you can use with the start\-session command, see [start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.