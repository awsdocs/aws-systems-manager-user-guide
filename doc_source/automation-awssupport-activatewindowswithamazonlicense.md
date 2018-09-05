# AWSSupport-ActivateWindowsWithAmazonLicense

## Description

The AWSSupport-ActivateWindowsWithAmazonLicense automation document activates an Amazon EC2 Windows Server instance with a license provided by Amazon.
The automation execution runs a command to check the current status of the activation and then proceeds only if Windows is not activated, or if the end user forces the activation.
If Windows is not activated, the command execution verifies the Windows route table (the route to Amazon KMS servers). If needed, the execution also repairs the Windows route table. The command execution also verifies the KMS server and port settings, and then attempts to activate Windows.

More specifically, the command execution restores the Amazon KMS servers (169.254.169.250 or 169.254.169.251), verifies that they can be reached from the instance on port 1688, and finally activates Windows using the Microsoft-provided Volume License Key for the specific OS version:

<https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)>

Note: this document can't be used on Bring Your Own License (BYOL) Windows instances. If you want to use your own license, please see <https://aws.amazon.com/windows/resources/licensing/>.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

- **Type**: String
- **Allowed values**: ^[m]{0,1}i-[a-z0-9]{8,17}$
- **Description**: (Required) The ID of the Windows instance you want to activate.

### ForceActivation

- **Type**: String
- **Allowed values**: True, False
- **Default**: False
- **Description**: (Optional) Set it to True if you want to proceed even if Windows is already activated.

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses the permissions of the user that executes this document.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWSSupport-ActivateWindowsWithAmazonLicense --parameters "InstanceId=INSTANCEID"
```

### Send the command with ForceActivation = True

```json
aws ssm start-automation-execution --document-name AWSSupport-ActivateWindowsWithAmazonLicense --parameters "InstanceId=INSTANCEID,ForceActivation=True"
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached.
The user must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to execute the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output.

## Document Version

Document Steps:

1. aws:runPowerShellScript - Executes the PowerShell script to attempt to fix Windows activation.
