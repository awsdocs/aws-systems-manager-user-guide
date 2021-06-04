# About patching applications released by Microsoft on Windows Server<a name="about-windows-app-patching"></a>

Use the information in this topic to help you prepare to patch applications on Windows Server using Patch Manager, a capability of AWS Systems Manager\.

**Microsoft application patching**  
Patching support for applications on Windows Server managed instances is limited to applications released by Microsoft\.

**Patch baselines for patching applications released by Microsoft**  
For Windows Server, three predefined patch baselines are provided\. The patch baselines `AWS-DefaultPatchBaseline` and `AWS-WindowsPredefinedPatchBaseline-OS` support only operating system updates on the Windows operating system itself\. `AWS-DefaultPatchBaseline` is used as the default patch baseline for Windows Server instances unless you specify a different patch baseline\. The configuration settings in these two patch baselines are the same\. The newer of the two, `AWS-WindowsPredefinedPatchBaseline-OS`, was created to distinguish it from the third predefined patch baseline for Windows Server\. That patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported applications released by Microsoft\.

You can also create a custom patch baseline to update applications released by Microsoft on Windows Server machines\.

**Support for patching applications released by Microsoft on virtual machines \(VMs\) and on\-premises instances**  
To patch applications released by Microsoft on virtual machines \(VMs\) and on\-premises instances, you must enable the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. There is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

**Windows update option for "other Microsoft products"**  
In order for Patch Manager to be able to patch applications released by Microsoft on your Windows Server managed instances, the Windows Update option **Give me updates for other Microsoft products when I update Windows** must be enabled on the instance\.

For information about enabling this option on a single instance, see [Update Office with Microsoft Update](https://support.microsoft.com/en-us/office/update-office-with-microsoft-update-f59d3f9d-bd5d-4d3b-a08e-1dd659cf5282) on the Microsoft Support website\.

For a fleet of instances running Windows Server 2016 and later, you can use a Group Policy Object \(GPO\) to enable the setting\. In the Group Policy Management Editor, go to **Computer Configuration**, **Administrative Templates**, **Windows Components**, **Windows Updates**, and choose **Install updates for other Microsoft products**\.

For a fleet of instances running Windows Server 2012 or 2012 R2 , you can enable the option by using a script, as described in [Enabling and Disabling Microsoft Update in Windows 7 via Script](https://docs.microsoft.com/en-us/archive/blogs/technet/danbuche/enabling-and-disabling-microsoft-update-in-windows-7-via-script) on the Microsoft Docs Blog website\. For example, you could do the following:

1. Save the script from the blog post in a file\.

1. Upload the file to an Amazon Simple Storage Service \(Amazon S3\) bucket or other accessible location\.

1. Use Run Command, a capability of AWS Systems Manager, to run the script on your instances using the Systems Manager document \(SSM document\) `AWS-RunPowerShellScript` with a command similar to the following\.

   ```
   Invoke-WebRequest `
       -Uri "http://s3.amazonaws.com/DOC-EXAMPLE-BUCKET/script.vbs" `
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
If you receive an error message about a mismatched product and family pairing, see [Troubleshooting mismatched product family/product pairs](patch-manager-troubleshooting.md#patch-manager-troubleshooting-product-family-mismatch) for help resolving the issue\.