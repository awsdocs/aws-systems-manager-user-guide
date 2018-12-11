# Rebooting Managed Instance from Scripts<a name="send-commands-reboot"></a>

If the scripts that you run by using Run Command reboot managed instances, then you must specify an exit code in your script\. If you attempt to reboot an instance from a script by using some other mechanism, the script execution status might not be updated correctly, even if the reboot is the last step in your script\. For Windows managed instances, you specify `exit 3010` in your script\. For Linux managed instances, you specify `exit 194`\. The exit code instructs SSM Agent to reboot the managed instance, and then restart the script after the reboot completed\. Before starting the reboot, SSM Agent informs the Systems Manager service in the cloud that communication will be disrupted during the server reboot\.

**Create idempotent scripts**

When developing scripts that reboot manged instances, make the scripts idempotent so that script execution continues where it left off after the reboot\. 

Here is an outline example of an idempotent script the reboots the instance mutliple times\.

```
$name = Get current computer name
If ($name –ne $desiredName) 
	{
		Rename computer
		exit 3010
	}
            
$domain = Get current domain name
If ($domain –ne $desiredDomain) 
	{
		Join domain
		exit 3010
	}
            
If (desired package not installed) 
	{
		Install package
		exit 3010
	}
```

The following script samples use exit codes to restart instances\. The Windows example installs the Hypver\-V application on the instance, and then restarts the intance\. The Linux example installs package updates on Amazon Linux, and then restarts the instance\. 

**Windows example**

```
$hyperV = Get-WindowsFeature -Name Hyper-V
if(-not $hyperV.Installed)
	{ 
		# Install Hyper-V and then send a reboot request to SSM Agent.
		Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
		exit 3010 
	}
```

**Amazon Linux example**

```
#!/bin/bash
yum -y update
needs-restarting -r
if [ $? -eq 1 ]
then
        exit 194
else
        exit 0
fi
```