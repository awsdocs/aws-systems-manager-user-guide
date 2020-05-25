# Tagging managed instances<a name="tagging-managed-instances"></a>

The topics in this section describe how to work with tags on managed instances\.

A managed instance is any machine configured for AWS Systems Manager\. This includes EC2 instances, as well as on\-premises servers or virtual machines \(VMs\) that you have configured to manage using Systems Manager in a hybrid environment\.

The instructions in this topic are applicable to any machine that is being managed using Systems Manager\.

**Topics**
+ [Creating or activating managed instances with tags](#tagging-managed-instances-new)
+ [Adding tags to existing managed instances](#tagging-managed-instances-update)
+ [Removing tags from managed instances](#tagging-managed-instances-remove)

## Creating or activating managed instances with tags<a name="tagging-managed-instances-new"></a>

You can add tags to EC2 instances at the time you create them\. You can add tags to on\-premises servers and virtual machines \(VMs\) at the time you activate them\.

For information, see the following topics:
+ For EC2 instances, see [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\. \(Content applies to both EC2 instances for Linux and for Windows\)
+ For on\-premises servers and VMs, see [Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\.

## Adding tags to existing managed instances<a name="tagging-managed-instances-update"></a>

You can add tags to managed instances by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing managed instance \(console\)](#tagging-managed-instances-update-console)
+ [Adding tags to an existing managed instance \(command line\)](#tagging-managed-instances-update-command-line)

### Adding tags to an existing managed instance \(console\)<a name="tagging-managed-instances-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose the name of the managed instance to add tags to, and then choose the **Tags** tab\.

1. In the **Tags** section, choose **Edit**, and then add one or more key\-value tag pairs\.

1. Choose **Save**\.

### Adding tags to an existing managed instance \(command line\)<a name="tagging-managed-instances-update-command-line"></a>

**To add tags to an existing managed instance \(command line\)**

1. Using your preferred command line tool, run the following command to view the list of managed instances that you can tag\.

------
#### [ Linux ]

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

   Note the ID of a managed instance that you want to tag\.
**Note**  
Machines that have been registered for use with Systems Manager in a hybrid environment begin with `mi-`, such as `mi-0471e04240EXAMPLE;`\. EC2 instances have IDs that begin with `i-`, such as `i-02573cafcfEXAMPLE`\.

1. Run the following command to tag a managed instance\.

------
#### [ Linux ]

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

   *tag\-key* is the name of a custom key you supply\. For example, *Region* or *Quarter*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *West* or *Q321*\.

   *instance\-id* is the ID of the managed instance you want to tag\.

   If successful, the command has no output\.

1. Run the following command to verify the managed instance tags\.

------
#### [ Linux ]

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

## Removing tags from managed instances<a name="tagging-managed-instances-remove"></a>

You can use the Systems Manager console or the command line to remove tags from managed instances\.

**Topics**
+ [Removing tags from managed instances \(console\)](#tagging-managed-instances-remove-console)
+ [Removing tags from managed instances \(command line\)](#tagging-managed-instances-remove-command-line)

### Removing tags from managed instances \(console\)<a name="tagging-managed-instances-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose the name of the managed instance to remove tags from, and then choose the **Tags** tab\.

1. In the **Tags** section, choose **Edit**, and then choose **Remove** next to the tag pair you no longer need\.

1. Choose **Save**\.

### Removing tags from managed instances \(command line\)<a name="tagging-managed-instances-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the managed instances in your account\.

------
#### [ Linux ]

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

   Note the name of a managed instance from which you want to remove tags\.

1. Run the following command to remove tags from a managed instance\.

------
#### [ Linux ]

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

   *instance\-id* is the name of the managed instance from which you want to remove tags\.

   *tag\-key* is the name of a key assigned to the managed instance\. For example, *Environment* or *Quarter*\.

   If successful, the command has no output\.

1. Run the following command to verify the managed instance tags\.

------
#### [ Linux ]

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