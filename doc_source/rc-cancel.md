# Canceling a Command<a name="rc-cancel"></a>

You can attempt to cancel a command as long as the service shows that it is in either a Pending or Executing state\. However, even if a command is still in one of these states, we cannot guarantee that the command will be terminated and the underlying process stopped\. 

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To cancel a command using the console \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Select the command invocation that you want to cancel\.

1. Choose **Cancel command**\.

**To cancel a command using the console \(Amazon EC2 Systems Manager\)**

1. In the navigation pane, choose **Run Command**\.

1. Select the command invocation that you want to cancel\.

1. Choose **Actions**, **Cancel Command**\.

**To cancel a command using the AWS CLI**  
Use the following command\.

```
aws ssm cancel-command --command-id "command  ID" --instance-ids "instance ID"
```

For information about the status of a cancelled command, see [Understanding Command Statuses](monitor-commands.md)\.