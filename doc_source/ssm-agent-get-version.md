# Checking the SSM Agent version number<a name="ssm-agent-get-version"></a>

Certain Systems Manager functionalities have prerequisites that include a minimum SSM Agent version be installed on your managed instances\. You can get the currently installed SSM Agent version on your managed instances using the AWS Systems Manager console, or by logging in to your instances\.

The following procedures describe how to get the currently installed SSM Agent version on your managed instances\.

**To check the version number of SSM Agent installed on an instance**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Note the **Agent version**\.

**To get the currently installed SSM Agent version from within the operating system**

**Windows**

1. Log in to your instance\.

1. Run the following PowerShell command\.

   ```
   Get-WmiObject Win32_Product | Where-Object {$_.Name -eq 'Amazon SSM Agent'} | Select-Object Name,Version
   ```

**Linux**
**Note**  
This command varies depending on the package manager for your operating system\.

1. Log in to your instance\.

1. Run the following command for Amazon Linux and Amazon Linux 2\.

   ```
   yum info amazon-ssm-agent
   ```

We recommend using the latest version of the SSM Agent so you can benefit from new or updated capabilities\. To ensure your managed instances are always running the most up\-to\-date version of the SSM Agent, you can automate the process of updating the SSM Agent\. For more information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\.