# Deploy AWS Config conformance packs<a name="quick-setup-cpack"></a>

A conformance pack is a collection of AWS Config rules and remediation actions\. With Quick Setup, you can deploy a conformance pack as a single entity in an account and an AWS Region or across an organization in AWS Organizations\. This helps you manage configuration compliance of your AWS resources at scale, from policy definition to auditing and aggregated reporting, by using a common framework and packaging model\. 

To deploy conformance packs, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**Note**  
You must enable AWS Config recording before deploying this configuration\. For more information, see [Conformance packs](https://docs.aws.amazon.com/config/latest/developerguide/conformance-packs.html) in the *AWS Config Developer Guide*\.

**To deploy conformance packs with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Conformance packs**, and then choose **Next**\.

1. In the **Configuration options** section, choose the conformance packs you want to deploy\.

1. In the **Targets** section, choose whether to deploy conformance packs to your entire organization, some AWS Regions, or the account you're currently logged in to\.

   If you choose **Entire organization**, continue to step 8\.

   If you choose **Custom**, continue to step 7\.

1. In the **Target Regions** section, select the check boxes of the Regions you want to deploy conformance packs to\.

1. Choose **Create**\.