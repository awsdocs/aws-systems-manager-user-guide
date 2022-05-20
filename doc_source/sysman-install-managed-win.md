# Step 5: Install SSM Agent for a hybrid environment \(Windows\)<a name="sysman-install-managed-win"></a>

This topic describes how to install SSM Agent on Windows Server machines in a hybrid environment\. If you plan to use Linux machines in a hybrid environment, see the previous step, [Step 4: Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\.

**Important**  
This procedure is for servers and virtual machines \(VMs\) in an on\-premises or hybrid environment\. To download and install SSM Agent on an EC2 instance for Windows Server, see [Working with SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation earlier in [Step 3: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\. You specify the Code and ID in the following procedure\.

**To install SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. If you use an HTTP or HTTPS proxy, you must set the `http_proxy` or `https_proxy` environment variables in the current shell session\. If you aren't using a proxy, you can skip this step\.

   For an HTTP proxy server, set this variable:

   ```
   http_proxy=http://hostname:port
   https_proxy=http://hostname:port
   ```

   For an HTTPS proxy server, set this variable:

   ```
   http_proxy=http://hostname:port
   https_proxy=https://hostname:port
   ```

1. Open Windows PowerShell in elevated \(administrative\) mode\.

1. Copy and paste the following command block into Windows PowerShell\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download SSM Agent from\.

   In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

------
#### [ 64\-bit ]

   ```
   $code = "activation-code"
   $id = "activation-id"
   $region = "region"
   $dir = $env:TEMP + "\ssm"
   New-Item -ItemType directory -Path $dir -Force
   cd $dir
   (New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.$region.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
   Start-Process .\AmazonSSMAgentSetup.exe -ArgumentList @("/q", "/log", "install.log", "CODE=$code", "ID=$id", "REGION=$region") -Wait
   Get-Content ($env:ProgramData + "\Amazon\SSM\InstanceData\registration")
   Get-Service -Name "AmazonSSMAgent"
   ```

------
#### [ 32\-bit ]

   ```
   $code = "activation-code"
   $id = "activation-id"
   $region = "region"
   $dir = $env:TEMP + "\ssm"
   New-Item -ItemType directory -Path $dir -Force
   cd $dir
   (New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.$region.amazonaws.com/latest/windows_386/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
   Start-Process .\AmazonSSMAgentSetup.exe -ArgumentList @("/q", "/log", "install.log", "CODE=$code", "ID=$id", "REGION=$region") -Wait
   Get-Content ($env:ProgramData + "\Amazon\SSM\InstanceData\registration")
   Get-Service -Name "AmazonSSMAgent"
   ```

------

1. Press Enter\.

The command does the following: 
+ Downloads and installs SSM Agent onto the server or VM\.
+ Registers the server or VM with the Systems Manager service\.
+ Returns a response to the request similar to the following:

  ```
      Directory: C:\Users\ADMINI~1\AppData\Local\Temp\2
  
  
  Mode                LastWriteTime         Length Name
  ----                -------------         ------ ----
  d-----       07/07/2018   8:07 PM                ssm
  {"ManagedInstanceID":"mi-008d36be46EXAMPLE","Region":"us-east-2"}
  
  Status      : Running
  Name        : AmazonSSMAgent
  DisplayName : Amazon SSM Agent
  ```

The server or VM is now a managed instance\. These instances are now identified with the prefix "mi\-"\. You can view managed instances on the **Managed instances** page in the Fleet Manager console, by using the AWS CLI command [describe\-instance\-information](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-information.html), or by using the API command [DescribeInstancePatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstancePatches.html)\.

## Setting up private key auto rotation<a name="ssm-agent-hybrid-private-key-rotation-windows"></a>

To strengthen your security posture, you can configure AWS Systems Manager Agent \(SSM Agent\) to rotate the hybrid environment private key automatically\. You can access this feature using SSM Agent version 3\.0\.1031\.0 or later\. Turn on this feature using the following procedure\.

**To configure SSM Agent to rotate the hybrid environment private key**

1. Navigate to `/etc/amazon/ssm/` on a Linux machine or `C:\Program Files\Amazon\SSM` for a Windows machine\.

1. Copy the contents of `amazon-ssm-agent.json.template` to a new file named `amazon-ssm-agent.json`\. Save `amazon-ssm-agent.json` in the same directory where `amazon-ssm-agent.json.template` is located\.

1. Find `Profile`, `KeyAutoRotateDays`\. Enter the number of days that you want between automatic private key rotations\. 

1. Restart SSM Agent\.

Every time you change the configuration, restart SSM Agent\.

You can customize other features of SSM Agent using the same procedure\. For an up\-to\-date list of the available configuration properties and their default values, see [Config Property Definitions](https://github.com/aws/amazon-ssm-agent#config-property-definitions)\. 

## Deregister and reregister a managed instance<a name="systems-manager-install-managed-win-deregister-reregister"></a>

You can deregister a managed instance by calling the [DeregisterManagedInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterManagedInstance.html) API operation from either the AWS CLI or Tools for Windows PowerShell\. Here's an example CLI command:

`aws ssm deregister-managed-instance --instance-id "mi-1234567890"`

You can reregister a managed instance after you deregistered it\. Use the following procedure to reregister a managed instance\. After you complete the procedure, your managed instance is displayed again in the list of managed instances\.

**To reregister a managed instance on a Windows hybrid machine**

1. Connect to your instance\.

1. Run the following command\. Be sure to replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the Region you want to download the SSM Agent from\.

   ```
   'yes' | & 'C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe' -register -code activation-code -id activation-id -region region; Restart-Service AmazonSSMAgent
   ```