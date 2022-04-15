# End a session<a name="session-manager-working-with-sessions-end"></a>

You can use the AWS Systems Manager console or the AWS Command Line Interface \(AWS CLI\) to end a session that you started in your account\. If there is no user activity after 20 minutes, a session is ended\. After a session is ended, it can't be resumed\. 

**Topics**
+ [Ending a session \(console\)](#stop-sys-console)
+ [Ending a session \(AWS CLI\)](#stop-cli)

## Ending a session \(console\)<a name="stop-sys-console"></a>

You can use the AWS Systems Manager console to end a session in your account\.

**To end a session \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. For **Sessions**, choose the option button to the left of the session you want to end\.

1. Choose **Terminate**\.

## Ending a session \(AWS CLI\)<a name="stop-cli"></a>

To end a session using the AWS CLI, run the following command\. Replace *session\-id* with your own information\.

```
aws ssm terminate-session \
    --session-id session-id
```

For more information about the terminate\-session command, see [terminate\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/terminate-session.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.