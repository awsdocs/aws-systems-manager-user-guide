# Working with change templates<a name="change-templates"></a>

A change template is a collection of configuration settings in Change Manager that define such things as required approvals, available runbooks, and notification options for change requests\.

**Note**  
AWS provides a sample [Hello World](change-templates-aws-managed.md) change template you can use to try out Change Manager, a capability of AWS Systems Manager\. However, you create your own change templates to define the changes you want to allow to the resources in your organization or account\. 

The changes that are made when a runbook workflow runs are based on the contents an Automation runbook\. In each change template you create, you can include one or more Automation runbooks that the user making a change request can choose from to run during the update\. You can also create change templates that allow requesters to choose any available Automation runbook for the change request\.

To create a change template, you can use the **Builder** option in the **Create template** console page to build a change template\. Alternatively, using the **Editor** option, you can manually author JSON or YAML content with the configuration you want for your runbook workflow\. You can also use a command line tool to create a change template, with JSON content for the change template stored in an external file\.

**Topics**
+ [Try out the AWS managed Hello World change template](change-templates-aws-managed.md)
+ [Creating change templates](change-templates-create.md)
+ [Reviewing and approving or rejecting change templates](change-templates-review.md)