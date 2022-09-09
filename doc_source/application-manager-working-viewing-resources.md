# Viewing application resources<a name="application-manager-working-viewing-resources"></a>

In Application Manager, a component of AWS Systems Manager, the **Resources** tab displays the AWS resources in your application\. If you choose a top\-level component, this page displays all resources for that component and any subcomponents\. If you choose a subcomponent, this page shows only the resources assigned to that subcomponent\. 

**Actions you can perform on this page**  
You can perform the following actions on this page:
+ Choose a resource name to view information about it, including details provided by the console where it was created, tags, Amazon CloudWatch alarms, AWS Config details, and AWS CloudTrail log information\.
+ Choose the option button beside a resource name\. Then, choose the **Resource timeline** button to open the AWS Config console where you can view compliance information about a selected resource\. 
+ If you enabled AWS Cost Explorer, the **Cost Explorer** section shows cost data for a specific non\-container application or application component\. You can enable this feature by choosing the **Go to AWS Cost Management console** button\. Use the filters in this section to view cost information about your application\.

**To open the **Resources** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose the **Resources** tab\.