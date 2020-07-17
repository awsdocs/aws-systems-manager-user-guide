# Uninstall a package<a name="distributor-working-with-packages-uninstall"></a>

You can use the AWS Management Console or the AWS CLI to uninstall Distributor packages from your AWS Systems Manager managed instances by using Run Command\. In this release, you can uninstall one version of one package per command\. You can uninstall a specific version or the default version\.

**Topics**
+ [Uninstalling a package \(console\)](#distributor-pkg-uninstall-console)
+ [Uninstalling a package \(AWS CLI\)](#distributor-pkg-uninstall-cli)

## Uninstalling a package \(console\)<a name="distributor-pkg-uninstall-console"></a>

You can use Run Command in the AWS Systems Manager console to uninstall a package one time\. Distributor uses [AWS Systems Manager Run Command](execute-remote-commands.md) to uninstall packages\.

**To uninstall a package \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

1. On the Run Command home page, choose **Run command**\.

1. Choose the `AWS-ConfigureAWSPackage` command document\.

1. From **Action**, choose **Uninstall** 

1. For **Name**, enter the name of the package that you want to uninstall\.

1. In **Targets**, choose target managed instances by specifying a tag key and values that are shared by the managed instances, or specify instances by any of seven attributes, including instance ID, platform, and SSM Agent version\.

1. You can use the advanced options to add comments about the operation, change **Concurrency** and **Error threshold** values in **Rate control**, specify output options, or configure Amazon SNS notifications\. For more information, see [Running Commands from the Console](https://docs.aws.amazon.com/systems-manager/latest/userguide/rc-console.html) in this guide\.

1. When you are ready to uninstall the package, choose **Run**, and then choose **View results**\.

1. In the commands list, choose the `AWS-ConfigureAWSPackage` command that you ran\. If the command is still in progress, choose the refresh icon in the top\-right corner of the console\.

1. When the **Status** column shows **Success** or **Failed**, choose the **Output** tab\.

1. Choose **View output**\. The command output page shows the results of your command execution\.

## Uninstalling a package \(AWS CLI\)<a name="distributor-pkg-uninstall-cli"></a>

You can use the AWS CLI to uninstall a Distributor package from managed instances by using Run Command\.

**To uninstall a package \(AWS CLI\)**
+ Run the following command in the AWS CLI\.

  ```
  aws ssm send-command \
      --document-name "AWS-ConfigureAWSPackage" \
      --instance-ids "instance-IDs" \
      --parameters '{"action":["Uninstall"],"name":["package-name (in same account) or package-ARN (shared from different account)"]}'
  ```

  The following is an example\.

  ```
  aws ssm send-command \
      --document-name "AWS-ConfigureAWSPackage" \
      --instance-ids "i-02573cafcfEXAMPLE" \
      --parameters '{"action":["Uninstall"],"name":["Test-ConfigureAWSPackage"]}'
  ```

For information about other options you can use with the send\-command command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the AWS Systems Manager section of the *AWS CLI Command Reference*\.