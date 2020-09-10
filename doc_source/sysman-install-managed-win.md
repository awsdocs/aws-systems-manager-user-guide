# Step 6: Install SSM Agent for a hybrid environment \(Windows\)<a name="sysman-install-managed-win"></a>

This topic describes how to install SSM Agent on Windows Server machines in a hybrid environment\. If you plan to use Linux machines in a hybrid environment, see the next step, [Step 5: Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\.

**Important**  
This procedure is for servers and virtual machines \(VMs\) in an on\-premises or hybrid environment\. To download and install SSM Agent on an EC2 instance for Windows Server, see [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation earlier in [Step 4: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\. You specify the Code and ID in the following procedure\.

**To install SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. Open Windows PowerShell in elevated \(administrative\) mode\.

1. Copy and paste the following command block into Windows PowerShell\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download SSM Agent from\.

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

------
#### [ 64\-bit ]

   ```
   $code = "activation-code"
   $id = "activation-id"
   $region = "region"
   $dir = $env:TEMP + "\ssm"
   New-Item -ItemType directory -Path $dir -Force
   cd $dir
   (New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
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
   (New-Object System.Net.WebClient).DownloadFile("https://amazon-ssm-$region.s3.amazonaws.com/latest/windows_386/AmazonSSMAgentSetup.exe", $dir + "\AmazonSSMAgentSetup.exe")
   Start-Process .\AmazonSSMAgentSetup.exe -ArgumentList @("/q", "/log", "install.log", "CODE=$code", "ID=$id", "REGION=$region") -Wait
   Get-Content ($env:ProgramData + "\Amazon\SSM\InstanceData\registration")
   Get-Service -Name "AmazonSSMAgent"
   ```

------

1. Press Enter\.

The command does the following: 
+ Downloads and installs SSM Agent onto the server or VM\.
+ Registers the server or VM with the SSM service\.
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

The server or VM is now a managed instance\. These instances are now identified with the prefix "mi\-"\. You can view managed instances on the **Managed Instances** page in the Systems Manager console, by using the AWS CLI command [describe\-instance\-information](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-information.html), or by using the API command [DescribeInstancePatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstancePatches.html)\.

**Note**  
You can deregister a managed instance by calling the [DeregisterManagedInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterManagedInstance.html) API action from either the AWS CLI or Tools for Windows PowerShell\. Here's an example CLI command:  

```
aws ssm deregister-managed-instance --instance-id "mi-1234567890"
```