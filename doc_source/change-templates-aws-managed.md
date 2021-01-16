# Try out the AWS managed Hello World change template<a name="change-templates-aws-managed"></a>

You can use the sample change template `AWS-HelloWorldChangeTemplate`, which uses the sample Automation document `AWS-HelloWorld`, to test the review and approval process after you have finished setting up Change Manager\. This template is designed for testing or verifying your configured permissions, approver assignments, and approval process\. Approval to use this change template in your organization or account has already been provided by AWS\. Any change request based on this change template, however, must still be approved by reviewers in your organization or account\.

Rather than make changes to a resource, the result of the runbook workflow associated with this template is to print a message in the output of an Automation step\.

**Before you begin**  
Before you begin, ensure you have completed the following tasks:
+ If you are using AWS Organizations to manage change across an organization, complete the organization setup tasks described in [Setting up Change Manager for an organization \(management account\)](change-manager-organization-setup.md)\.
+ Configure Change Manager for your delegated administrator account or single account, as described in [Configuring Change Manager options and best practices](change-manager-account-setup.md)\. 
**Note**  
If you have enabled the best practice option **Require monitors for all templates** in your Change Manager settings, disable it temporarily while you test the Hello World change template\.

**To try out the AWS managed Hello World change template**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose **Create request**\.

1. Choose the change template named `AWS-HelloWorldChangeTemplate`, and then choose **Next**\.

1. For **Name**, enter a name for the change request that makes its purpose easy to identify, such as **MyChangeRequestTest**\.

1. For the remainder of the steps to create your change request, see [Creating change requests](change-requests-create.md)\.

**Next steps**  
For information about approving change requests, see [Reviewing and approving or rejecting change requests](change-requests-review.md)\.

To view the status and results of your change request, choose the name of your change request on the **Requests** tab in Change Manager\. 