# DevOps Guru<a name="OpsCenter-integrate-with-devops-guru"></a>

Amazon DevOps Guru applies machine learning to analyze your operational data, application metrics, and application events to identify behaviors that deviate from normal operating patterns\. If you enable DevOps Guru to generate an OpsItem in OpsCenter, each insight generates a new OpsItem\. You can use OpsCenter to manage your OpsItems\. 

Amazon DevOps Guru automatically creates OpsItems\. You can enable Amazon DevOps Guru to create OpsItems by using Quick Setup, which is a capability of Systems Manager\. The system creates OpsItems by using the [AWSServiceRoleForDevOpsGuru](https://docs.aws.amazon.com/devops-guru/latest/userguide/using-service-linked-roles.html) AWS Identity and Access Management \(IAM\) service\-linked role\.

**To integrate OpsCenter with DevOps Guru:**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

1. On the **Customize DevOps Guru configuration options** page, choose the **Library** tab\. 

1. In the **DevOps Guru** pane, choose **Create**

1. For **Configuration options**, select **Enable AWS Systems Manager OpsItems\.**

1. Select **Create** once you complete the setup\.