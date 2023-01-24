# Deploy Distributor packages with Quick Setup<a name="quick-setup-distributor"></a>

Distributor is a capability of AWS Systems Manager\. A Distributor package is a collection of installable software or assets that can be deployed as a single entity\. With Quick Setup, you can deploy a Distributor package in an AWS account and an AWS Region or across an organization in AWS Organizations\. Currently, only the Amazon Elastic File System \(Amazon EFS\) utilities package and Amazon CloudWatch agent can be deployed with Quick Setup\. For more information about Distributor, see [AWS Systems Manager Distributor](distributor.md)\.

To deploy Distributor packages, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To deploy Distributor packages with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Distributor**, and then choose **Next**\.

1. In the **Configuration options** section, choose the package you want to deploy\.

1. In the **Targets** section, choose whether to deploy the package to your entire organization, some of your organizational units \(OUs\), or the account you're currently logged in to\.

   If you choose **Entire organization**, continue to step 8\.

   If you choose **Custom**, continue to step 7\.

1. In the **Target OUs** section, select the check boxes of the OUs and Regions you want to deploy the package to\.

1. Choose **Create**\.