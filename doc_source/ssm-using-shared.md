# Using shared SSM documents<a name="ssm-using-shared"></a>

When you share an SSM document, the system generates an Amazon Resource Name \(ARN\) and assigns it to the command\. If you select and run a shared document from the Amazon EC2 console, you do not see the ARN\. However, if you want to run a shared SSM document from a command line application, you must specify a full ARN\. You are shown the full ARN for an SSM document when you run the command to list documents\. 

**Note**  
You are not required to specify ARNs for AWS public documents \(documents that begin with AWS\-\*\) or documents that you own\.

## Use a shared SSM document \(command line\)<a name="ssm-using-shared-cli"></a>

 **To list all public SSM documents** 

------
#### [ Linux ]

```
aws ssm list-documents \
    --filters Key=Owner,Values=Public
```

------
#### [ Windows ]

```
aws ssm list-documents ^
    --filters Key=Owner,Values=Public
```

------
#### [ PowerShell ]

```
$filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
$filter.Key = "Owner"
$filter.Values = "Public"

Get-SSMDocumentList `
    -Filters @($filter)
```

------

 **To list private SSM documents that have been shared with you** 

------
#### [ Linux ]

```
aws ssm list-documents \
    --filters Key=Owner,Values=Private
```

------
#### [ Windows ]

```
aws ssm list-documents ^
    --filters Key=Owner,Values=Private
```

------
#### [ PowerShell ]

```
$filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
$filter.Key = "Owner"
$filter.Values = "Private"

Get-SSMDocumentList `
    -Filters @($filter)
```

------

 **To list all SSM documents available to you** 

------
#### [ Linux ]

```
aws ssm list-documents
```

------
#### [ Windows ]

```
aws ssm list-documents
```

------
#### [ PowerShell ]

```
Get-SSMDocumentList
```

------

 **To get information about an SSM document that has been shared with you** 

------
#### [ Linux ]

```
aws ssm describe-document \
    --name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------
#### [ Windows ]

```
aws ssm describe-document ^
    --name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------
#### [ PowerShell ]

```
Get-SSMDocumentDescription `
    –Name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------

 **To run a shared SSM document** 

------
#### [ Linux ]

```
aws ssm send-command \
    --document-name arn:aws:ssm:us-east-2:12345678912:document/documentName \
    --instance-ids ID
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name arn:aws:ssm:us-east-2:12345678912:document/documentName ^
    --instance-ids ID
```

------
#### [ PowerShell ]

```
Send-SSMCommand `
    –DocumentName arn:aws:ssm:us-east-2:12345678912:document/documentName `
    –InstanceIds ID
```

------