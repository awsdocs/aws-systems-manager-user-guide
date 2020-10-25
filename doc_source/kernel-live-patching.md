# Use Kernel Live Patching on Amazon Linux 2 instances<a name="kernel-live-patching"></a>

Kernel Live Patching for Amazon Linux 2 enables you to apply security vulnerability and critical bug patches to a running Linux kernel, without reboots or disruptions to running applications\. This allows you to benefit from improved service and application availability, while keeping your infrastructure secure and up to date\. Kernel Live Patching is supported on Amazon EC2 instances and [on\-premises virtual machines](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html) running Amazon Linux 2\.

For general information about Kernel Live Patching, see [Kernel Live Patching on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html) in the *Amazon EC2 User Guide for Linux Instances*\.

After you enable Kernel Live Patching on an Amazon Linux 2 instance, you can use Patch Manager to apply kernel live patches to the instance\. Using Patch Manager is an alternative to using existing yum workflows on the instance to apply the updates\.

**Before you begin**  
To use Patch Manager to apply kernel live patches to your Amazon Linux 2 instances, ensure your instances are based on the correct architecture and kernel version\. For information, see [Supported configurations and prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-prereq) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [About Kernel Live Patching and Patch Manager](#about-klp)
+ [How it works](#how-klp-works)
+ [Enabling Kernel Live Patching using Run Command](#enable-klp)
+ [Applying kernel live patches using Run Command](#install-klp)
+ [Disabling Kernel Live Patching using Run Command](#disable-klp)

## About Kernel Live Patching and Patch Manager<a name="about-klp"></a>

Updating the kernel version  
You do not need to reboot an instance after applying a kernel live patch update\. However, AWS provides kernel live patches for an Amazon Linux 2 kernel version for up to three months after its release\. After the three\-month period, you must update to a later kernel version to continue to receive kernel live patches\. We recommend using a maintenance window to schedule a reboot of your instance at least once every three months to prompt the kernel version update\.

Uninstalling kernel live patches  
Kernel live patches can't be uninstalled using Patch Manager\. Instead, you can disable Kernel Live Patching, which removes the RPM packages for the applied kernel live patches\. For more information, see [Disabling Kernel Live Patching using Run Command](#disable-klp)\.

Kernel compliance  
In some cases, installing all CVE fixes from live patches for the current kernel version can bring that kernel into the same compliance state that a newer kernel version would have\. When that happens, the newer version is reported as `Installed`, and the instance reported as `Compliant`\. No installation time is reported for newer kernel version, however\.

One kernel live patch, multiple CVEs  
If a kernel live patch addresses multiple CVEs, and those CVEs have various classification and severity values, only the highest classification and severity from among the CVEs is reported for the patch\. 

The remainder of this section describes how to use Patch Manager to apply kernel live patches to instances that meet these requirements\.

## How it works<a name="how-klp-works"></a>

AWS releases two types of kernel live patches for Amazon Linux 2: security updates and bug fixes\. To apply those types of patches, you use a patch baseline document that targets only the classifications and severities listed in the following table\.


| Classification | Severity | 
| --- | --- | 
| Security | Critical, Important | 
| Bugfix | All | 

You can create a custom patch baseline that targets only these patches, or use the predefined `AWS-AmazonLinux2DefaultPatchBaseline` patch baseline\. In other words, you can use `AWS-AmazonLinux2DefaultPatchBaseline` with Amazon Linux 2 instances on which Kernel Live Patching is enabled, and kernel live updates will be applied\.

**Note**  
The `AWS-AmazonLinux2DefaultPatchBaseline` configuration specifies a seven\-day waiting period after a patch is released before it is installed automatically\. If you don't want to wait seven days for kernel live patches to be auto\-approved, you can create and use a custom patch baseline\. In your patch baseline, you can specify no auto\-approval waiting period, or specify a shorter or longer one\. For more information, see [Working with custom patch baselines](sysman-patch-baseline-console.md)\.

We recommend the following strategy to patch your instances with kernel live updates:

1. Enable Kernel Live Patching on your Amazon Linux 2 instances\.

1. Use Run Command to run a `Scan` operation on your instances using the predefined `AWS-AmazonLinux2DefaultPatchBaseline` or a custom patch baseline that also targets only `Security` updates with severity classified as `Critical` and `Important`, and the `Bugfix` severity of `All`\. 

1. Open Systems Manager Compliance at [https://console\.aws\.amazon\.com/systems\-manager/compliance](https://console.aws.amazon.com/systems-manager/compliance) and review whether non\-compliance for patching is reported for any of the instances that were scanned\. If so, view the instance compliance details to determine whether any kernel live patches are missing from the instance\.

1. To install missing kernel live patches, use Run Command with the same patch baseline you specified before, but this time run an `Install` operation instead of a `Scan` operation\.

   Because kernel live patches are installed without the need to reboot, you can choose the `NoReboot` reboot option for this operation\. 
**Note**  
You can still reboot the instance if required for other types of patches installed on the instance, or if you want to update to a newer kernel\. In these cases, choose the `RebootIfNeeded` reboot option instead\.

1. Return to Systems Manager Compliance to verify that the kernel live patches were installed\.

## Enabling Kernel Live Patching using Run Command<a name="enable-klp"></a>

To enable Kernel Live Patching, you can either run `yum` commands on your instances or use Run Command and a custom Systems Manager document that you create\.

For information about enabling Kernel Live Patching by running `yum` commands directly on the instance, see [Enabling Kernel Live Patching](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-enable) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
When you enable Kernel Live Patching, if the kernel already running on the instance is *earlier* than `kernel-4.14.165-131.185.amzn2.x86_64` \(the minimum supported version\), the process installs the latest available kernel version and reboots the instance\. If the instance is already running `kernel-4.14.165-131.185.amzn2.x86_64` or later, the process doesn't install a newer version and doesn't reboot the instance\.

**To enable Kernel Live Patching using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the custom Systems Manager document `AWS-ConfigureKernelLivePatching`\.

1. In the **Command parameters** section, specify whether you want instances to reboot as part of this operation\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To enable Kernel Live Patching \(AWS CLI\)**
+ Run the following command on your local machine\.

------
#### [ Linux ]

  ```
  aws ssm send-command \
      --document-name "AWS-ConfigureKernelLivePatching" \
      --parameters "EnableOrDisable=Enable" \
      --targets "Key=instanceids,Values=instance-id"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-command ^
      --document-name "AWS-ConfigureKernelLivePatching" ^
      --parameters "EnableOrDisable=Enable" ^
      --targets "Key=instanceids,Values=instance-id"
  ```

------

  Replace *instance\-id* with the ID of the Amazon Linux 2 instance on which you want to enable the feature, such as i\-02573cafcfEXAMPLE\. To enable the feature on multiple instances, you can use either of the following formats\.
  + `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
  + `--targets "Key=tag:tag-key,Values=tag-value"`

  For information about other options you can use in the command, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.

## Applying kernel live patches using Run Command<a name="install-klp"></a>

To apply kernel live patches, you can either run `yum` commands on your instances or use Run Command and the Systems Manager document `AWS-RunPatchBaseline`\. 

For information about applying kernel live patches by running `yum` commands directly on the instance, see [Applying kernel live patches](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-apply) in the *Amazon EC2 User Guide for Linux Instances*\.

**To apply kernel live patches using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the Systems Manager document `AWS-RunPatchBaseline`\.

1. In the **Command parameters** section, do one of the following:
   + If you are checking whether new kernel live patches are available, for **Operation**, choose `Scan`\. For **Reboot Option**, if do not want your instances to reboot after this operation, choose `NoReboot`\. After the operation completes, you can check for new patches and compliance status in Systems Manager Compliance\.
   + If you checked patch compliance already and are ready to apply available kernel live patches, for **Operation**, choose `Install`\. For **Reboot Option**, if you do not want your instances to reboot after this operation, choose `NoReboot`\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To apply kernel live patches using Run Command \(AWS CLI\)**

1. To perform a `Scan` operation before checking your results in Systems Manager Compliance, run the following command from your local machine\.

------
#### [ Linux ]

   ```
   aws ssm send-command \
   	--document-name "AWS-RunPatchBaseline" \
   	--targets "Key=InstanceIds,Values=instance-id" \
   	--parameters '{"Operation":["Scan"],"RebootOption":["RebootIfNeeded"]}'
   ```

------
#### [ Windows ]

   ```
   aws ssm send-command ^
   	--document-name "AWS-RunPatchBaseline" ^
   	--targets "Key=InstanceIds,Values=instance-id" ^
   	--parameters {\"Operation\":[\"Scan\"],\"RebootOption\":[\"RebootIfNeeded\"]}
   ```

------

   For information about other options you can use in the command, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.

1. To perform an `Install` operation after checking your results in Systems Manager Compliance, run the following command from your local machine\.

------
#### [ Linux ]

   ```
   aws ssm send-command \
   	--document-name "AWS-RunPatchBaseline" \
   	--targets "Key=InstanceIds,Values=instance-id" \
   	--parameters '{"Operation":["Install"],"RebootOption":["NoReboot"]}'
   ```

------
#### [ Windows ]

   ```
   aws ssm send-command ^
   	--document-name "AWS-RunPatchBaseline" ^
   	--targets "Key=InstanceIds,Values=instance-id" ^
   	--parameters {\"Operation\":[\"Install\"],\"RebootOption\":[\"NoReboot\"]}
   ```

------

In both of the preceding commands, replace *instance\-id* with the ID of the Amazon Linux 2 instance on which you want to apply kernel live patches, such as i\-02573cafcfEXAMPLE\. To enable the feature on multiple instances, you can use either of the following formats\.
+ `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
+ `--targets "Key=tag:tag-key,Values=tag-value"`

For information about other options you can use in these commands, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.

## Disabling Kernel Live Patching using Run Command<a name="disable-klp"></a>

To disable Kernel Live Patching, you can either run `yum` commands on your instances or use Run Command and the custom Systems Manager document `AWS-ConfigureKernelLivePatching`\.

**Note**  
If you no longer need to use Kernel Live Patching, you can disable it at any time\. In most cases, disabling the feature is not necessary\.

For information about disabling Kernel Live Patching by running `yum` commands directly on the instance, see [Enabling Kernel Live Patching](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-enable) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
When you disable Kernel Live Patching, the process uninstalls the Kernel Live Patching plugin and then reboots the instance\.

**To disable Kernel Live Patching using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the Systems Manager document `AWS-ConfigureKernelLivePatching`\.

1. In the **Command parameters** section, specify values for required parameters\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To disable kernel live patching \(AWS CLI\)**
+ Run a command similar to the following\.

------
#### [ Linux ]

  ```
  aws ssm send-command \
      --document-name "AWS-ConfigureKernelLivePatching" \
      --targets "Key=instanceIds,Values=instance-id" \
      --parameters "EnableOrDisable=Disable"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-command ^
      --document-name "AWS-ConfigureKernelLivePatching" ^
      --targets "Key=instanceIds,Values=instance-id" ^
      --parameters "EnableOrDisable=Disable"
  ```

------

  Replace *instance\-id* with the ID of the Amazon Linux 2 instance on which you want to disable the feature, such as i\-02573cafcfEXAMPLE\. To disable the feature on multiple instances, you can use either of the following formats\.
  + `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
  + `--targets "Key=tag:tag-key,Values=tag-value"`

  For information about other options you can use in the command, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.