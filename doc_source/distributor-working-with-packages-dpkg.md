# Delete a package<a name="distributor-working-with-packages-dpkg"></a>

## Deleting a package \(console\)<a name="distributor-delete-pkg-console"></a>

You can use the AWS Systems Manager console to delete a package from Distributor\. Deleting a package deletes all versions of a package from Distributor\.

**To delete a package \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the **Distributor** home page, choose the package that you want to delete\.

1. On the package's details page, choose **Delete package**\.

1. When you are prompted to confirm the deletion, choose **Delete package**\.

## Deleting a package \(AWS CLI\)<a name="distributor-delete-pkg-cli"></a>

You can use the AWS CLI to delete a package from Distributor\.

**To delete a package \(AWS CLI\)**

1. Run the following command to list documents for specific packages\. In the results of this command, look for the package that you want to delete\.

   ```
   aws ssm list-documents --filters [{\"Key\":\"Name\",\"Values\":[\"package-name\",\"another-package-name\"]}]
   ```

1. Run the following command to delete a package version\. Replace *package\-name* with the package name\.

   ```
   aws ssm delete-document --name "package-name"
   ```

1. Run the list\-documents command again to verify that the package version was deleted\. The package version that you deleted should no longer be found\.

   ```
   aws ssm list-documents --filters [{\"Key\":\"Name\",\"Values\":[\"package-name\",\"another-package-name\"]}]
   ```

For information about other options you can use with the list\-documents command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/list-documents.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-documents.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*\. For information about other options you can use with the delete\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/delete-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/delete-document.html)\.