# Creating an Automation Document that Runs Scripts \(Command Line\)<a name="automation-document-script-commandline"></a>

The following examples show how to use the AWS CLI \(on Linux or Windows Server\) or AWS Tools for PowerShell to create an Automation document that runs a script using the `Attachment` parameter\.

**Before You Begin**  
Before you begin, ensure you have the following resources prepared\. 
+ The content for your Automation document in a YAML\-formatted file with a \.yaml extension, such as `MyAutomationDocument.yaml`\.
+ The files to attach to the document\. You can attach script files or \.zip files\. 

  For scripts, Automation supports Python 3\.6 and 3\.7, PowerShell Core 6\.0\.
+ Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

  For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md) or [Install or Upgrade the AWS Tools for PowerShell](getting-started-ps.md)\.

**Attach a single file from an S3 bucket**  
Run the following command to create an Automation document using a script file stored in an Amazon S3 bucket\.

------
#### [ Linux ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://AutomationDocument.yaml \
  --document-format YAML \
  --attachments "Key=S3FileUrl,Name=script.py,Values=https://bucket-name.s3-aws-region.amazonaws.com/filePath" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://AutomationDocument.yaml ^
  --document-format YAML ^
  --attachments Key=S3FileUrl,Name=script.py,Values="https://bucket-name.s3-aws-region.amazonaws.com/filePath" ^
  --document-type Automation
```

------
#### [ PowerShell ]

```
New-SSMDocument `
  -Name CustomAutomationScript `
  -Content file://AutomationDocument.yaml `
  -DocumentFormat YAML `
  -Attachments @{
      "Key"="S3FileUrl";
      "Name"="script.py";
      "Values"="https://bucket-name.s3-aws-region.amazonaws.com/filePath"
    }, @{
      "Key"="S3FileUrl";
      "Name"="Zip-file.zip";
      "Values"="https://bucket-name.s3-aws-region.amazonaws.com/filePath"
    } `
  -DocumentType Automation
```

------

**Attach files from an S3 bucket**  
Run the following command to create an Automation document using a script or multiple script files stored in an Amazon S3 bucket\. Note that a `Name` key for files is not specified\. The command attaches all supported files from the S3 bucket location\.

------
#### [ Linux ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://AutomationDocument.yaml \
  --document-format YAML \
  --attachments Key=SourceUrl,Values="https://bucket-name.s3-aws-region.amazonaws.com/filePath" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://AutomationDocument.yaml ^
  --document-format YAML ^
  --attachments Key=SourceUrl,Values="https://bucket-name.s3-aws-region.amazonaws.com/filePath" ^
  --document-type Automation
```

------
#### [ PowerShell ]

```
New-SSMDocument `
  -Name CustomAutomationScript `
  -Content file://AutomationDocument.yaml `
  -DocumentFormat YAML `
  -Attachments @{
      "Key"="SourceUrl";
      "Values"="https://bucket-name.s3-aws-region.amazonaws.com/filePath"
    } `
  -DocumentType Automation
```

------