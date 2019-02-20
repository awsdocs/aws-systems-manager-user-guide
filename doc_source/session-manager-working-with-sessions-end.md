# Terminate a Session<a name="session-manager-working-with-sessions-end"></a>

You can use the AWS Systems Manager console or the AWS CLI to terminate a session that you started to connect to an instance in your account\. By default, if there is no user activity after 20 minutes, a session is terminated\. After a session is terminated, it can't be resumed\. 

**Topics**
+ [Terminating a Session \(Console\)](#stop-sys-console)
+ [Terminating a Session \(AWS CLI\)](#stop-cli)

## Terminating a Session \(Console\)<a name="stop-sys-console"></a>

You can use the AWS Systems Manager console to terminate a session with an instance in your account\.

**To terminate a session \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. For **Sessions**, choose the option button to the left of the session you want to terminate\.

1. Choose **Terminate**\.

## Terminating a Session \(AWS CLI\)<a name="stop-cli"></a>

To terminate a session using the AWS CLI, run the following command:

**Note**  
To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

```
aws ssm terminate-session --session-id session-id
```

*session\-id* represents of the ID of an active Session Manager session that you want to end permanently\.

For more information about the terminate\-session command, see [terminate\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/terminate-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.