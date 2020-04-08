# Edit package tags \(console\)<a name="distributor-working-with-packages-tags"></a>

After you have added a package to AWS Systems Manager Distributor, you can edit the package's tags in the AWS Systems Manager console\. These tags are applied to the package, and are not connected to tags on the instances to which you want to deploy the package\. Tags are case sensitive key and value pairs that can help you group and filter your packages by criteria that are relevant to your organization\. If you do not want to add tags, you are ready to install your package or add a new version\.

**To edit package tags \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the **Packages** page, choose the package for which you want to edit tags\.

1. On the **Package details** tab, in **Tags**, choose **Edit**\.

1. For **Add tags**, enter a tag key, or a tag key and value pair, and then choose **Add**\. Repeat if you want to add more tags\. To delete tags, choose **X** on the tag at the bottom of the window\.

1. When you are finished adding tags to your package, choose **Save**\.