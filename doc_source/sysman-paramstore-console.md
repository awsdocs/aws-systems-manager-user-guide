# Walkthrough: Create and Use a Parameter in a Command \(Console\)<a name="sysman-paramstore-console"></a>

The following procedure walks you through the process of creating a parameter in Parameter Store and then executing a command that uses this parameter\.

**To create a parameter using Parameter Store**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Parameter Store**\. 

1. Choose **Create parameter**\.

1. In the **Name** box, type a hierarchy and a name\. For example, type `/Test/helloWorld`\.

   For more information about parameter hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

1. In the **Description** field, type a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**\.

1. In the **Value** field, type a string\. For example, type `My1stParameter`\.

1. Choose **Create parameter**\.

1. In the navigation pane, choose **Run Command**\. 

1. Choose **Run command**\.

1. In the **Command document** list, choose AWS\-RunPowershellScript \(Windows\) or AWS\-RunShellScript \(Linux\)\. 

1. Under **Target instances**, choose an instance you created earlier\.

1. In the **Commands** field, type echo `{{ssm:parameter name}}`, for example, echo `{{ssm:/Test/helloWorld}}`\. 

1. Choose **Run**\.

1. Scroll to the bottom of the **Command ID** page, select the radio button next to an instance ID, and then choose **View output**\. 