# AWS\-CreateServiceNowIncident<a name="automation-aws-createservicenowincident"></a>

**Description**

This document creates an incident in the ServiceNow incident table\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
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
+ Description

  Type: String

  Description: \(Required\) A detailed explanation on the incident\.
+ Impact

  Type: String

  Description: \(Optional\) The effect an incident has on business\.

  Valid Values: High \| Medium \| Low

  Default Value: Low
+ Category 

  Type: String

  Description: \(Optional\) The category of the incident\.

  Valid Values: None \| Inquiry/Help \| Software \| Hardware \| Network \| Database

  Default Value: None
+ Subcategory

  Type: String

  Description: \(Optional\) The subcategory of the incident\.

  Valid Values: None \| Antivirus \| Email \| Internal Application \| Operating System \| CPU \| Disk \| Keyboard \| Hardware \| Memory \| Monitor \| Mouse \| DHCP \| DNS \| IP Address \| VPN \| Wireless \| DB2 \| MS SQL Server \| Oracle 

  Default Value: None
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateServiceNowIncident --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

Push\_incident – Pushes the incident information to ServiceNow\.

**Outputs**

Push\_incident\.incidentID – The created incident ID\.