# About patching applications released by Microsoft on Windows Server<a name="about-windows-app-patching"></a>

Use the information in this topic to help you prepare to patch applications on Windows Server using Patch Manager, a capability of AWS Systems Manager\.

**Microsoft application patching**  
Patching support for applications on Windows Server managed nodes is limited to applications released by Microsoft\.

**Note**  
In some cases, Microsoft releases patches for applications that might not specify explicitly an updated date and time\. In these cases, an updated date and time of `01/01/1970` is supplied by default\.

**Patch baselines to patch applications released by Microsoft**  
For Windows Server, three predefined patch baselines are provided\. The patch baselines `AWS-DefaultPatchBaseline` and `AWS-WindowsPredefinedPatchBaseline-OS` support only operating system updates on the Windows operating system itself\. `AWS-DefaultPatchBaseline` is used as the default patch baseline for Windows Server managed nodes unless you specify a different patch baseline\. The configuration settings in these two patch baselines are the same\. The newer of the two, `AWS-WindowsPredefinedPatchBaseline-OS`, was created to distinguish it from the third predefined patch baseline for Windows Server\. That patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported applications released by Microsoft\.

You can also create a custom patch baseline to update applications released by Microsoft on Windows Server machines\.

**Support for patching applications released by Microsoft on on\-premises servers, edge devices, VMs, and other non\-EC2 nodes**  
To patch applications released by Microsoft on virtual machines \(VMs\) and non\-EC2 managed nodes, you must turn on the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. **However, there is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.** For more information, see [ Configuring instance tiers  Learn the difference between standard\-instances tier and advanced\-instances tier for servers and VMs in your hybrid environment\.   This topic describes the scenarios when you must activate the advanced\-instanced tier\.  AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for on\-premises servers, edge devices, and virtual machines \(VMs\) in a hybrid environment\.  You can register up to 1,000 standard hybrid nodes per account per AWS Region at no additional cost\. \(Hybrid nodes are managed nodes of any other type than Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.\) However, registering more than 1,000 hybrid nodes requires that you activate the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\. Even with fewer than 1,000 registered hybrid nodes, two other scenarios require the advanced\-instances tier:    You want to use Session Manager to connect to non\-EC2 nodes\.   You want to patch applications \(not operating systems\) released by Microsoft on non\-EC2 nodes\.  There is no charge to patch applications released by Microsoft on Amazon EC2 instances\.     Advanced\-instances tier detailed scenarios  The following information provides details on the three scenarios for which you must activate the advanced\-instances tier\.  

Scenario 1: You want to register more than 1,000 hybrid nodes  
Using the standard\-instances tier, you can register a maximum of 1,000 non\-EC2 nodes \(on\-premises servers, edge devices, and VMs in a hybrid environment\) per AWS Region in a specific account without additional charge\. If you need to register more than 1,000 non\-EC2 nodes in a Region, you must use the advanced\-instances tier\. You can then activate as many managed servers, edge devices, and VMs in a hybrid environment as you want\. Charges for the advanced\-instances tier are based on the number of advanced instances activated as Systems Manager managed instances and the hours those instances are running\.  
All Systems Manager managed nodes that use the managed\-instance activation process described in [Create a managed\-node activation for a hybrid environment](sysman-managed-instance-activation.md) are then subject to charge if you exceed 1,000 on\-premises nodes in a Region in a specific account \.   
You can also activate existing Amazon Elastic Compute Cloud \(Amazon EC2\) instances using Systems Manager hybrid activations and work with them as non\-EC2 instances, such as for testing\. These would also qualify as hybrid nodes\. This isn't a common scenario\. 

Scenario 2: Patching Microsoft\-released applications on hybrid\-activated nodes  
The advanced\-instances tier is also required if you want to patch Microsoft\-released applications on non\-EC2 nodes \(on\-premises servers, edge devices, and VMs\) in a hybrid environment\. If you activate the advanced\-instances tier to patch Microsoft applications on non\-EC2 nodes, charges are then incurred for all on\-premises nodes, even if you have fewer than 1,000\.  
There is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information, see [About patching applications released by Microsoft on Windows Server](about-windows-app-patching.md)\. 

Scenario 3: Connecting to hybrid\-activated nodes using Session Manager  
Session Manager provides interactive shell access to your instances\. To connect to hybrid\-activated \(non\-EC2\) nodes using Session Manager, including on\-premises servers, edge devices, and VMs in a hybrid environment, you must activate the advanced\-instances tier\. Charges are then incurred for all hybrid\-activated nodes, even if you have fewer than 1,000\.   Summary: When do I need the advanced\-instances tier? Use the following table to review when you must use the advanced\-instances tier, and for which scenarios additional charges apply\.  


****  

| Scenario | Advanced\-instances tier required? | Additional charges apply? | 
| --- | --- | --- | 
|  The number of hybrid\-activated nodes in my Region in a specific account is more than 1,000\.  | Yes | Yes | 
|  I want to use Patch Manager to patch Microsoft\-released applications on any number of hybrid\-activated nodes, even less than 1,000\.  | Yes | Yes | 
|  I want to use Session Manager to connect to any number of hybrid\-activated nodes, even less than 1,000\.  | Yes | Yes | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managed-instances-tiers.html)  | No | No |     Turning on the advanced\-instances tier  Configure your hybrid environment to use the advanced\-instances tier using the Systems Manager console, AWS CLI, or Tools for Windows PowerShell\.   AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers, edge devices, and VMs in a hybrid environment\. The standard\-instances tier lets you register a maximum of 1,000 hybrid\-activated machines per AWS account per AWS Region\. The advanced\-instances tier is also required to use Patch Manager to patch Microsoft\-released applications on non\-EC2 nodes, and to connect to non\-EC2 nodes using Session Manager\. For more information, see [Configuring instance tiers](systems-manager-managed-instances-tiers.md)\. This section describes how to configure your hybrid environment to use the advanced\-instances tier\.  Before you begin Review pricing details for advanced instances\. Advanced instances are available on a per\-use\-basis\. For more information see, [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.    Configuring permissions to turn on the advanced\-instances tier  Verify that you have permission in AWS Identity and Access Management \(IAM\) to change your environment from the standard\-instances tier to the advanced\-instances tier\. You must either have the `AdministratorAccess` policy attached to your IAM user, group, or role, or you must have permission to change the Systems Manager activation\-tier service setting\. The activation\-tier setting uses the following API operations:    [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)   [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)   [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)   Use the following procedure to add an inline IAM policy to a user account\. This policy allows a user to view the current managed\-instance tier setting\. This policy also allows the user to change or reset the current setting in the specified AWS account and AWS Region\.  Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.  In the navigation pane, choose **Users**\.   In the list, choose the name of the user to embed a policy in\.   Choose the **Permissions** tab\.   On the right side of the page, under **Permission policies**, choose **Add inline policy**\.    Choose the **JSON** tab\.   Replace the default content with the following: 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:GetServiceSetting"
                   
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:ResetServiceSetting",
                   "ssm:UpdateServiceSetting"
               ],
               "Resource": "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
           }
       ]
   }
   ```   Choose **Review policy**\.   On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **Managed\-Instances\-Tier**\.   Choose **Create policy**\.   Administrators can specify read\-only permission by assigning the following inline policy to the user's account\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetServiceSetting"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ssm:ResetServiceSetting",
                "ssm:UpdateServiceSetting"
            ],
            "Resource": "*"
        }
    ]
}
``` For more information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.   Turning on the advanced\-instances tier \(console\)  The following procedure shows you how to use the Systems Manager console to change *all* on\-premises servers, edge devices, and virtual machines \(VMs\) that were added using managed\-instance activation, in the specified AWS account and AWS Region, to use the advanced\-instances tier\.  The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.  To turn on the advanced\-instances tier \(console\) Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. In the navigation pane, choose **Fleet Manager**\. \-or\- If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.  In the **Account management** menu, choose **Instance tier settings**\. If you don't see the **Settings** tab, then do the following:   Verify that the console is open in the AWS Region where you created your managed instances\. You can switch Regions by using the list in the top, right corner of the console\.    Verify that your instances meet Systems Manager requirements\. For information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.   For servers and VMs in a hybrid environment, verify that you completed the activation process\. For more information, see [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.     Choose **Change account settings**\.   Review the information in the pop\-up about changing account settings, and then, if you approve, choose the option to accept and continue\.   The system can take several minutes to complete the process of moving all instances from the standard\-instances tier to the advanced\-instances tier\.  For information about changing back to the standard\-instances tier, see [Reverting from the advanced\-instances tier to the standard\-instances tier](systems-manager-managed-instances-advanced-reverting.md)\.    Turning on the advanced\-instances tier \(AWS CLI\)  The following procedure shows you how to use the AWS Command Line Interface to change *all* on\-premises servers and VMs that were added using managed\-instance activation, in the specified AWS account and AWS Region, to use the advanced\-instances tier\.  The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.  To turn on the advanced\-instances tier using the AWS CLI  Open the AWS CLI and run the following command\. Replace each *example resource placeholder* with your own information\.   Linux & macOS  

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier \
       --setting-value advanced
   ```    Windows  

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier ^
       --setting-value advanced
   ```    There is no output if the command succeeds\.   Run the following command to view the current service settings for managed nodes in the current AWS account and AWS Region\.   Linux & macOS  

   ```
   aws ssm get-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```    Windows  

   ```
   aws ssm get-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```    The command returns information like the following\. 

   ```
   {
       "ServiceSetting": {
           "SettingId": "/ssm/managed-instance/activation-tier",
           "SettingValue": "advanced",
           "LastModifiedDate": 1555603376.138,
           "LastModifiedUser": "arn:aws:sts::123456789012:assumed-role/Administrator/User_1",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier",
           "Status": "PendingUpdate"
       }
   }
   ```     Turning on the advanced\-instances tier \(PowerShell\)  The following procedure shows you how to use the AWS Tools for Windows PowerShell to change *all* on\-premises servers and VMs that were added using managed\-instance activation, in the specified AWS account and AWS Region, to use the advanced\-instances tier\.  The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.  To turn on the advanced\-instances tier using PowerShell  Open AWS Tools for Windows PowerShell and run the following command\. Replace each *example resource placeholder* with your own information\. 

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier" `
       -SettingValue "advanced"
   ``` There is no output if the command succeeds\.   Run the following command to view the current service settings for managed nodes in the current AWS account and AWS Region\. 

   ```
   Get-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
   ``` The command returns information like the following\. 

   ```
   ARN:arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier
   LastModifiedDate : 4/18/2019 4:02:56 PM
   LastModifiedUser : arn:aws:sts::123456789012:assumed-role/Administrator/User_1
   SettingId        : /ssm/managed-instance/activation-tier
   SettingValue     : advanced
   Status           : PendingUpdate
   ```   The system can take several minutes to complete the process of moving all nodes from the standard\-instances tier to the advanced\-instances tier\.  For information about changing back to the standard\-instances tier, see [Reverting from the advanced\-instances tier to the standard\-instances tier](systems-manager-managed-instances-advanced-reverting.md)\.     Reverting from the advanced\-instances tier to the standard\-instances tier  Change hybrid instances running in the advanced\-instances tier back to the standard\-instances tier\.    This section describes how to change hybrid machines running in the advanced\-instances tier back to the standard\-instances tier\. This configuration applies to all hybrid machines in an AWS account and a single AWS Region\.  Before you begin Review the following important details\.     You can't revert back to the standard\-instance tier if you're running more than 1,000 hybrid instances in the account and Region\. You must first deregister hybrid instances until you have 1,000 or fewer\. This also applies to Amazon Elastic Compute Cloud \(Amazon EC2\) instances that use a Systems Manager on\-premises activation \(which isn't a common scenario\)\. For more information, see [Deregistering managed nodes in a hybrid environment](systems-manager-managed-instances-advanced-deregister.md)\.   After you revert, you won't be able to use Session Manager, a capability of AWS Systems Manager, to interactively access your hybrid instances\.   After you revert, you won't be able to use Patch Manager, a capability of AWS Systems Manager, to patch applications released by Microsoft on hybrid servers and virtual machines \(VMs\)\.   The process of reverting all hybrid machines back to the standard\-instance tier can take 30 minutes or more to complete\.    This section describes how to revert all hybrid machines in an AWS account and AWS Region from the advanced\-instances tier to the standard\-instances tier\.  Reverting to the standard\-instances tier \(console\)  The following procedure shows you how to use the Systems Manager console to change all on\-premises servers, edge devices, and virtual machines \(VMs\) in your hybrid environment to use the standard\-instances tier in the specified AWS account and AWS Region\. To revert to the standard\-instances tier \(console\) Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. In the navigation pane, choose **Fleet Manager**\. \-or\- If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.  Select the **Account settings** dropdown and choose **Instance tier settings**\.   Choose **Change account setting**\.   Review the information in the pop\-up about changing account settings, and then if you approve, choose the option to accept and continue\.     Reverting to the standard\-instances tier \(AWS CLI\)  The following procedure shows you how to use the AWS Command Line Interface to change all on\-premises servers, edge devices, and VMs in your hybrid environment to use the standard\-instances tier in the specified AWS account and AWS Region\. To revert to the standard\-instances tier using the AWS CLI  Open the AWS CLI and run the following command\. Replace each *example resource placeholder* with your own information\.   Linux & macOS  

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier \
       --setting-value standard
   ```    Windows  

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier ^
       --setting-value standard
   ```    There is no output if the command succeeds\.   Run the following command 30 minutes later to view the settings for managed instances in the current AWS account and AWS Region\.   Linux & macOS  

   ```
   aws ssm get-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```    Windows  

   ```
   aws ssm get-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```    The command returns information like the following\. 

   ```
   {
       "ServiceSetting": {
           "SettingId": "/ssm/managed-instance/activation-tier",
           "SettingValue": "standard",
           "LastModifiedDate": 1555603376.138,
           "LastModifiedUser": "System",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier",
           "Status": "Default"
       }
   }
   ``` The status changes to *Default* after the request has been approved\.     Reverting to the standard\-instances tier \(PowerShell\)  The following procedure shows you how to use AWS Tools for Windows PowerShell to change all on\-premises servers, edge devices, and VMs in your hybrid environment to use the standard\-instances tier in the specified AWS account and AWS Region\. To revert to the standard\-instances tier using PowerShell  Open AWS Tools for Windows PowerShell and run the following command\. 

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier" `
       -SettingValue "standard"
   ``` There is no output if the command succeeds\.   Run the following command 30 minutes later to view the settings for managed instances in the current AWS account and AWS Region\. 

   ```
   Get-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
   ``` The command returns information like the following\. 

   ```
   ARN: arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier
   LastModifiedDate : 4/18/2019 4:02:56 PM
   LastModifiedUser : System
   SettingId        : /ssm/managed-instance/activation-tier
   SettingValue     : standard
   Status           : Default
   ``` The status changes to *Default* after the request has been approved\.     ](systems-manager-managed-instances-tiers.md)\.

**Windows update option for "other Microsoft products"**  
In order for Patch Manager to be able to patch applications released by Microsoft on your Windows Server managed nodes, the Windows Update option **Give me updates for other Microsoft products when I update Windows** must be activated on the managed node\. 

For information about allowing this option on a single managed node, see [Update Office with Microsoft Update](https://support.microsoft.com/en-us/office/update-office-with-microsoft-update-f59d3f9d-bd5d-4d3b-a08e-1dd659cf5282) on the Microsoft Support website\.

For a fleet of managed nodes running Windows Server 2016 and later, you can use a Group Policy Object \(GPO\) to turn on the setting\. In the Group Policy Management Editor, go to **Computer Configuration**, **Administrative Templates**, **Windows Components**, **Windows Updates**, and choose **Install updates for other Microsoft products**\. We also recommend configuring the GPO with additional parameters that prevent unplanned automatic updates and reboots outside of Patch Manager\. For more information, see [Configuring Automatic Updates in a Non\-Active Directory Environment](https://docs.microsoft.com/de-de/security-updates/windowsupdateservices/18127499) on the Microsoft technical documentation website\.

For a fleet of managed nodes running Windows Server 2012 or 2012 R2 , you can turn on the option by using a script, as described in [Enabling and Disabling Microsoft Update in Windows 7 via Script](https://docs.microsoft.com/en-us/archive/blogs/technet/danbuche/enabling-and-disabling-microsoft-update-in-windows-7-via-script) on the Microsoft Docs Blog website\. For example, you could do the following:

1. Save the script from the blog post in a file\.

1. Upload the file to an Amazon Simple Storage Service \(Amazon S3\) bucket or other accessible location\.

1. Use Run Command, a capability of AWS Systems Manager, to run the script on your managed nodes using the Systems Manager document \(SSM document\) `AWS-RunPowerShellScript` with a command similar to the following\.

   ```
   Invoke-WebRequest `
       -Uri "https://s3.aws-api-domain/DOC-EXAMPLE-BUCKET/script.vbs" `
       -Outfile "C:\script.vbs" cscript c:\script.vbs
   ```

**Minimum parameter requirements**  
To include applications released by Microsoft in your custom patch baseline, you must, at a minimum, specify the product that you want to patch\. The following AWS Command Line Interface \(AWS CLI\) command demonstrates the minimal requirements to patch a product, such as Microsoft Office 2016\.

------
#### [ Linux & macOS ]

```
aws ssm create-patch-baseline \
    --name "My-Windows-App-Baseline" \
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=PRODUCT,Values='Office 2016'},{Key=PATCH_SET,Values='APPLICATION'}]},ApproveAfterDays=5}]"
```

------
#### [ Windows ]

```
aws ssm create-patch-baseline ^
    --name "My-Windows-App-Baseline" ^
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=PRODUCT,Values='Office 2016'},{Key=PATCH_SET,Values='APPLICATION'}]},ApproveAfterDays=5}]"
```

------

If you specify the Microsoft application product family, each product you specify must be a supported member of the selected product family\. For example, to patch the product "Active Directory Rights Management Services Client 2\.0," you must specify its product family as "Active Directory" and not, for example, "Office" or "SQL Server\." The following AWS CLI command demonstrates a matched pairing of product family and product\.

------
#### [ Linux & macOS ]

```
aws ssm create-patch-baseline \
    --name "My-Windows-App-Baseline" \
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=PRODUCT_FAMILY,Values='Active Directory'},{Key=PRODUCT,Values='Active Directory Rights Management Services Client 2.0'},{Key=PATCH_SET,Values='APPLICATION'}]},ApproveAfterDays=5}]"
```

------
#### [ Windows ]

```
aws ssm create-patch-baseline ^
    --name "My-Windows-App-Baseline" ^
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=PRODUCT_FAMILY,Values='Active Directory'},{Key=PRODUCT,Values='Active Directory Rights Management Services Client 2.0'},{Key=PATCH_SET,Values='APPLICATION'}]},ApproveAfterDays=5}]"
```

------

**Note**  
If you receive an error message about a mismatched product and family pairing, see [Issue: mismatched product family/product pairs](patch-manager-troubleshooting.md#patch-manager-troubleshooting-product-family-mismatch) for help resolving the issue\.