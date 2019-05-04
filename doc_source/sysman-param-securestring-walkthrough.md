# Walkthrough: Create a Secure String Parameter and Join an Instance to a Domain \(PowerShell\)<a name="sysman-param-securestring-walkthrough"></a>

This walkthrough shows you how to join a Windows instance to a domain using Systems Manager secure string parameters and Run Command\. The walkthrough uses typical domain parameters, such as the domain name and a domain user name\. These values are passed as unencrypted string values\. The domain password is encrypted using an AWS\-managed customer master key \(CMK\) and passed as a secure string\. 

**Prerequisites**  
This walkthrough assumes that you have already specified your domain name and DNS server IP address in the DHCP option set that is associated with your Amazon VPC\. For information, see [Working with DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#DHCPOptionSet) in the *Amazon VPC User Guide*\.

**To create a secure string parameter and join an instance to a domain**

1. Enter parameters into the system using AWS Tools for Windows PowerShell\.

   ```
   Write-SSMParameter -Name "domainName" -Value "DOMAIN-NAME" -Type String -Tier Standard
   Write-SSMParameter -Name "domainJoinUserName" -Value "DOMAIN\USERNAME" -Type String -Tier Standard
   Write-SSMParameter -Name "domainJoinPassword" -Value "PASSWORD" -Type SecureString -Tier Standard
   ```
**Important**  
Only the *value* of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.

1. Attach the **AmazonEC2RoleforSSM** managed policy to the IAM role permissions for your instance\. For information, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

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
               "arn:aws:kms:region:account-id:key/key-id"
            ]
         }
      ]
   }
   ```

1. Copy and paste the following json sample into a simple text editor and save the file as JoinInstanceToDomain\.json in the following location: `c:\temp\JoinInstanceToDomain.json`\.

   ```
   {
       "schemaVersion": "2.2",
       "description": "Run a PowerShell script to securely domain-join a Windows instance",
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
                       "$username = (Get-SSMParameterValue -Name domainJoinUserName).Parameters[0].Value",
                       "$password = (Get-SSMParameterValue -Name domainJoinPassword -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -asPlainText -Force",
                       "$credential = New-Object System.Management.Automation.PSCredential($username,$password)",
                       "Add-Computer -DomainName $domain -Credential $credential -ErrorAction Stop",
                       "Restart-Computer -force"
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