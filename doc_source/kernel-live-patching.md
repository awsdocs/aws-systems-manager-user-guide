# Using Kernel Live Patching on Amazon Linux 2 managed nodes<a name="kernel-live-patching"></a>

Kernel Live Patching for Amazon Linux 2 allows you to apply security vulnerability and critical bug patches to a running Linux kernel without reboots or disruptions to running applications\. This allows you to benefit from improved service and application availability, while keeping your infrastructure secure and up to date\. Kernel Live Patching is supported on Amazon EC2 instances, AWS IoT Greengrass core devices, and [on\-premises virtual machines](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html) running Amazon Linux 2\.

For general information about Kernel Live Patching, see [Kernel Live Patching on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html) in the *Amazon EC2 User Guide for Linux Instances*\.

After you turn on Kernel Live Patching on an Amazon Linux 2 managed node, you can use Patch Manager, a capability of AWS Systems Manager, to apply kernel live patches to the managed node\. Using Patch Manager is an alternative to using existing yum workflows on the node to apply the updates\.

**Before you begin**  
To use Patch Manager to apply kernel live patches to your Amazon Linux 2 managed nodes, ensure your nodes are based on the correct architecture and kernel version\. For information, see [Supported configurations and prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-prereq) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [About Kernel Live Patching and Patch Manager](#about-klp)
+ [How it works](#how-klp-works)
+ [Turning on Kernel Live Patching using Run Command](#enable-klp)
+ [Applying kernel live patches using Run Command](#install-klp)
+ [Turning off Kernel Live Patching using Run Command](#disable-klp)

## About Kernel Live Patching and Patch Manager<a name="about-klp"></a>

Updating the kernel version  
You don't need to reboot a managed node after applying a kernel live patch update\. However, AWS provides kernel live patches for an Amazon Linux 2 kernel version for up to three months after its release\. After the three\-month period, you must update to a later kernel version to continue to receive kernel live patches\. We recommend using a maintenance window to schedule a reboot of your node at least once every three months to prompt the kernel version update\.

Uninstalling kernel live patches  
Kernel live patches can't be uninstalled using Patch Manager\. Instead, you can turn off Kernel Live Patching, which removes the RPM packages for the applied kernel live patches\. For more information, see [Turning off Kernel Live Patching using Run Command](#disable-klp)\.

Kernel compliance  
In some cases, installing all CVE fixes from live patches for the current kernel version can bring that kernel into the same compliance state that a newer kernel version would have\. When that happens, the newer version is reported as `Installed`, and the managed node reported as `Compliant`\. No installation time is reported for newer kernel version, however\.

One kernel live patch, multiple CVEs  
If a kernel live patch addresses multiple CVEs, and those CVEs have various classification and severity values, only the highest classification and severity from among the CVEs is reported for the patch\. 

The remainder of this section describes how to use Patch Manager to apply kernel live patches to managed nodes that meet these requirements\.

## How it works<a name="how-klp-works"></a>

AWS releases two types of kernel live patches for Amazon Linux 2: security updates and bug fixes\. To apply those types of patches, you use a patch baseline document that targets only the classifications and severities listed in the following table\.


| Classification | Severity | 
| --- | --- | 
| Security | Critical, Important | 
| Bugfix | All | 

You can create a custom patch baseline that targets only these patches, or use the predefined `AWS-AmazonLinux2DefaultPatchBaseline` patch baseline\. In other words, you can use `AWS-AmazonLinux2DefaultPatchBaseline` with Amazon Linux 2 managed nodes on which Kernel Live Patching is turned on, and kernel live updates will be applied\.

**Note**  
The `AWS-AmazonLinux2DefaultPatchBaseline` configuration specifies a seven\-day waiting period after a patch is released before it's installed automatically\. If you don't want to wait seven days for kernel live patches to be auto\-approved, you can create and use a custom patch baseline\. In your patch baseline, you can specify no auto\-approval waiting period, or specify a shorter or longer one\. For more information, see [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)\.

We recommend the following strategy to patch your managed nodes with kernel live updates:

1. Turn on Kernel Live Patching on your Amazon Linux 2 managed nodes\.

1. Use Run Command, a capability of AWS Systems Manager, to run a `Scan` operation on your managed nodes using the predefined `AWS-AmazonLinux2DefaultPatchBaseline` or a custom patch baseline that also targets only `Security` updates with severity classified as `Critical` and `Important`, and the `Bugfix` severity of `All`\. 

1. Use Compliance, a capability of AWS Systems Manager, to review whether non\-compliance for patching is reported for any of the managed nodes that were scanned\. If so, view the node compliance details to determine whether any kernel live patches are missing from the managed node\.

1. To install missing kernel live patches, use Run Command with the same patch baseline you specified before, but this time run an `Install` operation instead of a `Scan` operation\.

   Because kernel live patches are installed without the need to reboot, you can choose the `NoReboot` reboot option for this operation\. 
**Note**  
You can still reboot the managed node if required for other types of patches installed on it, or if you want to update to a newer kernel\. In these cases, choose the `RebootIfNeeded` reboot option instead\.

1. Return to Compliance to verify that the kernel live patches were installed\.

## Turning on Kernel Live Patching using Run Command<a name="enable-klp"></a>

To turn on Kernel Live Patching, you can either run `yum` commands on your managed nodes or use Run Command and a custom Systems Manager document \(SSM document\) that you create\.

For information about turning on Kernel Live Patching by running `yum` commands directly on the managed node, see [Enable Kernel Live Patching](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-prereq) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
When you turn on Kernel Live Patching, if the kernel already running on the managed node is *earlier* than `kernel-4.14.165-131.185.amzn2.x86_64` \(the minimum supported version\), the process installs the latest available kernel version and reboots the managed node\. If the node is already running `kernel-4.14.165-131.185.amzn2.x86_64` or later, the process doesn't install a newer version and doesn't reboot the node\.

**To turn on Kernel Live Patching using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the custom SSM document `AWS-ConfigureKernelLivePatching`\.

1. In the **Command parameters** section, specify whether you want managed nodes to reboot as part of this operation\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To turn on Kernel Live Patching \(AWS CLI\)**
+ Run the following command on your local machine\.

------
#### [ Linux & macOS ]

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

  Replace *instance\-id* with the ID of the Amazon Linux 2 managed node on which you want to turn on the feature, such as i\-02573cafcfEXAMPLE\. To turn on the feature on multiple managed nodes, you can use either of the following formats\.
  + `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
  + `--targets "Key=tag:tag-key,Values=tag-value"`

  For information about other options you can use in the command, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.

## Applying kernel live patches using Run Command<a name="install-klp"></a>

To apply kernel live patches, you can either run `yum` commands on your managed nodes or use Run Command and the SSM document `AWS-RunPatchBaseline`\. 

For information about applying kernel live patches by running `yum` commands directly on the managed node, see [Apply kernel live patches](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-apply) in the *Amazon EC2 User Guide for Linux Instances*\.

**To apply kernel live patches using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the SSM document `AWS-RunPatchBaseline`\.

1. In the **Command parameters** section, do one of the following:
   + If you're checking whether new kernel live patches are available, for **Operation**, choose `Scan`\. For **Reboot Option**, if don't want your managed nodes to reboot after this operation, choose `NoReboot`\. After the operation is complete, you can check for new patches and compliance status in Compliance\.
   + If you checked patch compliance already and are ready to apply available kernel live patches, for **Operation**, choose `Install`\. For **Reboot Option**, if you don't want your managed nodes to reboot after this operation, choose `NoReboot`\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To apply kernel live patches using Run Command \(AWS CLI\)**

1. To perform a `Scan` operation before checking your results in Compliance, run the following command from your local machine\.

------
#### [ Linux & macOS ]

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

1. To perform an `Install` operation after checking your results in Compliance, run the following command from your local machine\.

------
#### [ Linux & macOS ]

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

In both of the preceding commands, replace *instance\-id* with the ID of the Amazon Linux 2 managed node on which you want to apply kernel live patches, such as i\-02573cafcfEXAMPLE\. To turn on the feature on multiple managed nodes, you can use either of the following formats\.
+ `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
+ `--targets "Key=tag:tag-key,Values=tag-value"`

For information about other options you can use in these commands, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.

## Turning off Kernel Live Patching using Run Command<a name="disable-klp"></a>

To turn off Kernel Live Patching, you can either run `yum` commands on your managed nodes or use Run Command and the custom SSM document `AWS-ConfigureKernelLivePatching`\.

**Note**  
If you no longer need to use Kernel Live Patching, you can turn it off at any time\. In most cases, turning off the feature isn't necessary\.

For information about turning off Kernel Live Patching by running `yum` commands directly on the managed node, see [Enable Kernel Live Patching](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html#al2-live-patching-enable) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
When you turn off Kernel Live Patching, the process uninstalls the Kernel Live Patching plugin and then reboots the managed node\.

**To turn off Kernel Live Patching using Run Command \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the SSM document `AWS-ConfigureKernelLivePatching`\.

1. In the **Command parameters** section, specify values for required parameters\.

1. For information about working with the remaining controls on this page, see [Running commands from the console](rc-console.md)\.

1. Choose **Run**\.

**To turn off Kernel Live Patching \(AWS CLI\)**
+ Run a command similar to the following\.

------
#### [ Linux & macOS ]

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

  Replace *instance\-id* with the ID of the Amazon Linux 2 managed node on which you want to turn off the feature, such as i\-02573cafcfEXAMPLE\. To turn off the feature on multiple managed nodes, you can use either of the following formats\.
  + `--targets "Key=instanceids,Values=instance-id1,instance-id2"`
  + `--targets "Key=tag:tag-key,Values=tag-value"`

  For information about other options you can use in the command, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.