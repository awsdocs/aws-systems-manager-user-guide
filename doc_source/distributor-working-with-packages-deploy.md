# Install Packages<a name="distributor-working-with-packages-deploy"></a>

You can use the AWS Management Console or the AWS CLI to deploy packages to your AWS Systems Manager managed instances by using AWS Systems Manager Distributor\. You can currently deploy one version of one package per command\. You can choose to deploy a specific version or choose to always deploy the latest version of a package for deployment\.


| Preference | AWS Systems Manager Action | More Information | 
| --- | --- | --- | 
|  Install a package immediately\.  |  Run Command  |  [Installing a Package One Time \(Console\)](#distributor-deploy-pkg-console) or [Installing a Package One Time \(AWS CLI\)](#distributor-deploy-pkg-cli)  | 
|  Install a package on a schedule, so that the installation always includes the default version\.  |  State Manager  |  [Scheduling a Package Installation \(Console\)](#distributor-deploy-sm-pkg-console) or [Scheduling a Package Installation \(AWS CLI\)](#distributor-smdeploy-pkg-cli)  | 
|  Automatically install a package on new instances that have a specific tag or set of tags\. For example, installing the Amazon CloudWatch agent on new instances\.  |  State Manager  |  One way to do this is to apply tags to new instances, and then specify the tags as targets in your State Manager association\. State Manager automatically installs the package in an association on instances that have matching tags\. See [Create an Association That Uses Targets and Rate Controls \(Console\)](systems-manager-state-manager-targets-and-rate-controls.md#sysman-state-targets-console)\.  | 

**Topics**
+ [Installing a Package One Time \(Console\)](#distributor-deploy-pkg-console)
+ [Scheduling a Package Installation \(Console\)](#distributor-deploy-sm-pkg-console)
+ [Installing a Package One Time \(AWS CLI\)](#distributor-deploy-pkg-cli)
+ [Scheduling a Package Installation \(AWS CLI\)](#distributor-smdeploy-pkg-cli)

## Installing a Package One Time \(Console\)<a name="distributor-deploy-pkg-console"></a>

You can use the AWS Systems Manager console to install a package one time\. When you configure a one\-time installation, Distributor uses [AWS Systems Manager Run Command](execute-remote-commands.md) to perform the installation\.

**To install a package one time \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose the package that you want to install\.

1. To install the default package version, you can choose **Install package** from the package details page\. To install another version, choose the **Versions** tab, choose the version you want to install, and then choose **Install package**\.

1. In **Install package**, choose **Install one time**\.

   This command opens Systems Manager Run Command with the command document `AWS-ConfigureAWSPackage` and your Distributor package already selected\.

1. Keep **Install** as the value of **Action**\. For **Name**, enter the name of the package that you want to install\.

1. For **Version**, enter the **Version name** value of the package\. If you leave this field blank, Run Command installs the default version that you selected in Distributor\.

1. In **Targets**, choose target managed instances by specifying a tag key and values that are shared by the managed instances, or specify instances by any of seven attributes, including instance ID, platform, and SSM Agent version\.

1. You can use the advanced options to add comments about the installation, change **Concurrency** and **Error threshold** values in **Rate control**, specify output options, or configure Amazon Simple Notification Service \(Amazon SNS\) notifications\. For more information, see [Running Commands from the Console](https://docs.aws.amazon.com/systems-manager/latest/userguide/rc-console.html) in this guide\.

1. When you are ready to install the package, choose **Run**, and then choose **View results**\.

1. In the commands list, choose the `AWS-ConfigureAWSPackage` command that you ran\. If the command is still in progress, choose the refresh icon in the top\-right corner of the console\.

1. When the **Status** column shows **Success** or **Failed**, choose the **Output** tab\.

1. Choose **View output**\. The command output page shows the results of your command execution\.

## Scheduling a Package Installation \(Console\)<a name="distributor-deploy-sm-pkg-console"></a>

You can use the AWS Systems Manager console to schedule the installation of a package\. When you schedule package installation, Distributor uses [AWS Systems Manager State Manager](systems-manager-state.md) to perform the installation\.

**To schedule a package installation \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose the package that you want to install\.

1. To install the default package version, you can choose **Install package** from the package details page\. To install another version, choose the **Versions** tab, choose the version you want to install, and then choose **Install package**\. If you do not choose a version, Systems Manager installs the default version\.

1. In **Install package**, choose **Install on a schedule**\.

   This command opens Systems Manager State Manager to a new association that is created for you\.

1. In **Name**, enter a name \(for example, **Deploy\-test\-agent\-package**\)\. This is optional, but recommended\. Spaces aren't allowed in the name\.

1. In the **Command document** list, the document name `AWS-ConfigureAWSPackage` is already selected\. Verify that the **Package** value is set to the name of your package\. Verify that in **Parameters**, for **Operation**, **Install** is already selected\.

1. For **Targets**, choose **Selecting all managed instances in this account**, **Specifying tags**, or **Manually Selecting Instance**\. If you target resources by using tags, enter a tag key and a tag value in the fields provided\.

1. For **Specify schedule**, choose **On Schedule** to run the association on a regular schedule, or **No Schedule** to run the association once\. For more information about these options, see [Create an Association](sysman-state-assoc.md) in this guide\. Use the controls to create a `cron` or rate schedule for the association\.

1. For more information about advanced options, such as compliance severity, rate control, and output options, see [Create an Association](sysman-state-assoc.md) in this guide\. When you are finished editing your association, choose **Save Changes**\.

1. On the association's home page, choose **Apply association now**\.

   State Manager creates and immediately runs the association on the specified instances or targets\. For more information about the results of running associations, see [Create an Association](sysman-state-assoc.md) in this guide\.

## Installing a Package One Time \(AWS CLI\)<a name="distributor-deploy-pkg-cli"></a>

You can run send\-command in the AWS CLI to install a Distributor package one time\.

**To install a package one time \(AWS CLI\)**
+ Run the following command in the AWS CLI\.

  ```
  aws ssm send-command --document-name "AWS-ConfigureAWSPackage" --instance-ids "instance-IDs" --parameters '{"action":["Install"],"name":["package-name (in same account) or package-ARN (shared from different account)"]}'
  ```

  The following is an example\.

  ```
  aws ssm send-command --document-name "AWS-ConfigureAWSPackage" --instance-ids "i-00000000000000" --parameters '{"action":["Install"],"name":["ExamplePackage"]}'
  ```

For information about other options you can use with the send\-command command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*\.

## Scheduling a Package Installation \(AWS CLI\)<a name="distributor-smdeploy-pkg-cli"></a>

You can run create\-association in the AWS CLI to install a Distributor package on a schedule\. The value of `--name`, the document name, is always `AWS-ConfigureAWSPackage`\. The following command uses the key `InstanceIds` to specify target instances\.

```
aws ssm create-association --name "AWS-ConfigureAWSPackage" --parameters '{"action":["Install"],"name":["package-name (in same account) or package-ARN (shared from different account)"]}' --targets [{\"Key\":\"InstanceIds\",\"Values\":[\"instance-ID1\",\"instance-ID2\"]}]
```

The following is an example\.

```
aws ssm create-association --name "AWS-ConfigureAWSPackage" --parameters '{"action":["Install"],"name":["Test-ConfigureAWSPackage"]}' --targets [{\"Key\":\"InstanceIds\",\"Values\":[\"i-000100010000010\",\"i-020100010000011\"]}]
```

For information about other options you can use with the create\-association command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/create-association.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-association.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.