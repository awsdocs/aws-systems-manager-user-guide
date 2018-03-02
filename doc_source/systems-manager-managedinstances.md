# Setting Up AWS Systems Manager in Hybrid Environments<a name="systems-manager-managedinstances"></a>

AWS Systems Manager lets you remotely and securely manage on\-premises servers and virtual machines \(VMs\) in your hybrid environment\. Configuring your hybrid environment for Systems Manager provides the following benefits\. 

+ Create a consistent and secure way to remotely manage your on\-premises workloads from one location using the same tools or scripts\.

+ Centralize access control for actions that can be performed on your servers and VMs by using AWS Identity and Access Management \(IAM\)\.

+ Centralize auditing and your view into the actions performed on your servers and VMs because all actions are recorded in AWS CloudTrail\.

+ Centralize monitoring because you can configure CloudWatch Events and Amazon SNS to send notifications about service execution success\.

Complete the procedures in this topic to configure your hybrid machines for Systems Manager\.

**Important**  
After you finish, your hybrid machines that are configured for Systems Manager are listed in the Amazon EC2 console and described as *managed instances*\. Amazon EC2 instances configured for Systems Manager are *also* managed instances\. In the Amazon EC2 console, however, your on\-premise instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\.


+ [Create an IAM Service Role](#sysman-service-role)
+ [Create a Managed\-Instance Activation](#sysman-managed-instance-activation)
+ [Install the SSM Agent on Servers and VMs in Your Windows Hybrid Environment](#sysman-install-managed-win)
+ [Install the SSM Agent on Servers and VMs in Your Linux Hybrid Environment](#sysman-install-managed-linux)

## Create an IAM Service Role<a name="sysman-service-role"></a>

Servers and VMs in a hybrid environment require an IAM role to communicate with the Systems Manager SSM service\. The role grants AssumeRole trust to the SSM service\. 

**Note**  
You only need to create the service role once for each AWS account\.

**To create an IAM service role using AWS Tools for Windows PowerShell**

1. Create a text file \(in this example it is named `SSMService-Trust.json`\) with the following trust policy\. Save the file with the `.json` file extension\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": {
       "Effect": "Allow",
       "Principal": {"Service": "ssm.amazonaws.com"},
       "Action": "sts:AssumeRole"
     }
   }
   ```

1. Use [New\-IAMRole](http://docs.aws.amazon.com/powershell/latest/reference/items/New-IAMRole.html) as follows to create a service role\. This example creates a role named SSMServiceRole\.

   ```
   New-IAMRole -RoleName SSMServiceRole -AssumeRolePolicyDocument (Get-Content -raw SSMService-Trust.json)
   ```

1. Use [Register\-IAMRolePolicy](http://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to enable the SSMServiceRole to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.

   ```
   Register-IAMRolePolicy -RoleName SSMServiceRole -PolicyArn arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM
   ```

**To create an IAM service role using the AWS CLI**

1. Create a text file \(in this example it is named `SSMService-Trust.json`\) with the following trust policy\. Save the file with the `.json` file extension\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": {
       "Effect": "Allow",
       "Principal": {"Service": "ssm.amazonaws.com"},
       "Action": "sts:AssumeRole"
     }
   }
   ```

1. Use the [create\-role](http://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the service role\. This example creates a role named SSMServiceRole\.

   ```
   aws iam create-role --role-name SSMServiceRole --assume-role-policy-document file://SSMService-Trust.json 
   ```

1. Use [attach\-role\-policy](http://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) as follows to enable the SSMServiceRole to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.

   ```
   aws iam attach-role-policy --role-name SSMServiceRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM 
   ```

**Note**  
Users in your company or organization who will use Systems Manager on your hybrid machines must be granted permission in IAM to call the SSM API\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

## Create a Managed\-Instance Activation<a name="sysman-managed-instance-activation"></a>

To set up servers and VMs in your hybrid environment as managed instances, you need to create a managed\-instance activation\. After you complete the activation, you receive an Activation Code and Activation ID\. This Code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a managed\-instance activation \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Activations**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Activations**\.

1. Choose **Create activation**\.

1. \(Optional\) In the **Activation description** field, enter a description for this activation\. The description is optional, be we recommend that you enter a description if you plan to activate large numbers of servers and VMs\.

1. In the **Instance limit** field, specify the total number of servers or VMs that you want to register with AWS\. 

1. In the **IAM role name** section, choose a service role option that enables your servers and VMs to communicate with AWS Systems Manager in the cloud:

   1. Choose **Use the system created default command execution role** to use a role and managed policy created by AWS\. 

   1. Choose **Select an existing custom IAM role that has the required permissions** to use the optional custom role you created earlier\.

1. In the **Activation expiry date** field, specify an expiration date for the activation\. 
**Note**  
If you want to register additional managed instances after the expiry date, you must create a new activation\. The expiry date has no impact on registered and running instances\.

1. \(Optional\) In the **Default instance name** field, specify a name\. 

1. Choose **Create activation**\.
**Important**  
Store the managed\-instance Activation Code and Activation ID in a safe place\. You specify this Code and ID when you install SSM Agent on servers and VMs in your hybrid environment\. If you lose the Code and ID, you must create a new activation\.

**To create a managed\-instance activation using the console \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and choose **Activations**\.

1. Choose **Create an Activation**\.

1. Fill out the form and choose **Create Activation**\.

   Note that you can specify a date when the activation expires\. If you want to register additional managed instances after the expiry date, you must create a new activation\. The expiry date has no impact on registered and running instances\.

1. Store the managed\-instance Activation Code and Activation ID in a safe place\. You specify this Code and ID when you install SSM Agent on servers and VMs in your hybrid environment\. If you lose the code and ID, you must create a new activation\.

**To create a managed\-instance activation using the AWS Tools for Windows PowerShell**

1. On a machine with where you have installed AWS Tools for Windows PowerShell, run the following command in AWS Tools for Windows PowerShell\.

   ```
   New-SSMActivation -DefaultInstanceName name -IamRole iam-service-role-name -RegistrationLimit number-of-managed-instances –Region region
   ```

   *region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   For example:

   ```
   New-SSMActivation -DefaultInstanceName MyWebServers -IamRole RunCommandServiceRole -RegistrationLimit 10 –Region us-east-1
   ```

1. Press Enter\. If the activation is successful, the system returns an Activation Code and an Activation ID\. Store the Activation Code and Activation ID in a safe place\.

**To create a managed\-instance activation using the AWS CLI**

1. On a machine where you have installed the AWS Command Line Interface \(AWS CLI\), run the following command in the CLI\.

   ```
   aws ssm create-activation --default-instance-name name --iam-role IAM service role --registration-limit number of managed instances --region region
   ```

   *region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   For example:

   ```
   aws ssm create-activation --default-instance-name MyWebServers --iam-role RunCommandServiceRole --registration-limit 10 --region us-east-1
   ```

1. Press Enter\. If the activation is successful, the system returns an Activation Code and an Activation ID\. Store the Activation Code and Activation ID in a safe place\.

## Install the SSM Agent on Servers and VMs in Your Windows Hybrid Environment<a name="sysman-install-managed-win"></a>

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation in the previous section\. You will specify the Code and ID in the following procedure\.

**Important**  
This procedure is for servers and VMs in an on\-premises or hybrid environment\. To download and install the SSM Agent on an Amazon EC2 Windows instance, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.

**To install the SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. Open Windows PowerShell\. 

1. Copy and paste the following command block into AWS Tools for Windows PowerShell\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download the SSM Agent from\.

   *region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

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

+ Downloads and installs the SSM Agent onto the server or VM\.

+ Registers the server or VM with the SSM service\.

+ Returns a response to the request similar to the following:

  ```
      Directory: C:\Users\ADMINI~1\AppData\Local\Temp\2
  
  
  Mode                LastWriteTime         Length Name
  ----                -------------         ------ ----
  d-----       07/07/2018   8:07 PM                ssm
  {"ManagedInstanceID":"mi-008d36be46EXAMPLE","Region":"us-east-1"}
  
  Status      : Running
  Name        : AmazonSSMAgent
  DisplayName : Amazon SSM Agent
  ```

The server or VM is now a managed instance\. In the console, these instances are listed with the prefix "mi\-"\. You can view all instances using a `List` command\. For more information, see the [Amazon EC2 Systems Manager API Reference](http://docs.aws.amazon.com/ssm/latest/APIReference/)\.

## Install the SSM Agent on Servers and VMs in Your Linux Hybrid Environment<a name="sysman-install-managed-linux"></a>

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation\. You will specify the Code and ID in the following procedure\.

**Important**  
This procedure is for servers and VMs in an on\-premises or hybrid environment\. To download and install the SSM Agent on an Amazon EC2 Linux instance, see [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md)\.

The URLs in the following scripts let you download the SSM Agent from *any* AWS region\. If you want to download the agent from a *specific* region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

For example, to download the SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West \(N\. California\) Region \(us\-west\-1\), use the following URL:

```
https://s3-us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm 
```

If the download fails, try replacing https://s3\-*region* with https://s3\.*region*\.

+ **Amazon Linux, RHEL, CentOS, and SLES 64\-bit**

   https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 

+ **Amazon Linux, RHEL, and CentOS 32\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm

+ **Ubuntu Server 64\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb

+ **Ubuntu Server 32\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb

+ **Raspbian**

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

**To install the SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. Copy and paste one of the following command blocks into SSH\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download the SSM Agent from\. 

    Note that `sudo` is not necessary if you are a root user\.

   *region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   **On Amazon Linux, RHEL 6\.x, and CentOS 6\.x**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
   sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
   sudo stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo start amazon-ssm-agent
   ```

   **On RHEL 7\.x and CentOS 7\.x**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
   sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
   sudo systemctl stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo systemctl start amazon-ssm-agent
   ```

   **On SLES**

   ```
   mkdir /tmp/ssm
   sudo wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   sudo rpm --install amazon-ssm-agent.rpm
   sudo systemctl stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo systemctl enable amazon-ssm-agent
   sudo systemctl start amazon-ssm-agent
   ```

   **On Ubuntu**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   sudo service amazon-ssm-agent stop
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
   sudo service amazon-ssm-agent start
   ```

   **On Raspbian**

   ```
   mkdir /tmp/ssm
   sudo curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   sudo service amazon-ssm-agent stop
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
   sudo service amazon-ssm-agent start
   ```
**Note**  
If you see the following error in the SSM Agent error logs, then the machine ID did not persist after a reboot:  
`Unable to load instance associations, unable to retrieve associations unable to retrieve associations error occurred in RequestManagedInstanceRoleToken: MachineFingerprintDoesNotMatch: Fingerprint does not match`  
Execute the following command to make the machine ID persist after a reboot\.  

   ```
   umount /etc/machine-id
   systemd-machine-id-setup
   ```

1. Press Enter\.

The command downloads and installs the SSM Agent onto the server or VM in your hybrid environment\. The command stops the SSM Agent, and then registers the server or VM with the SSM service\. The server or VM is now a managed instance\. Amazon EC2 instances configured for Systems Manager are also managed instances\. In the Amazon EC2 console, however, your on\-premises instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\.