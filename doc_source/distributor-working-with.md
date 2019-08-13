# Working with Distributor<a name="distributor-working-with"></a>

You can use the AWS Systems Manager console, AWS CLI, or AWS Tools for PowerShell to add, manage, or deploy packages in Distributor\. Before you add a package to Distributor:
+ Create and zip installable assets\.
+ \(Optional\) Create a JSON manifest file for the package\. This is not required to use the **Simple** package creation process in the Distributor console\. Simple package creation generates a JSON manifest file for you\.

  You can use the AWS Systems Manager console or a text or JSON editor to create the manifest file\.
+ Have an Amazon S3 bucket ready to store your installable assets or software\. If you are using the **Advanced** package creation process, upload your assets to the S3 bucket before you begin\.
**Note**  
You can delete or repurpose this bucket after you finish creating your package because Distributor moves the package contents to an internal Systems Manager bucket as part of the package creation process\.

AWS\-published packages are already packaged and ready for deployment\. To deploy an AWS\-published package to managed instances, see [Install Packages](distributor-working-with-packages-deploy.md)\.

**Topics**
+ [Create a Package](distributor-working-with-packages-create.md)
+ [Edit Package Permissions \(Console\)](distributor-working-with-packages-ep.md)
+ [Edit Package Tags \(Console\)](distributor-working-with-packages-tags.md)
+ [Add a Package Version to Distributor](distributor-working-with-packages-version.md)
+ [Install Packages](distributor-working-with-packages-deploy.md)
+ [Uninstall a Package](distributor-working-with-packages-uninstall.md)
+ [Delete a Package](distributor-working-with-packages-dpkg.md)