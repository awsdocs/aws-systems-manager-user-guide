# Troubleshooting Patch Manager<a name="patch-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Patch Manager\.

## Troubleshooting Mismatched Product Family/Product Pairs<a name="patch-manager-troubleshooting-product-family-mismatch"></a>

**Problem**: When you create a patch baseline in the console, you specify a product family and a product\. For example, you might choose:
+ **Product family**: **Office**

  **Product**: **Office 2016**

If you attempt to create a patch baseline with a mismatched product family/product pair, an error message is displayed\. The following are reasons this can occur:
+ You selected a valid product family/product pair, but then removed the product family selection\.
+ You chose a product from the **Obsolete or mismatched options** sublist instead of the **Available and matching options** sublist\. 

  Items in the product **Obsolete or mismatched options** sublist might have been entered in error through an SDK or AWS CLI `create-patch-baseline` command\. This could mean a typo was introduced or a product was assigned to the wrong product family\. A product also appears in the **Obsolete or mismatched options** sublist if it was specified for a previous patch baseline but currently has no patches available from Microsoft\. 
+ **Solution**: To avoid this issue in the console, always choose options from the **Currently available options** sublists\.

  You can also view the products that have currently available patches by using the `[describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html)` command in the AWS CLI or the `[DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)` API command\.