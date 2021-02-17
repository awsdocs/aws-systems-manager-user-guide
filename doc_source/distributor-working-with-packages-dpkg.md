# Delete a package<a name="distributor-working-with-packages-dpkg"></a>

## Deleting a package \(console\)<a name="distributor-delete-pkg-console"></a>

You can use the AWS Systems Manager console to delete a package from Distributor\. Deleting a package deletes all versions of a package from Distributor\.

**To delete a package \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the **Distributor** home page, choose the package that you want to delete\.

1. On the package's details page, choose **Delete package**\.

1. When you are prompted to confirm the deletion, choose **Delete package**\.

## Deleting a package version \(console\)<a name="distributor-delete-pkg-version-console"></a>

You can use the AWS Systems Manager console to delete a package version from Distributor\.

**To delete a package version \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the **Distributor** home page, choose the package that you want to delete a version of\.

1. On the versions page for the package, choose the version to delete and choose **Delete version**\.

1. When you are prompted to confirm the deletion, choose **Delete package version**\.

## Deleting a package \(command line\)<a name="distributor-delete-pkg-cli"></a>

You can use your preferred command line tool to delete a package from Distributor\.

------
#### [ Linux ]

**To delete a package \(AWS CLI\)**

1. Run the following command to list documents for specific packages\. In the results of this command, look for the package that you want to delete\.

   ```
   aws ssm list-documents \
       --filters Key=Name,Values=package-name
   ```

1. Run the following command to delete a package\. Replace *package\-name* with the package name\.

   ```
   aws ssm delete-document \
       --name "package-name"
   ```

1. Run the list\-documents command again to verify that the package was deleted\. The package you deleted should not appear in the list\.

   ```
   aws ssm list-documents \
       --filters Key=Name,Values=package-name
   ```

------
#### [ Windows ]

**To delete a package \(AWS CLI\)**

1. Run the following command to list documents for specific packages\. In the results of this command, look for the package that you want to delete\.

   ```
   aws ssm list-documents ^
       --filters Key=Name,Values=package-name
   ```

1. Run the following command to delete a package\. Replace *package\-name* with the package name\.

   ```
   aws ssm delete-document ^
       --name "package-name"
   ```

1. Run the list\-documents command again to verify that the package was deleted\. The package you deleted should not appear in the list\.

   ```
   aws ssm list-documents ^
       --filters Key=Name,Values=package-name
   ```

------
#### [ PowerShell ]

**To delete a package \(Tools for PowerShell\)**

1. Run the following command to list documents for specific packages\. In the results of this command, look for the package that you want to delete\.

   ```
   $filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
   $filter.Key = "Name"
   $filter.Values = "package-name"
   
   Get-SSMDocumentList `
       -Filters @($filter)
   ```

1. Run the following command to delete a package\. Replace *package\-name* with the package name\.

   ```
   Remove-SSMDocument `
       -Name "package-name"
   ```

1. Run the Get\-SSMDocumentList command again to verify that the package was deleted\. The package you deleted should not appear in the list\.

   ```
   $filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
   $filter.Key = "Name"
   $filter.Values = "package-name"
   
   Get-SSMDocumentList `
       -Filters @($filter)
   ```

------

## Deleting a package version \(command line\)<a name="distributor-delete-pkg-version-cli"></a>

You can use your preferred command line tool to delete a package version from Distributor\.

------
#### [ Linux ]

**To delete a package version \(AWS CLI\)**

1. Run the following command to list the versions of your package\. In the results of this command, look for the package version that you want to delete\.

   ```
   aws ssm list-document-versions \
       --name "package-name"
   ```

1. Run the following command to delete a package version\. Replace *package\-name* with the package name and *version* with the version number\.

   ```
   aws ssm delete-document \
       --name "package-name" \
       --document-version version
   ```

1. Run the list\-document\-versions command to verify that the version of the package was deleted\. The package version that you deleted should not be found\.

   ```
   aws ssm list-document-versions \
       --name "package-name"
   ```

------
#### [ Windows ]

**To delete a package version \(AWS CLI\)**

1. Run the following command to list the versions of your package\. In the results of this command, look for the package version that you want to delete\.

   ```
   aws ssm list-document-versions ^
       --name "package-name"
   ```

1. Run the following command to delete a package version\. Replace *package\-name* with the package name and *version* with the version number\.

   ```
   aws ssm delete-document ^
       --name "package-name" ^
       --document-version version
   ```

1. Run the list\-document\-versions command to verify that the version of the package was deleted\. The package version that you deleted should not be found\.

   ```
   aws ssm list-document-versions ^
       --name "package-name"
   ```

------
#### [ PowerShell ]

**To delete a package version \(Tools for PowerShell\)**

1. Run the following command to list the versions of your package\. In the results of this command, look for the package version that you want to delete\.

   ```
   Get-SSMDocumentVersionList `
       -Name "package-name"
   ```

1. Run the following command to delete a package version\. Replace *package\-name* with the package name and *version* with the version number\.

   ```
   Remove-SSMDocument `
       -Name "package-name" `
       -DocumentVersion version
   ```

1. Run the Get\-SSMDocumentVersionList command to verify that the version of the package was deleted\. The package version that you deleted should not be found\.

   ```
   Get-SSMDocumentVersionList `
       -Name "package-name"
   ```

------

For information about other options you can use with the list\-documents command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/list-documents.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-documents.html) in the AWS Systems Manager section of the *AWS CLI Command Reference*\. For information about other options you can use with the delete\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/delete-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/delete-document.html)\.