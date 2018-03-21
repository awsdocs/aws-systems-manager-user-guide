# Create an SSM Document \(AWS CLI\)<a name="create-ssm-document-cli"></a>

1. Copy and customize an existing document, as described in [Copy a Document](copy-document.md)\.

1. Add the document using the AWS CLI\.

   ```
   aws ssm create-document --content file://path to your file\your file --name "document name" --document-type "Command" 
   ```

   **Windows example**

   ```
   aws ssm create-document --content file://c:\temp\PowershellScript.json --name "PowerShellScript" --document-type "Command" 
   ```

   **Linux example**

   ```
   aws ssm create-document --content file:///home/ec2-user/RunShellScript.json  --name "RunShellScript" --document-type "Command" 
   ```