# Configure SSM Agent to use a proxy<a name="sysman-proxy-with-ssm-agent"></a>

You can configure SSM Agent to communicate through an HTTP proxy by creating an override configuration file and adding `http_proxy`, `https_proxy`, and `no_proxy` settings to the file\. An override file also preserves the proxy settings if you install newer or older versions of SSM Agent\. This section includes procedures for creating an override file in both *upstart* and *systemd* environments\. 

**Note**  
Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.

**Topics**
+ [Configure SSM Agent to use a proxy \(upstart\)](#ssm-agent-proxy-upstart)
+ [Configure SSM Agent to use a proxy \(systemd\)](#ssm-agent-proxy-systemd)
+ [Upgrade the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)

## Configure SSM Agent to use a proxy \(upstart\)<a name="ssm-agent-proxy-upstart"></a>

Use the following procedure to create an override configuration file for an `upstart` environment\.

**To configure SSM Agent to use a proxy \(upstart\)**

1. Connect to the instance where you installed SSM Agent\.

1. Open a simple editor like VIM, and depending on whether you're using an HTTP proxy server or HTTPS proxy server, add one of the following configurations\.

   **For an HTTP proxy server:**

   ```
   env http_proxy=http://hostname:port
   env https_proxy=http://hostname:port
   env no_proxy=169.254.169.254
   ```

   **For an HTTPS proxy server:**

   ```
   env http_proxy=http://hostname:port
   env https_proxy=https://hostname:port
   env no_proxy=169.254.169.254
   ```
**Note**  
You must add the `no_proxy` setting to the file and specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager\. Without this IP address, calls to Systems Manager fail\.

1. Save the file with the name `amazon-ssm-agent.override` in the following location: `/etc/init/`

1. Stop and restart SSM Agent using the following commands:

   ```
   sudo stop amazon-ssm-agent
   sudo start amazon-ssm-agent
   ```

**Note**  
For more information about working with `.override` files in Upstart environments, see [init: Upstart init daemon job configuration](https://www.systutorials.com/docs/linux/man/5-init/)\.

## Configure SSM Agent to use a proxy \(systemd\)<a name="ssm-agent-proxy-systemd"></a>

Use the following procedure to configure SSM Agent to use a proxy in a `systemd` environment\.

**Note**  
Some of the steps in this procedure contain explicit instructions for Ubuntu Server instances where SSM Agent was installed using Snap\.

1. Connect to the instance where you installed SSM Agent\.

1. Run one of the follow commands, depending on the operating system type\.
   + On Ubuntu Server instances where SSM Agent is installed by using a snap:

     ```
     systemctl edit snap.amazon-ssm-agent.amazon-ssm-agent
     ```

     On other operating systems:

     ```
     systemctl edit amazon-ssm-agent
     ```

1. Open a simple editor like VIM, and depending on whether you're using an HTTP proxy server or HTTPS proxy server, add one of the following configurations\.

   **For an HTTP proxy server:**

   ```
   [Service]
   Environment="http_proxy=http://hostname:port"
   Environment="https_proxy=http://hostname:port"
   Environment="no_proxy=169.254.169.254"
   ```

   **For an HTTPS proxy server:**

   ```
   [Service]
   Environment="http_proxy=http://hostname:port"
   Environment="https_proxy=https://hostname:port"
   Environment="no_proxy=169.254.169.254"
   ```
**Note**  
You must add the `no_proxy` setting to the file and specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager\. Without this IP address, calls to Systems Manager fail\.

1. Save your changes\. The system automatically creates one of the following files, depending on the operating system type\.
   + On Ubuntu Server instances where SSM Agent is installed by using a snap: 

     `etc/systemd/system/snap.amazon-ssm-agent.amazon-ssm-agent.service.d/override.conf`
   + On Amazon Linux 2 instances: 

     `etc/systemd/system/amazon-ssm-agent.service.d/override.conf`
   + On other operating systems: 

     `etc/systemd/system/amazon-ssm-agent.service.d/amazon-ssm-agent.override`

1. Restart SSM Agent by using one of the following commands, depending on the operating system type\.
   + On Ubuntu Server instances installed by using a snap:

     ```
     systemctl start snap.amazon-ssm-agent.amazon-ssm-agent
     ```
   + On other operating systems:

     ```
     sudo systemctl stop amazon-ssm-agent ; sudo systemctl daemon-reload
     ```

**Note**  
For more information about working with `.override` files in systemd environments, see [Modifying Existing Unit Files](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-managing_services_with_systemd#sect-Managing_Services_with_systemd-Unit_File_Modify) in the *Red Hat Enterprise Linux 7 System Administrator's Guide*\.