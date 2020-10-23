# View packages<a name="distributor-view-packages"></a>

To view packages that are available for installation, you can use the AWS Systems Manager console or your preferred AWS command line tool\. To access Distributor, open the AWS Systems Manager console and choose **Distributor** in the left navigation pane\. You'll see all of the packages available to you\.

The following section describes how you can view Distributor packages using your preferred command line tool\.

## View packages \(command line\)<a name="distributor-view-packages-cmd"></a>

This section contains information about how you can use your preferred command line tool to view Distributor packages using the provided commands\.

------
#### [ Linux ]

**To view packages using the AWS CLI on Linux**
+ To view all packages, excluding shared packages, run the following command\.

  ```
  aws ssm list-documents \
      --filters Key=DocumentType,Values=Package
  ```
+ To view all packages owned by Amazon, run the following command\.

  ```
  aws ssm list-documents \
      --filters Key=DocumentType,Values=Package Key=Owner,Values=Amazon
  ```
+ To view all packages owned by third parties, run the following command\.

  ```
  aws ssm list-documents \
      --filters Key=DocumentType,Values=Package Key=Owner,Values=ThirdParty
  ```

------
#### [ Windows ]

**To view packages using the AWS CLI on Windows**
+ To view all packages, excluding shared packages, run the following command\.

  ```
  aws ssm list-documents ^
      --filters Key=DocumentType,Values=Package
  ```
+ To view all packages owned by Amazon, run the following command\.

  ```
  aws ssm list-documents ^
      --filters Key=DocumentType,Values=Package Key=Owner,Values=Amazon
  ```
+ To view all packages owned by third parties, run the following command\.

  ```
  aws ssm list-documents ^
      --filters Key=DocumentType,Values=Package Key=Owner,Values=ThirdParty
  ```

------
#### [ PowerShell ]

**To view packages using the Tools for PowerShell**
+ To view all packages, excluding shared packages, run the following command\.

  ```
  $filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
  $filter.Key = "DocumentType"
  $filter.Values = "Package"
  
  Get-SSMDocumentList `
      -Filters @($filter)
  ```
+ To view all packages owned by Amazon, run the following command\.

  ```
  $typeFilter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
  $typeFilter.Key = "DocumentType"
  $typeFilter.Values = "Package"
  
  $ownerFilter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
  $ownerFilter.Key = "Owner"
  $ownerFilter.Values = "Amazon"
  
  Get-SSMDocumentList `
      -Filters @($typeFilter,$ownerFilter)
  ```
+ To view all packages owned by third parties, run the following command\.

  ```
  $typeFilter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
  $typeFilter.Key = "DocumentType"
  $typeFilter.Values = "Package"
  
  $ownerFilter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
  $ownerFilter.Key = "Owner"
  $ownerFilter.Values = "ThirdParty"
  
  Get-SSMDocumentList `
      -Filters @($typeFilter,$ownerFilter)
  ```

------