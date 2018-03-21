# Create an SSM Document \(Tools for Windows PowerShell\)<a name="create-ssm-document-ps"></a>

1. Copy and customize an existing document, as described in [Copy a Document](copy-document.md)\.

1. Add the document using the AWS Tools for Windows PowerShell\.

   ```
   $json = Get-Content C:\your file | Out-String
   New-SSMDocument -DocumentType Command -Name document name -Content $json
   ```