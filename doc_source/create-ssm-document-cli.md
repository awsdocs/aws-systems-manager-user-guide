# Create an SSM document \(command line\)<a name="create-ssm-document-cli"></a>

After you create the content for your custom SSM document, as described in [Writing SSM document content](create-ssm-doc.md#writing-ssm-doc-content), you can use the AWS CLI or AWS Tools for PowerShell to create an SSM document using your content\. This is shown in the following command\.

**Before you begin**  
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

------
#### [ Linux ]

```
aws ssm create-document \
    --content file://path/to/file/documentContent.json \
    --name "document-name" \
    --document-type "Command" \
    --tags "Key=tag-key,Value=tag-value"
```

------
#### [ Windows ]

```
aws ssm create-document ^
    --content file://C:\path\to\file\documentContent.json ^
    --name "document-name" ^
    --document-type "Command" ^
    --tags "Key=tag-key,Value=tag-value"
```

------
#### [ PowerShell ]

```
$json = Get-Content -Path "C:\path\to\file\documentContent.json" | Out-String
New-SSMDocument `
    -Content $json `
    -Name "document-name" `
    -DocumentType "Command" `
    -Tags "Key=tag-key,Value=tag-value"
```

------

*document\-name* is the name of the SSM document you want to tag\.

*tag\-key* is the name of a custom key you supply\. For example, *Region* or *Quarter*\.

*tag\-value* is the custom content for the value you want to supply for that key\. For example, *West* or *Q321*\.

If successful, the command returns a response similar to the following\.

```
{
   "DocumentDescription":{
      "CreatedDate":1.585061751738E9,
      "DefaultVersion":"1",
      "Description":"MyCustomDocument",
      "DocumentFormat":"JSON",
      "DocumentType":"Command",
      "DocumentVersion":"1",
      "Hash":"0d3d879b3ca072e03c12638d0255ebd004d2c65bd318f8354fcde820dEXAMPLE",
      "HashType":"Sha256",
      "LatestVersion":"1",
      "Name":"Example",
      "Owner":"111122223333",
      "Parameters":[
         --truncated--
      ],
      "PlatformTypes":[
         "Windows",
         "Linux"
      ],
      "SchemaVersion":"0.3",
      "Status":"Creating",
      "Tags": [
            {
                "Key": "Purpose",
                "Value": "Test"
            }
        ]
   }
}
```