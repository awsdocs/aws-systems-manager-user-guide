# Working with Distributor<a name="distributor-working-with"></a>

You can use the AWS Systems Manager console, AWS command line tools \(AWS CLI and AWS Tools for PowerShell\), and AWS SDKs to add, manage, or deploy packages in Distributor\. Before you add a package to Distributor:
+ Create and zip installable assets\.
+ \(Optional\) Create a JSON manifest file for the package\. This is not required to use the **Simple** package creation process in the Distributor console\. Simple package creation generates a JSON manifest file for you\.

  You can use the AWS Systems Manager console or a text or JSON editor to create the manifest file\.
+ Have an S3 bucket ready to store your installable assets or software\. If you are using the **Advanced** package creation process, upload your assets to the S3 bucket before you begin\.
**Note**  
You can delete or repurpose this bucket after you finish creating your package because Distributor moves the package contents to an internal Systems Manager bucket as part of the package creation process\.

AWS\-published packages are already packaged and ready for deployment\. To deploy an AWS\-published package to managed instances, see [Install or update packages](distributor-working-with-packages-deploy.md)\.

**Topics**
+ [View packages](distributor-view-packages.md)
+ [Create a package](distributor-working-with-packages-create.md)
+ [Edit package permissions \(console\)](distributor-working-with-packages-ep.md)
+ [Edit package tags \(console\)](distributor-working-with-packages-tags.md)
+ [Add a package version to Distributor](distributor-working-with-packages-version.md)
+ [Install or update packages](distributor-working-with-packages-deploy.md)
+ [Uninstall a package](distributor-working-with-packages-uninstall.md)
+ [Delete a package](distributor-working-with-packages-dpkg.md)