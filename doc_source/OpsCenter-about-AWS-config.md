# AWS Config<a name="OpsCenter-about-AWS-config"></a>

AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. When AWS Config detects a noncompliant instance, it creates an event and sends it to EventBridge\. EventBridge reviews the event against rules, and if the rule matches, it creates an OpsItem in OpsCenter\. Using this OpsItem, you can track details of the noncompliant resource, record investigative actions, and provide access to consistent remediation actions\.

When you enable OpsCenter using Integrated Setup, it integrates AWS Config with OpsCenter\. 