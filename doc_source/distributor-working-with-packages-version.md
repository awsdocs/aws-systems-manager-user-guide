# Add a Package Version to Distributor<a name="distributor-working-with-packages-version"></a>

To add a package version, [create a package and manifest](distributor-working-with-packages-create.md), upload it to Amazon S3, and then use Distributor to add a package version by adding an entry to the document that already exists for older versions\. To save time, update the manifest for an older version of the package, change the value of the `version` entry in the manifest \(for example, from `Test_1.0` to `Test_2.0`\) and save it as the manifest for the new version\. 

A new package version can:
+ Replace at least one of the ZIP files attached to the current version\.
+ Add new ZIP files to support additional platforms\.
+ Delete files to discontinue support for specific platforms\.

A newer version can use the same S3 bucket, but must have a URL with a different file name shown at the end\. You can use the AWS Systems Manager console or the AWS CLI to add the new version\.

**Topics**
+ [Adding a Package Version \(Console\)](#add-pkg-version)
+ [Adding a Package Version \(AWS CLI\)](#add-pkg-version-cli)

## Adding a Package Version \(Console\)<a name="add-pkg-version"></a>

Before you perform these steps, follow the instructions in [Create a Package](distributor-working-with-packages-create.md) to create a new package for the version\. Then, use the AWS Systems Manager console to add a new package version to Distributor\.

**To add a package version \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose the package to which you want to add another version\.

1. On the **Add package version** page, enter a description for the new package version\. Descriptions can be a maximum of 512 characters\.

1. \(Optional\) If you did not upload your JSON manifest to the S3 bucket where you stored your ZIP files, you can author or paste the entire JSON manifest in the **JSON manifest** field\. For more information about how to create the JSON manifest, see [Step 2: Create the JSON Package Manifest](distributor-working-with-packages-create.md#packages-manifest)\.

1. In **Version name**, enter the exact value that is in the `version` entry of your manifest file\.

1. In **Package location**, paste the URL that you copied when you uploaded your package to S3, and then choose **Add package version**\.

1. On the package's **Details** page, on the **Versions** tab, view the new version in the list of available package versions\. Set a default version of the package by choosing a version, and then choosing **Set default version**\.

   If you do not set a default version, the newest package version is the default version\.

## Adding a Package Version \(AWS CLI\)<a name="add-pkg-version-cli"></a>

You can use the AWS CLI to add a new package version to Distributor\. Before you run these commands, you must create a new package version and upload it to S3, as described at the start of this topic\.

**To add a package version \(AWS CLI\)**

1. Run the following command to edit the AWS Systems Manager document with an entry for a new package version\. Replace *document\-name* with the name of your document\. Replace *S3\-bucket\-URL\-to\-manifest\-file* with the URL of the JSON manifest that you copied in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](distributor-working-with-packages-create.md#packages-upload-s3)\. *S3\-bucket\-URL\-of\-package* is the URL of the S3 bucket where the entire package is stored\. Replace *version\-name\-from\-updated\-manifest* with the value of `version` in the manifest\. Set the `--document-version` parameter to `$LATEST` to make the document associated with this package version the latest version of the document\.

   ```
   aws ssm update-document --name "document-name" --content "S3-bucket-URL-to-manifest-file" --attachments Key="SourceUrl",Values="S3-bucket-URL-of-package" --version-name version-name-from-updated-manifest --document-version $LATEST
   ```

   The following is an example\.

   ```
   aws ssm update-document --name ExamplePackage --content "https://s3.amazonaws.com/mybucket/ExamplePackage/manifest.json" --attachments Key="SourceUrl",Values="https://s3.amazonaws.com/mybucket/ExamplePackage" --version-name 1.1.1 --document-version $LATEST
   ```

1. Run the following command to verify that your package was updated and show the package manifest\. Replace *package\-name* with the name of your package, and optionally, *document\-version* with the version number of the document \(not the same as the package version\) that you updated\. If this package version is associated with the latest version of the document, you can specify `$LATEST` for the value of the optional `--document-version` parameter\.

   ```
   aws ssm get-document --name "package-name" --document-version "document-version"
   ```

For information about other options you can use with the update\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/update-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/update-document.html) in the AWS Systems Manager section of the AWS CLI Command Reference\.