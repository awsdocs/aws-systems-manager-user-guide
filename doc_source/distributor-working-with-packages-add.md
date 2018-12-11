# Add a Package to Distributor<a name="distributor-working-with-packages-add"></a>

You can use the AWS Systems Manager console or the AWS CLI to add a new package to AWS Systems Manager Distributor\. When you add a package, you are adding a new [SSM document](sysman-ssm-docs.md)\. The document lets you deploy the package to managed instances\.

**Topics**
+ [Add a Package \(Console\)](#create-pkg-console)
+ [Add a Package \(CLI\)](#create-pkg-cli)

## Add a Package \(Console\)<a name="create-pkg-console"></a>

You can use the AWS Systems Manager console to create a package\. Have the URL ready from the bucket to which you uploaded your package in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](distributor-working-with-packages-create.md#packages-upload-s3)\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose **Add package**\.

1. On the **Add package** page, enter a name for your package\. Package names can contain letters, numbers, periods, dashes, and underscores\. The name should be generic enough to apply to all versions of the package attachments, but specific enough to identify the purpose of the package\.

1. In **Version name**, enter the exact value of the `version` entry in your manifest file\.

1. \(Optional\) If you did not upload your JSON manifest to the S3 bucket where you stored your ZIP files, you can author or paste the entire JSON manifest in the **JSON manifest** field\. For more information about how to create the JSON manifest, see [Step 2: Create the JSON Package Manifest](distributor-working-with-packages-create.md#packages-manifest)\.

1. In **Package location**, paste the URL that you copied when you uploaded your package content to S3, and then choose **Add package**\.

## Add a Package \(CLI\)<a name="create-pkg-cli"></a>

1. To use the AWS CLI to create a package, run the following command, replacing *package\_name* with the name of your package and *S3\_bucket\_URL\_to\_manifest\_file* with the URL of the JSON manifest that you copied in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](distributor-working-with-packages-create.md#packages-upload-s3)\. *S3\_bucket\_URL\_of\_package* is the URL of the S3 bucket where the entire package is stored\. When you run the create\-document command in Distributor, you specify the `Package` value for `--document-type`\.

   If you did not add your manifest file to the S3 bucket, the `--content` parameter value is the entire content of the JSON manifest file, in quotations\.

   ```
   aws ssm create-document --name "package_name" --content "S3_bucket_URL_to_manifest_file" --attachments Key="SourceUrl",Values="S3_bucket_URL_of_package" --version-name version_value_from_manifest --document-type Package
   ```

   The following is an example\.

   ```
   aws ssm create-document --name ExamplePackage --content "https://s3.amazonaws.com/mybucket/ExamplePackage/manifest.json" --attachments Key="SourceUrl",Values="https://s3.amazonaws.com/mybucket/ExamplePackage" --version-name 1.0.1 --document-type Package
   ```

1. Verify that your package was added and show the package manifest by running the following command, replacing *package\_name* with the name of your package\. To get a specific version of the document \(not the same as the version of a package\), you can add the `--document-version` parameter\.

   ```
   aws ssm get-document --name "package_name"
   ```

For information about other options you can use with the create\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/create-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-document.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*\. For information about other options you can use with the get\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/get-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-document.html)\.