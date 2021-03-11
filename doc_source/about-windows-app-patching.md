# About patching applications on Windows Server<a name="about-windows-app-patching"></a>

For Windows Server, two predefined patch baselines are provided\. The patch baseline `AWS-WindowsPredefinedPatchBaseline-OS` supports only operating system updates on the Windows operating system itself\. It is used as the default patch baseline for Windows Server instances unless you specify a different patch baseline\. The other predefined Windows patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported Microsoft applications\. 

**Note**  
Microsoft application patching is only available on EC2 instances and in the advanced\-instances tier\. To patch Microsoft applications on on\-premises servers and VMs, you must enable the advanced\-instances tier\. For more information, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

You can also create a custom patch baseline to update Microsoft applications on Windows Server machines\. 

To include Microsoft applications in your custom patch baseline, you must, at a minimum, specify the product that you want to patch\. The following AWS CLI command demonstrates the minimal requirements to patch a product, such as Office 2016:

------
#### [ Linux ]

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

If you specify the Microsoft application product family, each product you specify must be a supported member of the selected product family\. For example, to patch the product "Active Directory Rights Management Services Client 2\.0," you must specify its product family as "Active Directory" and not, for example, "Office" or "SQL Server\." The following AWS CLI command demonstrates a matched pairing of product family and product:

------
#### [ Linux ]

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