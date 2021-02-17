# Edit package permissions \(console\)<a name="distributor-working-with-packages-ep"></a>

After you add a package to AWS Systems Manager Distributor, you can edit the package's permissions in the AWS Systems Manager console\. You can add other AWS accounts to a package's permissions\. Packages can be shared with other accounts in the same AWS Region only\. Cross\-Region sharing is not supported\. By default, packages are set to **Private**, meaning only those with access to the package creator's AWS account can view package information and update or delete the package\. If **Private** permissions are acceptable, you can skip this procedure\.

**To edit package permissions \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the **Packages** page, choose the package for which you want to edit permissions\.

1. On the **Package details** tab, choose **Edit permissions** to change permissions\.

1. For **Edit permissions**, choose **Shared with specific accounts**\.

1. Under **Shared with specific accounts**, add AWS account numbers, one at a time\. When you are finished, choose **Save**\.