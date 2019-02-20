# Working with Distributor<a name="distributor-working-with"></a>

You can use the AWS Systems Manager console, AWS CLI, or AWS Tools for PowerShell to add, manage, or deploy packages in Distributor\. Before you add a package to Distributor:
+ Create and zip installable assets\.
+ Create a JSON manifest file for the package\.

  You can use the AWS Systems Manager console or a text or JSON editor to create the manifest file\.
+ Upload the package's installable assets to an Amazon S3 bucket\.

AWS\-published packages are already packaged and ready for deployment\. To deploy an AWS\-published package to managed instances, see [Install Packages](distributor-working-with-packages-deploy.md)\.

**Topics**
+ [Create a Package](distributor-working-with-packages-create.md)
+ [Add a Package to Distributor](distributor-working-with-packages-add.md)
+ [Edit Package Permissions \(Console\)](distributor-working-with-packages-ep.md)
+ [Edit Package Tags \(Console\)](distributor-working-with-packages-tags.md)
+ [Add a Package Version to Distributor](distributor-working-with-packages-version.md)
+ [Install Packages](distributor-working-with-packages-deploy.md)
+ [Uninstall a Package](distributor-working-with-packages-uninstall.md)
+ [Delete a Package](distributor-working-with-packages-dpkg.md)