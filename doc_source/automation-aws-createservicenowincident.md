# AWS\-CreateServiceNowIncident<a name="automation-aws-createservicenowincident"></a>

**Description**

This document creates an incident in the ServiceNow incident table\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateServiceNowIncident)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Category 

  Type: String

  Description: \(Optional\) The category of the incident\.

  Valid values: None \| Inquiry/Help \| Software \| Hardware \| Network \| Database

  Default Value: None
+ Description

  Type: String

  Description: \(Required\) A detailed explanation on the incident\.
+ Impact

  Type: String

  Description: \(Optional\) The effect an incident has on business\.

  Valid values: High \| Medium \| Low

  Default Value: Low
+ ServiceNowInstanceUsername

  Type: String

  Description: \(Required\) The name of the user the incident will be created with\.
+ ServiceNowInstancePassword

  Type: String

  Description: \(Required\) The name of an encrypted SSM Parameter containing the password for the ServiceNow user\.
+ ServiceNowInstanceURL

  Type: String

  Description: \(Required\) The URL of the ServiceNow instance
+ ShortDescription

  Type: String

  Description: \(Required\) A brief description of the incident\.
+ Subcategory

  Type: String

  Description: \(Optional\) The subcategory of the incident\.

  Valid values: None \| Antivirus \| Email \| Internal Application \| Operating System \| CPU \| Disk \| Keyboard \| Hardware \| Memory \| Monitor \| Mouse \| DHCP \| DNS \| IP Address \| VPN \| Wireless \| DB2 \| MS SQL Server \| Oracle 

  Default Value: None

**Document Steps**

Push\_incident – Pushes the incident information to ServiceNow\.

**Outputs**

Push\_incident\.incidentID – The created incident ID\.