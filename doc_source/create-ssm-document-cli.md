# Create an SSM document \(command line\)<a name="create-ssm-document-cli"></a>

After you create the content for your custom SSM document, as described in [Writing SSM document content](create-ssm-doc.md#writing-ssm-doc-content), you can use the AWS CLI or AWS Tools for PowerShell to create an SSM document using your content\. This is shown in the following command\.

**Before you begin**  
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

------
#### [ Linux ]

```
aws ssm create-document \
    --content file://path/to/file/documentContent.json \  
    --name "ExampleDocument" \
    --document-type "Command"
```

------
#### [ Windows ]

```
aws ssm create-document ^
    --content file://C:\path\to\file\documentContent.json ^
    --name "ExampleDocument" ^
    --document-type "Command"
```

------
#### [ PowerShell ]

```
$json = Get-Content -Path "C:\path\to\file\documentContent.json" | Out-String
New-SSMDocument `
    -Content $json `
    -Name "ExampleDocument" `
    -DocumentType "Command"
```

------