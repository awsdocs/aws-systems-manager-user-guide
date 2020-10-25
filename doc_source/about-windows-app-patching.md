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

If you specify the Microsoft application product family, each product you specify must be a supported member of the selected product family\. For example, to patch the product "Active Directory Rights Management Services Client 2\.0," you must specify its product family as "Active Directory" and not, for example, "Office" or "SQL Server\." The following AWS CLI command demonstrates a match pairing of product family and product:

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

**Troubleshooting Mismatched Product Family/Product Pairs**  
When you create a patch baseline in the console, you specify a product family and a product\. For example, you might choose:
+ **Product family**: **Office**

  **Product**: **Office 2016**

If you attempt to create a patch baseline with a mismatched product family/product pair, an error message is displayed\. The following are reasons this can occur:
+ You selected a valid product family/product pair, but then removed the product family selection\.
+ You chose a product from the **Obsolete or mismatched options** sublist instead of the **Available and matching options** sublist\. 

  Items in the product **Obsolete or mismatched options** sublist might have been entered in error through an SDK or AWS CLI `create-patch-baseline` command\. This could mean a typo was introduced or a product was assigned to the wrong product family\. A product also appears in the **Obsolete or mismatched options** sublist if it was specified for a previous patch baseline but currently has no patches available from Microsoft\. 

  To avoid this issue in the console, always choose options from the **Currently available options** sublists\.

  You can also view the products that have currently available patches by using the `[describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html)` command in the AWS CLI or the `[DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)` API command\.