# Install SSM Agent on Servers and VMs in a Windows Hybrid Environment<a name="sysman-install-managed-win"></a>

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation in the previous section\. You will specify the Code and ID in the following procedure\.

**Important**  
This procedure is for servers and VMs in an on\-premises or hybrid environment\. To download and install SSM Agent on an Amazon EC2 Windows instance, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.

**To install SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. Open Windows PowerShell\. 

1. Copy and paste the following command block into AWS Tools for Windows PowerShell\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download SSM Agent from\.

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

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

The server or VM is now a managed instance\. In the console, these instances are listed with the prefix "mi\-"\. You can view all instances using a `List` command\. For more information, see the [Amazon EC2 Systems Manager API Reference](http://docs.aws.amazon.com/ssm/latest/APIReference/)\.