# Walkthrough: Create and test a String parameter \(console\)<a name="sysman-paramstore-console"></a>

The following procedure walks you through the process of creating a String parameter in Parameter Store and then running a command that uses this parameter\.

**To create a String parameter using Parameter Store**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\. 

1. Choose **Create parameter**\.

1. In the **Name** box, enter a hierarchy and a name\. For example, enter **/Test/helloWorld**\.

   For more information about parameter hierarchies, see [Organizing parameters into hierarchies](sysman-paramstore-su-organize.md)\.

1. In the **Description** field, enter a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**\.

1. For **Data type**, leave the default selection `text`\.

1. In the **Value** field, enter a string\. For example, enter **This is my first parameter**\.

1. Choose **Create parameter**\.

1. In the navigation pane, choose **Run Command**\. 

1. Choose **Run command**\.

1. In the **Command document** list, choose `AWS-RunPowerShellScript` \(Windows\) or `AWS-RunShellScript` \(Linux\)\. 

1. For **Command parameters**, enter **echo \{\{ssm:*parameter\-name*\}\}**\. For example: **echo \{\{ssm:/Test/helloWorld\}\}**\. 

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. Choose **Run**\.

1. In the **Command ID** page, in the **Targets and outputs** area, select the button next to the ID of an instance where you ran the command, and then choose **View output**\. Verify that the output of the command is the value you provided for the parameter, such as **This is my first parameter**\.