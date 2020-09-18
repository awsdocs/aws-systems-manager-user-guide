# Walkthrough: Create a SecureString parameter and join an instance to a Domain \(PowerShell\)<a name="sysman-param-securestring-walkthrough"></a>

This walkthrough shows how to join a Windows Server instance to a domain using Systems Manager `SecureString` parameters and Run Command\. The walkthrough uses typical domain parameters, such as the domain name and a domain user name\. These values are passed as unencrypted string values\. The domain password is encrypted using an AWS\-managed customer master key \(CMK\) and passed as an encrypted string\. 

**Prerequisites**  
This walkthrough assumes that you already specified your domain name and DNS server IP address in the DHCP option set that is associated with your Amazon VPC\. For information, see [Working with DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#DHCPOptionSet) in the *Amazon VPC User Guide*\.

**To create a `SecureString` parameter and join an instance to a domain**

1. Enter parameters into the system using AWS Tools for Windows PowerShell\.

   ```
   Write-SSMParameter -Name "domainName" -Value "DOMAIN-NAME" -Type String
   Write-SSMParameter -Name "domainJoinUserName" -Value "DOMAIN\USERNAME" -Type String
   Write-SSMParameter -Name "domainJoinPassword" -Value "PASSWORD" -Type SecureString
   ```
**Important**  
Only the *value* of a `SecureString` parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.

1. Attach the following IAM policies to the IAM role permissions for your instance: 
   + **AmazonSSMManagedInstanceCore** – Required\. This AWS managed policy enables a managed instance to use Systems Manager service core functionality\.
   + **AmazonSSMDirectoryServiceAccess** – Required\. This AWS managed policy allows SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\.
   + **A custom policy for S3 bucket access** – Required\. SSM Agent, which is on your instance and performs Systems Manager tasks, requires access to specific Amazon\-owned S3 buckets\. In the custom S3 bucket policy that you create, you also provide access to S3 buckets of your own that are necessary for Systems Manager operations\. 

     Examples: You can write output for Run Command commands or Session Manager sessions to an S3 bucket, and then use this output later for auditing or troubleshooting\. You store access scripts or custom patch baseline lists in an S3 bucket, and then reference the script or list when you run a command, or when a patch baseline is applied\.

     For information about creating a custom policy for Amazon S3 bucket access, see [Create a custom S3 bucket policy for an instance profile](setup-instance-profile.md#instance-profile-custom-s3-policy)
**Note**  
Saving output log data in an S3 bucket is optional, but we recommend setting it up at the beginning of your Systems Manager configuration process if you have decided to use it\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.
   + **CloudWatchAgentServerPolicy** – Optional\. This AWS managed policy allows you to run the CloudWatch agent on managed instances\. This policy makes it possible to read information on an instance and write it to Amazon CloudWatch\. Your instance profile needs this policy only if you will use services such as EventBridge or CloudWatch Logs\.
**Note**  
Using CloudWatch Logs and EventBridge features is optional, but we recommend setting them up at the beginning of your Systems Manager configuration process if you have decided to use them\. For more information, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)* and the *[Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)*\.

1. Edit the IAM role attached to the instance and add the following policy\. This policy gives the instance permissions to call the `kms:Decrypt` API\. 

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":[
               "kms:Decrypt"
            ],
            "Resource":[
               "arn:aws:kms:region:account-id:key/kms-key-id"
            ]
         }
      ]
   }
   ```

1. Copy and paste the following json sample into a simple text editor and save the file as `JoinInstanceToDomain.json` in the following location: `c:\temp\JoinInstanceToDomain.json`\.

   ```
   {
       "schemaVersion": "2.2",
       "description": "Run a PowerShell script to securely domain-join a Windows Server instance",
       "mainSteps": [
           {
               "action": "aws:runPowerShellScript",
               "name": "runPowerShellWithSecureString",
               "precondition": {
                   "StringEquals": [
                       "platformType",
                       "Windows"
                   ]
               },
               "inputs": {
                   "runCommand": [
                       "$domain = (Get-SSMParameterValue -Name domainName).Parameters[0].Value",
                       "if ((gwmi Win32_ComputerSystem).domain -eq $domain){write-host \"Computer is part of $domain, exiting\"; exit 0}",
                       "$username = (Get-SSMParameterValue -Name domainJoinUserName).Parameters[0].Value",
                       "$password = (Get-SSMParameterValue -Name domainJoinPassword -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -asPlainText -Force",
                       "$credential = New-Object System.Management.Automation.PSCredential($username,$password)",
                       "Add-Computer -DomainName $domain -Credential $credential -ErrorAction SilentlyContinue -ErrorVariable domainjoinerror",
                       "if($?){Write-Host \"Domain join succeeded, restarting\"; exit 3010}else{Write-Host \"Failed to join domain with error:\" $domainjoinerror; exit 1 }"
                   ]
               }
           }
       ]
   }
   ```

1. Run the following command in AWS Tools for Windows PowerShell to create a new SSM document\.

   ```
   $json = Get-Content C:\temp\JoinInstanceToDomain | Out-String
   New-SSMDocument -Name JoinInstanceToDomain -Content $json -DocumentType Command
   ```

1. Run the following command in AWS Tools for Windows PowerShell to join the instance to the domain

   ```
   Send-SSMCommand -InstanceId instance-id -DocumentName JoinInstanceToDomain 
   ```

   If the command succeeds, the system returns information similar to the following\. 

   ```
   WARNING: The changes will take effect after you restart the computer EC2ABCD-EXAMPLE.
   Domain join succeeded, restarting
   Computer is part of example.local, exiting
   ```

   If the command fails, the system returns information similar to the following\. 

   ```
   Failed to join domain with error:
   Computer 'EC2ABCD-EXAMPLE' failed to join domain 'example.local'
   from its current workgroup 'WORKGROUP' with following error message:
   The specified domain either does not exist or could not be contacted.
   ```