# Creating a runbook that runs scripts \(command line\)<a name="automation-document-script-commandline"></a>

The following examples show how to use the AWS CLI or AWS Tools for PowerShell to create an runbook that runs a script using the `Attachment` parameter\.

**Before You Begin**  
Before you begin, ensure you have the following resources prepared\. 
+ The content for your runbook in a YAML\-formatted file with a \.yaml extension, such as `MyAutomationDocument.yaml`\.
+ The files to attach to the runbook\. You can attach script files or \.zip files\. 

  For scripts, Automation supports Python 3\.6 and 3\.7, PowerShell Core 6\.0\.
+ Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

  For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

**Attach a single file from an S3 bucket**  
Run the following command to create a runbook using a script file stored in an S3 bucket\.

------
#### [ Linux & macOS ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://Runbook.yaml \
  --document-format YAML \
  --attachments "Key=S3FileUrl,Name=script.py,Values=https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://AutomationDocument.yaml ^
  --document-format YAML ^
  --attachments Key=S3FileUrl,Name=script.py,Values="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath" ^
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
      "Values"="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath"
    }, @{
      "Key"="S3FileUrl";
      "Name"="Zip-file.zip";
      "Values"="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath"
    } `
  -DocumentType Automation
```

------

**Attach files from an S3 bucket**  
Run the following command to create a runbook using a script or multiple script files stored in an S3 bucket\. Note that a `Name` key for files is not specified\. The command attaches all supported files from the S3 bucket location\.

------
#### [ Linux & macOS ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://Runbook.yaml \
  --document-format YAML \
  --attachments Key=SourceUrl,Values="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://Runbook.yaml ^
  --document-format YAML ^
  --attachments Key=SourceUrl,Values="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath" ^
  --document-type Automation
```

------
#### [ PowerShell ]

```
New-SSMDocument `
  -Name CustomAutomationScript `
  -Content file://Runbook.yaml `
  -DocumentFormat YAML `
  -Attachments @{
      "Key"="SourceUrl";
      "Values"="https://DOC-EXAMPLE-BUCKET.s3-aws-region.amazonaws.com/filePath"
    } `
  -DocumentType Automation
```

------

**Attach files from another runbook in your AWS account**  
Run the following command to create a runbook using a script that is already attached to another runbook in your account\.

The format of the key value in this command is *runbook\-name/runbook\-version\-number/file\-name*\. For example:

```
"MyDocument/2/script.py"
```

------
#### [ Linux & macOS ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://Runbook.yaml \
  --document-format YAML \
  --attachments Key=AttachmentReference,Values="runbook-name/runbook-version-number/file-name" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://Runbook.yaml ^
  --document-format YAML ^
  --attachments Key=AttachmentReference,Values="runbook-name/runbook-version-number/file-name" ^
  --document-type Automation
```

------
#### [ PowerShell ]

```
New-SSMDocument `
  -Name CustomAutomationScript `
  -Content file://Runbook.yaml `
  -DocumentFormat YAML `
  -Attachments @{
      "Key"="AttachmentReference";
      "Values"="runbook-name/runbook-version-number/file-name"
    } `
  -DocumentType Automation
```

------

**Attach files from an runbook in another AWS account**  
Run the following command to create a runbook using a script that is already attached to a runbook that has been shared with you from another AWS account\.

The format of the key value in this command is *runbook\-arn/runbook\-version\-number/file\-name*\. For example:

```
"arn:aws:ssm:us-east-2:123456789012:document/OtherAccountDocument/2/script.py"
```

------
#### [ Linux & macOS ]

```
aws ssm create-document \
  --name CustomAutomationScript \
  --content file://Runbook.yaml \
  --document-format YAML \
  --attachments Key=AttachmentReference,Values="runbook-arn/runbook-version-number/file-name/file-name" \
  --document-type Automation
```

------
#### [ Windows ]

```
aws ssm create-document ^
  --name CustomAutomationScript ^
  --content file://Runbook.yaml ^
  --document-format YAML ^
  --attachments Key=AttachmentReference,Values="runbook-arn/runbook-version-number/file-name" ^
  --document-type Automation
```

------
#### [ PowerShell ]

```
New-SSMDocument `
  -Name CustomAutomationScript `
  -Content file://Runbook.yaml `
  -DocumentFormat YAML `
  -Attachments @{
      "Key"="AttachmentReference";
      "Values"="runbook-arn/runbook-version-number/file-name"
    } `
  -DocumentType Automation
```

------