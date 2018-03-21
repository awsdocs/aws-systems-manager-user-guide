# Walkthrough: Create a Secure String Parameter and Join an Instance to a Domain \(PowerShell\)<a name="sysman-param-securestring-walkthrough"></a>

This walkthrough shows you how to join a Windows instance to a domain using Systems Manager Secure String parameters and Run Command\. The walkthrough uses typical domain parameters, such as the DNS address, the domain name, and a domain user name\. These values are passed as unencrypted string values\. The domain password is encrypted using a AWS KMS master key and passed as a Secure String\. 

**To create a Secure String Parameter and Join an Instance to a Domain**

1. Enter parameters into the system using AWS Tools for Windows PowerShell\.

   ```
   Write-SSMParameter -Name DNS-IP -Value a DNS IP address -Type String
   Write-SSMParameter -Name domainName -Value the domain name -Type String
   Write-SSMParameter -Name domainJoinUserName -Value a user name -Type String
   Write-SSMParameter -Name a-name-for-a-password -Value a password -Type SecureString
   ```
**Important**  
Only the value of the secure string parameter is encrypted\. The name of the parameter, description, and other properties are not encrypted\. For this reason, consider creating a naming system that avoids the word "password" in parameter names\. 

1. Attach the **AmazonEC2RoleforSSM** managed policy to the IAM role permissions for your instance\. For information, see [Managed Policies and Inline Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies)\. 

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
               "arn:aws:kms:region:account_id:key/key_id"
            ]
         }
      ]
   }
   ```

1. Copy and paste the following json sample into a simple text editor and save the file as JoinInstanceToDomain\.json in the following location: `c:\temp\JoinInstanceToDomain.json`\.

   ```
   {
      "schemaVersion":"2.0",
      "description":"Run a PowerShell script to securely domain-join a Windows instance",
      "mainSteps":[
         {
            "action":"aws:runPowerShellScript",
            "name":"runPowerShellWithSecureString",
            "inputs":{
               "runCommand":[
                  "$ipdns = (Get-SSMParameterValue -Name dns).Parameters[0].Value\n",
                  "$domain = (Get-SSMParameterValue -Name domainName).Parameters[0].Value\n",
                  "$username = (Get-SSMParameterValue -Name domainJoinUserName).Parameters[0].Value\n",
                  "$password = (Get-SSMParameterValue -Name domainJoinPassword -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -asPlainText -Force\n",
                  "$credential = New-Object System.Management.Automation.PSCredential($username,$password)\n",
                  "Set-DnsClientServerAddress \"Ethernet 2\" -ServerAddresses $ipdns\n",
                  "Add-Computer -DomainName $domain -Credential $credential\n",
                  "Restart-Computer -force"
               ]
            }
         }
      ]
   }
   ```

1. Execute the following command in AWS Tools for Windows PowerShell to create a new SSM document\.

   ```
   $json = Get-Content C:\temp\JoinInstanceToDomain | Out-String
   New-SSMDocument -Name JoinInstanceToDomain -Content $json -DocumentType Command
   ```

1. Execute the following command in AWS Tools for Windows PowerShell to join the instance to the domain

   ```
   Send-SSMCommand -InstanceId Instance-ID -DocumentName JoinInstanceToDomain 
   ```