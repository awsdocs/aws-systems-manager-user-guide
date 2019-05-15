# About Parameters<a name="parameter-store-about-examples"></a>

A Parameter Store parameter is any piece of configuration data, such as a password or license key, that is saved in Parameter Store\. You can then centrally and securely reference this data in your scripts, commands, and SSM documents\. Parameter Store enables you to create three different types of parameters: String, String List, or Secure String\. When you reference a parameter, you specify the parameter name by using the following convention:

`ssm:parameter_name`

The following is an example of a Systems Manager parameter named DNS\-IP\. The value of this parameter is simply the IP address of an instance\. This example uses an AWS CLI command to echo the parameter value\.

```
aws ssm send-command --document-name "AWS-RunPowerShellScript" --document-version "1" --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" --parameters "commands='echo {{ssm:DNS-IP}}'" --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region us-east-1
```

Here is another example parameter that uses a secure string parameter named SecurePassword\. The command `commands=['$secure = (Get-SSMParameterValue -Names SecurePassword -WithDecryption $True).Parameters[0].Value','net user administrator $secure']` retrieves and decrypts the value of the Secure String parameter, and then resets the local administrator password without having to pass the password in clear text\.

```
aws ssm send-command --document-name "AWS-RunPowerShellScript" --document-version "1" --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" --parameters "commands=['$secure = (Get-SSMParameterValue -Names SecurePassword -WithDecryption $True).Parameters[0].Value','net user administrator $secure']" --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region us-east-1
```

You can also reference Systems Manager parameters in the *Parameters* section of an SSM document, as shown in the following example\.

```
{
   "schemaVersion":"2.0",
   "description":"Sample version 2.0 document v2",
   "parameters":{
      "commands" : {
        "type": "StringList",
        "default": ["{{ssm:parameter_name}}"]
      }
    },
    "mainSteps":[
       {
          "action":"aws:runShellScript",
          "name":"runShellScript",
          "inputs":{
             "runCommand": "{{commands}}"
          }
       }
    ]
}
```

**Note**  
The runtimeConfig section of SSM documents use similar syntax for *local parameters*\. A local parameter isn't the same as a Systems Manager parameter\. You can distinguish local parameters from Systems Manager parameters by the absence of the `ssm:` prefix\.  

```
"runtimeConfig":{
        "aws:runShellScript":{
            "properties":[
                {
                    "id":"0.aws:runShellScript",
                    "runCommand":"{{ commands }}",
                    "workingDirectory":"{{ workingDirectory }}",
                    "timeoutSeconds":"{{ executionTimeout }}"
```

SSM documents currently don't support references to secure string parameters\. This means that to use secure string parameters with, for example, Run Command, you have to retrieve the parameter value before passing it to Run Command, as shown in the following examples:

**AWS CLI**

```
value=$(aws ssm get-parameters --names parameter_name --with-decryption)
```

```
aws ssm send-command –name AWS-JoinDomain –parameters password=$value –instance-id instance-id
```

**Tools for Windows PowerShell**

```
$secure = (Get-SSMParameterValue -Names parameter_name -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -AsPlainText -Force
```

```
$cred = New-Object System.Management.Automation.PSCredential -argumentlist user_name,$secure
```