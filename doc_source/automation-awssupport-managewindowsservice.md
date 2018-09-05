# AWSSupport-ManageWindowsService

## Description

The AWSSupport-ManageWindowsService automation document enables a user to stop, start, restart, pause, or disable any Windows service on the target instance.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

- **Type**: String
- **Allowed pattern**: ^[m]{0,1}i-[a-z0-9]{8,17}$
- **Description**: (Required) The ID of the managed instance to manage the RDP settings of.

### WindowsServiceName

- **Type**: String
- **Description**: (Required) A valid Windows service name.

### StartupType

- **Type**: String
- **Allowed values**: Auto, Demand, Disabled, DelayedAutoStart, Check
- **Default**: Check
- **Description**: (Required) A start type to apply to the Windows service: Auto, Demand, Disabled, DelayedAutoStart, Check.

### ServiceAction

- **Type**: String
- **Allowed values**: Restart, Force-Restart, Start, Stop, Force-Stop, Pause, Check
- **Default**: Check
- **Description**: (Required) An action to apply to the Windows service: Restart, Force-Restart, Start, Stop, Force-Stop, Pause, Check. Note: Force-Restart and Force-Stop can be used to restart and to stop a service that has dependent services.

## Examples

### Check RDP Settings

```json
aws ssm start-automation-execution --document-name "AWSSupport-ManageWindowsService" --parameters "InstanceId=INSTANCEID, WindowsServiceName=WINDOWSSERVICE"

```

### Change the Startup Type of the TermService to Auto and change Service Action to Start

```json
aws ssm start-automation-execution --document-name "AWSSupport-ManageWindowsService" --parameters "InstanceId=INSTANCEID, WindowsServiceName=TermService, StartupType=Auto, ServiceAction=Start"

```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached. The user must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to execute the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output.

## Document Version

Document Steps:

1. **aws:runPowerShellScript** - Executes the PowerShell script to apply the desired configuration to the Windows service on the target instance.
