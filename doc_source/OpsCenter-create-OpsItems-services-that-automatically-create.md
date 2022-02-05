# Services and capabilities that automatically create OpsItems<a name="OpsCenter-create-OpsItems-services-that-automatically-create"></a>

OpsCenter, a capability of AWS Systems Manager, integrates with the following AWS services and Systems Manager capabilities\. When enabled, these services and capabilities can automatically create OpsItems in OpsCenter on your behalf\.

**Amazon DevOps Guru**  
DevOps Guru applies machine learning to analyze your operational data, application metrics, and application events to identify behaviors that deviate from normal operating patterns\. The system notifies you when DevOps Guru detects an operational issue or risk\.  
If you enabled DevOps Guru, the system creates OpsItems on your behalf by using the following AWS Identity and Access Management \(IAM\) service\-linked role: [AWSServiceRoleForDevOpsGuru](https://docs.aws.amazon.com/devops-guru/latest/userguide/using-service-linked-roles.html)\.

**AWS Security Hub**  
Security Hub provides you with a comprehensive view of the security state of your AWS resources\. Security Hub collects security data from across AWS accounts and services, and helps you analyze your security trends to identify and prioritize the security issues across your AWS environment\.  
If you enabled Security Hub, and if you enabled the Security Hub data source in Explorer, then the system creates OpsItems on your behalf by using the following IAM service\-linked role: [AWSServiceRoleForAmazonSSM\_OpsInsights](https://docs.aws.amazon.com/systems-manager/latest/userguide/using-service-linked-roles.html)\.

**OpsCenter operational insights**  
You can configure OpsCenter to automatically analyze OpsItems in your account and generate two types of insights\. In the Systems Manager console, the **Duplicate OpsItems** field displays a count of insights when eight or more OpsItems have the same title for the same resource\. The **Sources generating most OpsItems** field displays a count of insights when more than 50 OpsItems have the same title\. If you choose an insight, OpsCenter displays information about the affected OpsItems and resources\.  
If you enabled operational insights, then the system creates OpsItems on your behalf by using the following IAM service\-linked role: [AWSSSMOpsInsightsServiceRolePolicy](https://docs.aws.amazon.com/systems-manager/latest/userguide/using-service-linked-roles.html)\.