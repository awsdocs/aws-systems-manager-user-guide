# Configure SSM Agent to Use a Proxy<a name="sysman-proxy-with-ssm-agent"></a>

You can configure SSM Agent to communicate through an HTTP proxy by adding the `http_proxy` and `no_proxy` settings to the SSM Agent configuration file\. This section includes procedures for *upstart* and *systemd* environments\. 

**Note**  
Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.


+ [Configure SSM Agent to Use a Proxy \(Upstart\)](#ssm-agent-proxy-upstart)
+ [Configure SSM Agent to Use a Proxy \(systemd\)](#ssm-agent-proxy-systemd)
+ [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)

## Configure SSM Agent to Use a Proxy \(Upstart\)<a name="ssm-agent-proxy-upstart"></a>

1. Connect to the instance where you installed SSM Agent\.

1. Open the `amazon-ssm-agent.conf` file in an editor such as VIM\. The default location of the the file is:

   `/etc/init/amazon-ssm-agent.conf`

1. Add the following settings to the file:

   ```
   env http_proxy=http://hostname:port
   env https_proxy=http://hostname:port
   env HTTP_PROXY=http://hostname:port
   env HTTPS_PROXY=http://hostname:port
   ```

1. Add the `no_proxy` setting to the file in the following format\. You must specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager and without this IP address calls to Systems Manager fail:

   ```
   env no_proxy=169.254.169.254
   ```

1. Save your changes and close the editor\.

1. Stop and restart SSM Agent using the following commands:

   ```
   sudo stop amazon-ssm-agent
   sudo start amazon-ssm-agent
   ```

The following Upstart example includes the `http_proxy` and `no_proxy` settings in the `amazon-ssm-agent.conf` file:

```
description "Amazon SSM Agent"
author "Amazon.com"

start on (runlevel [345] and started network)
stop on (runlevel [!345] or stopping network)

respawn

env http_proxy=http://i-1234567890abcdef0:443
env no_proxy=169.254.169.254

chdir /usr/bin/
exec ./amazon-ssm-agent
```

## Configure SSM Agent to Use a Proxy \(systemd\)<a name="ssm-agent-proxy-systemd"></a>

1. Connect to the instance where you installed SSM Agent\.

1. Open the `amazon-ssm-agent.service` file in an editor such as VIM\. The default location of the file is:

   `/etc/systemd/system/amazon-ssm-agent.service`

1. Add the following settings to the file:

   ```
   Environment="http_proxy=http://hostname:port"
   Environment="https_proxy=http://hostname:port"
   Environment="HTTP_PROXY=http://hostname:port"
   Environment="HTTPS_PROXYy=http://hostname:port"
   ```

1. Add the `no_proxy` setting to the file in the following format\. You must specify the IP address listed here\. It is the instance metadata endpoint for Systems Manager and without this IP address calls to Systems Manager fail:

   ```
   Environment="no_proxy=169.254.169.254"
   ```

1. Save your changes and close the editor\.

1. Restart SSM Agent using the following commands:

   ```
   sudo systemctl stop amazon-ssm-agent
   sudo systemctl daemon-reload
   ```

The following systemd example includes the `http_proxy` and `no_proxy` settings in the `amazon-ssm-agent.service` file:

```
Type=simple
Environment="HTTP_PROXY=http://i-1234567890abcdef0:443"
Environment="no_proxy=169.254.169.254"
WorkingDirectory=/opt/amazon/ssm/
ExecStart=/usr/bin/amazon-ssm-agent
KillMode=process
Restart=on-failure
RestartSec=15min
```