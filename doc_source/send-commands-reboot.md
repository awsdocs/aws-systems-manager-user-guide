# Rebooting managed instance from scripts<a name="send-commands-reboot"></a>

If the scripts that you run by using Run Command reboot managed instances, then you must specify an exit code in your script\. If you attempt to reboot an instance from a script by using some other mechanism, the script execution status might not be updated correctly, even if the reboot is the last step in your script\. For Windows managed instances, you specify `exit 3010` in your script\. For Linux managed instances, you specify `exit 194`\. The exit code instructs SSM Agent to reboot the managed instance, and then restart the script after the reboot completed\. Before starting the reboot, SSM Agent informs the Systems Manager service in the cloud that communication will be disrupted during the server reboot\.

**Create idempotent scripts**

When developing scripts that reboot managed instances, make the scripts idempotent so the script execution continues where it left off after the reboot\. Idempotent scripts manage state and validate if the action was performed or not\. This prevents a step from running multiple times when it is only intended to run once\.

Here is an outline example of an idempotent script the reboots the instance multiple times\.

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

The following script samples use exit codes to restart instances\. The Linux example installs package updates on Amazon Linux, and then restarts the instance\. The Windows example installs the Telnet\-Client on the instance, and then restarts the instance\. 

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

**Windows example**

```
$telnet = Get-WindowsFeature -Name Telnet-Client
if (-not $telnet.Installed)
	{ 
		# Install Telnet and then send a reboot request to SSM Agent.
		Install-WindowsFeature -Name "Telnet-Client"
		exit 3010 
	}
```