# Integrate AWS services with OpsCenter<a name="OpsCenter-applications-that-integrate"></a>

 OpsCenter, a capability of AWS Systems Manager, integrates with multiple AWS services to diagnose and remediate issues with AWS resources\. You must set up the AWS service before you integrate it with OpsCenter\. Based on your requirements, you can use any of the following services to create automatic OpsItems: 
+ By default, the following AWS services are integrated with OpsCenter:
  + [Amazon CloudWatch](OpsCenter-about-cloudwatch.md)
  + [Amazon CloudWatch Application Insights](OpsCenter-about-cloudwatch-insights.md)
  + [Amazon EventBridge](OpsCenter-about-eventbridge.md)
  + [AWS Config](OpsCenter-about-eventbridge.md)
  + [AWS Systems Manager Incident Manager](OpsCenter-about-incident-manager.md)
+ You have to integrate the following services with OpsCenter:
  + [Amazon DevOps Guru](OpsCenter-integrate-with-devops-guru.md)
  + [AWS Security Hub](OpsCenter-integrate-with-security-hub.md)

When any of these services create an OpsItem, you can manage and remediate the OpsItem from OpsCenter\. For more information, see [Manage OpsItems](OpsCenter-working-with-OpsItems.md) and [Remediate OpsItem issues](OpsCenter-remediating.md)\. 

For more information about each AWS service and how it integrates with OpsCenter, see the following topics\.