# Accessing the Red Hat Knowledgebase portal<a name="fleet-rhel"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to access the Knowledgebase portal if you are a Red Hat customer\. You are considered a Red Hat customer if you run Red Hat Enterprise Linux \(RHEL\) instances or use RHEL services on AWS\. The Knowledgebase portal includes binaries, and knowledge\-share and discussion forums for community support that are available only to Red Hat licensed customers\.

In addition to the required AWS Identity and Access Management \(IAM\) permissions for Systems Manager and Fleet Manager, the user or role you use to access the console must allow the `rhelkb:GetRhelURL` action to access the Knowledgebase portal\.

**To access the Red Hat Knowledgebase Portal**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the RHEL instance you want to use to connect to the Red Hat Knowledgebase Portal\.

1. Select the **Account management** dropdown\. Then choose **Access Red Hat Knowledgebase**\. You will then be redirected to the Red Hat Knowledgebase page\.

If you use RHEL on AWS to run fully supported RHEL workloads, you can also access the Red Hat Knowledgebase through Red Hat's website by using your AWS credentials\.