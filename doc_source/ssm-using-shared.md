# Use a Shared Systems Manager Document<a name="ssm-using-shared"></a>

When you share a Systems Manager document, the system generates an Amazon Resource Name \(ARN\) and assigns it to the command\. If you select and execute a shared document from the Amazon EC2 console, you do not see the ARN\. However, if you want to execute a shared Systems Manager document from a command line application, you must specify a full ARN\. You are shown the full ARN for a Systems Manager document when you execute the command to list documents\. 

**Note**  
You are not required to specify ARNs for AWS public documents \(documents that begin with AWS\-\*\) or commands that you own\.

**Topics**
+ [Use a Shared Systems Manager Document \(AWS CLI\)](#ssm-using-shared-cli)
+ [Use a Shared Systems Manager Document \(AWS Tools for Windows PowerShell\)](#ssm-using-shared-ps)

## Use a Shared Systems Manager Document \(AWS CLI\)<a name="ssm-using-shared-cli"></a>

**To list all public Systems Manager documents**

```
aws ssm list-documents --document-filter-list key=Owner,value=Public
```

**To list private Systems Manager documents that have been shared with you**

```
aws ssm list-documents --document-filter-list key=Owner,value=Private
```

**To list all Systems Manager documents available to you**

```
aws ssm list-documents --document-filter-list key=Owner,value=All
```

**Execute a command from a shared Systems Manager document using a full ARN**

```
aws ssm send-command --document-name FullARN/name
```

For example:

```
aws ssm send-command --document-name arn:aws:ssm:us-east-2:12345678912:document/highAvailabilityServerSetup --instance-ids i-12121212
```

## Use a Shared Systems Manager Document \(AWS Tools for Windows PowerShell\)<a name="ssm-using-shared-ps"></a>

**To list all public Systems Manager documents**

```
Get-SSMDocumentList -DocumentFilterList @(New-Object Amazon.SimpleSystemsManagement.Model.DocumentFilter("Owner", "Public"))
```

**To list private Systems Manager documents that have been shared with you**

```
Get-SSMDocumentList -DocumentFilterList @(New-Object Amazon.SimpleSystemsManagement.Model.DocumentFilter("Owner", "Private"))
```

**To get information about a Systems Manager document that has been shared with you**

```
Get-SSMDocument –Name FullARN/name
```

For example:

```
Get-SSMDocument –Name arn:aws:ssm:us-east-2:12345678912:document/highAvailabilityServerSetup
```

**To get a description of a Systems Manager document that has been shared with you**

```
Get-SSMDocumentDescription –Name FullARN/name
```

For example:

```
Get-SSMDocumentDescription –Name arn:aws:ssm:us-east-2:12345678912:document/highAvailabilityServerSetup
```

**To execute a command from a shared Systems Manager document using a full ARN**

```
Send-SSMCommand –DocumentName FullARN/name –InstanceId IDs
```

For example:

```
Send-SSMCommand –DocumentName arn:aws:ssm:us-east-2:555450671542:document/highAvailabilityServerSetup –InstanceId @{"i-273d4e9e"}
```