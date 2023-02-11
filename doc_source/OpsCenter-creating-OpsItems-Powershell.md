# Creating OpsItems manually \(PowerShell\)<a name="OpsCenter-creating-OpsItems-Powershell"></a>

The following procedure describes how to create an OpsItem by using AWS Tools for Windows PowerShell\. 

**To create an OpsItem using AWS Tools for Windows PowerShell**

1. Open AWS Tools for Windows PowerShell and run the following command to specify your credentials\. 

   ```
   Set-AWSCredentials –AccessKey key-name –SecretKey key-name
   ```

1. Run the following command to set the AWS Region for your PowerShell session\.

   ```
   Set-DefaultAWSRegion -Region Region
   ```

1. Run the following command to create a new OpsItem\. Replace each *example resource placeholder* with your own information\. This command specifies a Systems Manager Automation runbook for remediating this OpsItem\. 

   ```
   $opsItem = New-Object Amazon.SimpleSystemsManagement.Model.OpsItemDataValue
   $opsItem.Type = [Amazon.SimpleSystemsManagement.OpsItemDataType]::SearchableString 
   $opsItem.Value = '[{\"automationId\":\"runbook_name\",\"automationType\":\"AWS::SSM::Automation\"}]'
   $newHash = @{" /aws/automations"=[Amazon.SimpleSystemsManagement.Model.OpsItemDataValue]$opsItem}
   
   New-SSMOpsItem `
       -Title "title" `
       -Description "description" `
       -Priority priority_number `
       -Source AWS_service `
       -OperationalData $newHash
   ```

   If successful, the command outputs the ID of the new OpsItem\.

The following example specifies the Amazon Resource Name \(ARN\) of an impaired Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

```
$opsItem = New-Object Amazon.SimpleSystemsManagement.Model.OpsItemDataValue
$opsItem.Type = [Amazon.SimpleSystemsManagement.OpsItemDataType]::SearchableString 
$opsItem.Value = '[{\"arn\":\"arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0\"}]'
$newHash = @{" /aws/resources"=[Amazon.SimpleSystemsManagement.Model.OpsItemDataValue]$opsItem}
New-SSMOpsItem -Title "EC2 instance disk full still" -Description "Log clean up may have failed which caused the disk to be full" -Priority 2 -Source ec2 -OperationalData $newHash
```