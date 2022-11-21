# Tagging managed nodes<a name="tagging-managed-instances"></a>

The topics in this section describe how to work with tags on managed nodes\.

A managed node is any machine configured for AWS Systems Manager\. This includes Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers or virtual machines \(VMs\) in a hybrid environment that are configured for Systems Manager\.

The instructions in this topic are applicable to any machine that is managed using Systems Manager\.

**Topics**
+ [Creating or activating managed nodes with tags](#tagging-managed-instances-new)
+ [Adding tags to existing managed nodes](#tagging-managed-instances-update)
+ [Removing tags from managed nodes](#tagging-managed-instances-remove)

## Creating or activating managed nodes with tags<a name="tagging-managed-instances-new"></a>

You can add tags to EC2 instances at the time you create them\. You can add tags to on\-premises servers and virtual machines \(VMs\) at the time you activate them\.

For information, see the following topics:
+ For EC2 instances, see [Tag your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\. \(Content applies to both EC2 instances for Linux and for Windows\)
+ For on\-premises servers and VMs, see [Create a managed\-node activation for a hybrid environment](sysman-managed-instance-activation.md)\.

## Adding tags to existing managed nodes<a name="tagging-managed-instances-update"></a>

You can add tags to managed nodes by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing managed node \(console\)](#tagging-managed-instances-update-console)
+ [Adding tags to an existing managed node \(command line\)](#tagging-managed-instances-update-command-line)

### Adding tags to an existing managed node \(console\)<a name="tagging-managed-instances-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the ID of the managed node to add tags to, and then choose the **Tags** tab\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. In the **Tags** section, choose **Edit**, and then add one or more key\-value tag pairs\.

1. Choose **Save**\.

### Adding tags to an existing managed node \(command line\)<a name="tagging-managed-instances-update-command-line"></a>

**To add tags to an existing managed node \(command line\)**

1. Using your preferred command line tool, run the following command to view the list of managed nodes that you can tag\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-instance-information
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-information
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMInstanceInformation
   ```

------

   Note the ID of a managed node that you want to tag\.
**Note**  
Machines that have been registered for use with Systems Manager in a hybrid environment begin with `mi-`, such as `mi-0471e04240EXAMPLE`\. EC2 instances have IDs that begin with `i-`, such as `i-02573cafcfEXAMPLE`\.

1. Run the following command to tag a managed node\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "ManagedInstance" \
       --resource-id "instance-id" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "ManagedInstance" ^
       --resource-id "instance-id" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ PowerShell ]

   ```
   $tag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   ```

   ```
   $tag.Key = "tag-key"
   ```

   ```
   $tag.Value = "tag-value"
   ```

   ```
   Add-SSMResourceTag `
       -ResourceType "ManagedInstance" `
       -ResourceId "instance-id" `
       -Tag $tag `
       -Force
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the managed node tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "ManagedInstance" \
       --resource-id "instance-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "ManagedInstance" ^
       --resource-id "instance-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "ManagedInstance" `
       -ResourceId "instance-id"
   ```

------

## Removing tags from managed nodes<a name="tagging-managed-instances-remove"></a>

You can use the Systems Manager console or the command line to remove tags from managed nodes\.

**Topics**
+ [Removing tags from managed nodes \(console\)](#tagging-managed-instances-remove-console)
+ [Removing tags from managed nodes \(command line\)](#tagging-managed-instances-remove-command-line)

### Removing tags from managed nodes \(console\)<a name="tagging-managed-instances-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the name of the managed node to remove tags from, and then choose the **Tags** tab\.

1. In the **Tags** section, choose **Edit**, and then choose **Remove** next to the tag pair you no longer need\.

1. Choose **Save**\.

### Removing tags from managed nodes \(command line\)<a name="tagging-managed-instances-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the managed nodes in your account\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-instance-information
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-information
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMInstanceInformation
   ```

------

   Note the name of a managed node from which you want to remove tags\.

1. Run the following command to remove tags from a managed node\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "ManagedInstance" \
       --resource-id "instance-id" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "ManagedInstance" ^
       --resource-id "instance-id" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceId "instance-id" `
       -ResourceType "ManagedInstance" `
       -TagKey "tag-key" `
       -Force
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the managed node tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "ManagedInstance" \
       --resource-id "instance-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "ManagedInstance" ^
       --resource-id "instance-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "ManagedInstance" `
       -ResourceId "instance-id"
   ```

------