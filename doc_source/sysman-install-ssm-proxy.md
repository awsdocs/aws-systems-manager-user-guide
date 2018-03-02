# Configure SSM Agent to Use a Proxy for Windows Instances<a name="sysman-install-ssm-proxy"></a>

The information in this topic applies to Windows Server instances created in or after November 2016 that do *not* use the Nano installation option\.

If your instance is a Windows Server 2003\-2012 R2 instance created *before* November 2016, then EC2Config processes Systems Manager requests on your instance\. For information about configuring EC2Config to use a proxy, see [Configure Proxy Settings for the EC2Config Service](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html#ec2config-proxy)\. 

For Windows Server 2016 instances that use the Nano installation option \(Nano Server\), you must connect using PowerShell\. For more information, see [Connect to a Windows Server 2016 Nano Server Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting-nano.html)\.

**To configure SSM Agent to use a proxy**

1. Using Remote Desktop or Windows PowerShell, connect to the instance that you would like to configure to use a proxy\. 

1. Run the following command block in PowerShell\. Replace *hostname* and *port* with the information about your proxy:

   ```
   $serviceKey = "HKLM:\SYSTEM\CurrentControlSet\Services\AmazonSSMAgent"
   $keyInfo = (Get-Item -Path $serviceKey).GetValue("Environment")
   $proxyVariables = @("http_proxy=hostname:port", "no_proxy=169.254.169.254")
   
   If($keyInfo -eq $null)
   {
   New-ItemProperty -Path $serviceKey -Name Environment -Value $proxyVariables -PropertyType MultiString -Force
   } else {
   Set-ItemProperty -Path $serviceKey -Name Environment -Value $proxyVariables
   }
   Restart-Service AmazonSSMAgent
   ```

**To reset SSM Agent proxy configuration**

1. Using Remote Desktop or Windows PowerShell, connect to the instance to configure\.

1. If you connected using Remote Desktop, launch PowerShell as an administrator\.

1. Run the following command block in PowerShell:

   ```
   Remove-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\AmazonSSMAgent -Name Environment
   Restart-Service AmazonSSMAgent
   ```