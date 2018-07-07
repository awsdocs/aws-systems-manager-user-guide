# Configure SSM Agent to Use a Proxy<a name="sysman-proxy-with-ssm-agent"></a>

You can configure SSM Agent to communicate through an HTTP proxy by adding the `http_proxy`, `https_proxy`, and `no_proxy` settings to an amazon\-ssm\-agent\.override configuration file\. An override file also preserves the proxy settings if you install newer or older versions of SSM Agent\. This section includes procedures for *upstart* and *systemd* environments\. 

**Note**  
Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.

**Topics**
+ [Configure SSM Agent to Use a Proxy \(Upstart\)](#ssm-agent-proxy-upstart)
+ [Configure SSM Agent to Use a Proxy \(systemd\)](#ssm-agent-proxy-systemd)
+ [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)

## Configure SSM Agent to Use a Proxy \(Upstart\)<a name="ssm-agent-proxy-upstart"></a>

1. Connect to the instance where you installed SSM Agent\.

1. Open a simple editor like VIM, and specify the following settings:

   ```
   env http_proxy=http://hostname:port
   env https_proxy=https://hostname:port
   env no_proxy=169.254.169.254
   ```
**Note**  
You must add the `no_proxy` setting to the file and specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager\. Without this IP address, calls to Systems Manager fail\.

1. Save the file as `amazon-ssm-agent.override` in the following location: /etc/init/

1. Stop and restart SSM Agent using the following commands:

   ```
   sudo stop amazon-ssm-agent
   sudo start amazon-ssm-agent
   ```

**Note**  
For more information about working with \.override files in Upstart environments, see [init: Upstart init daemon job configuration](https://www.systutorials.com/docs/linux/man/5-init/)\.

## Configure SSM Agent to Use a Proxy \(systemd\)<a name="ssm-agent-proxy-systemd"></a>

The steps in the following procedure describe how to configure SSM Agent to use a proxy in systemd environments\. Some of the steps in this procedure contain explicit instructions for Ubuntu Server instances installed by using Snap\.

1. Connect to the instance where you installed SSM Agent\.

1. Execute the following command:

   ```
   systemctl edit amazon-ssm-agent
   ```

   For Ubuntu Server instances installed by using snap, execute the following command:

   ```
   systemctl edit snap.amazon-ssm-agent.amazon-ssm-agent
   ```

1. Specify the following settings:

   ```
   [Service]
   Environment="http_proxy=http://hostname:port"
   Environment="https_proxy=https://hostname:port"
   Environment="no_proxy=169.254.169.254"
   ```
**Note**  
You must add the `no_proxy` setting to the file and specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager\. Without this IP address, calls to Systems Manager fail\.

1. Save your changes\. The system creates an amazon\-ssm\-agent\.override file in the amazon\-ssm\-agent\.service\.d folder\.

1. Restart SSM Agent by using the following commands:

   ```
   sudo systemctl stop amazon-ssm-agent
   sudo systemctl daemon-reload
   ```

   For Ubuntu Server instances installed by using snap, restart SSM Agent by using the following command:

   ```
   systemctl start snap.amazon-ssm-agent.amazon-ssm-agent
   ```

**Note**  
For more information about working with \.override files in systemd environments, see [Modifying Existing Unit Files](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-managing_services_with_systemd-unit_files#sect-Managing_Services_with_systemd-Unit_File_Modify)\.