# Working with Incident Manager incidents in OpsCenter<a name="OpsCenter-create-OpsItems-for-Incident-Manager"></a>

This topic describes how to create an AWS Incident Manager incident from an existing OpsItem\. An *incident* is any unplanned interruption or reduction in quality of services\. Incident Manager, a capability of AWS Systems Manager, provides an incident management console that helps you mitigate and recover from incidents affecting you AWS hosted applications\.

Incident Manager increases incident resolution by notifying responders of how AWS resources have been impacted, highlighting relevant troubleshooting data, and providing collaboration tools to get services back up and running\. To achieve the primary goal of reducing the time\-to\-resolution of critical incidents, Incident Manager automates response plans and allows responder team escalation\. For more information, see the [AWS Systems Manager Incident Manager User Guide](https://docs.aws.amazon.com/incident-manager/latest/userguide)\.

After an incident is resolved, post incident analysis guides you through identifying improvements to your incident response and recommending action items to address the findings\. With high severity operational issues such as incidents, creating an OpsItem in OpsCenter provides operators with a complete view of the incident, including analysis and action items\. OpsCenter is a capability of Systems Manager\. This comprehensive view improves time\-to\-resolution and helps mitigate similar issues in the future\.

**How it works**  
After you set up and configure Incident Manager, the system integrates with OpsCenter in the following ways:

1. After an incident is created in Incident Manager, the system creates an OpsItem in OpsCenter \(if an OpsItem doesn't already exist\)\. The incident is added as a related item to the OpsItem\. This first OpsItem is known as the *parent* OpsItem\. You can also manually create an incident from an OpsItem\. After you create an incident from an OpsItem, the OpsItem is promoted to a parent OpsItem\.

   OpsItems that include incidents have two additional tabs beyond the **Overview** and **Related resource details** tabs displayed for standard OpsItems\. OpsItems that include incidents display related incidents, OpsItems, analyses, and action items on the **Associated items** tab\. The **Timeline** tab displays a chronological history of related incidents and analyses for the parent OpsItem\.

1. If an incident grows in scale and scope, you can add additional incidents to the parent OpsItem\.

1. After an incident is closed, you can create an analysis from the incident in Incident Manager\. An analysis can help you define improvement processes for mitigating similar issues in the future\. The system automatically updates the incident in OpsCenter with an analysis\. If an analysis includes action items, the system creates additional OpsItems beneath the analysis\. These additional OpsItems are of type **Action Item**\. 

**Before you begin**  
You must set up and configure response plans in Incident Manager\. A *response plan* defines how to escalate an incident to first responders and what actions those responders should take\. For more information, see [Response plans](https://docs.aws.amazon.com/incident-manager/latest/userguide/response-plans.html)\.

## Create an incident for an OpsItem<a name="OpsCenter-create-OpsItems-for-Incident-Manager-create"></a>

Use the following procedure to manually create an incident for an OpsItem\.

**To manually create an incident for an OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. If Incident Manager created an OpsItem for you, choose it and go to step 5\. If not, choose **Create OpsItem** and complete the form\. If you don't see this button, choose the **OpsItems** tab, and then choose **Create OpsItem**\.

1. If you created a new OpsItem, open it\.

1. Choose **Start Incident**\.

1. For **Response plan**, choose the Incident Manager response plan you want to assign to this incident\.

1. \(Optional\) For **Title**, enter a descriptive name to help other team members understand the nature of the incident\. If you don't enter a new title, OpsCenter creates the OpsItem and the corresponding incident in Incident Manager using the title in the response plan\.

1. \(Optional\) For **Incident impact**, choose an impact level for this incident\. If you don't choose an impact level, OpsCenter creates the OpsItem and the corresponding incident in Incident Manager using the impact level in the response plan\.

1. Choose **Start**\.