# Viewing compliance information<a name="application-manager-working-viewing-resource-compliance"></a>

The AWS Systems Manager Application Manager **Configurations** page displays [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/) resource and configuration rule compliance information\. This page also displays AWS Systems Manager [State Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html) association compliance information\. You can choose a resource, a rule, or an association to open the corresponding console for more information\. This page displays compliance information from the last 90 days\.

**Actions you can perform on this page**  
You can perform the following actions on this page:
+ Choose a resource name to open the AWS Config console where you can view compliance information about a selected resource\.
+ Choose the option button beside a resource name\. Then, choose the **Resource timeline** button to open the AWS Config console where you can view compliance information about a selected resource\.
+ In the **Config rules compliance** section, you can do the following:
  + Choose a rule name to open the AWS Config console where you can view information about that rule\.
  + Choose **Add rules** to open the AWS Config console where you can create a rule\.
  + Choose the option button beside a rule name, choose **Actions**, and then choose **Manage remediation** to change the remediation action for a rule\.
  + Choose the option button beside a rule name, choose **Actions**, and then choose **Re\-evaluate** to have AWS Config run a compliance check on the selected rule\.
+ In the **Association compliance** section, you can do the following:
  + Choose an association name to open the **Associations** page where you can view information about that association\.
  + Choose **Create association** to open Systems Manager State Manager where you can create an association\.
  + Choose the option button beside an association name and choose **Apply association** to immediately start all actions specified in the association\.

**To open the **Configurations** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose the **Configurations** tab\.