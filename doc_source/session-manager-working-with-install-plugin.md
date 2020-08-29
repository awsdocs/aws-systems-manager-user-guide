# \(Optional\) Install the Session Manager Plugin for the AWS CLI<a name="session-manager-working-with-install-plugin"></a>

If you want to use the AWS CLI to start and end sessions that connect you to your managed instances, you must first install the Session Manager plugin on your local machine\. The plugin can be installed on supported versions of Microsoft Windows, macOS, Linux, and Ubuntu Server\.

**Use the Latest Version of the Session Manager Plugin**  
The plugin is updated occasionally with enhanced functionality\. We recommend that you regularly ensure you are using the latest version of the plugin\. For more information, see [ Session Manager Plugin latest version and release history](#plugin-version-history)\.

**Installation Prerequisite**  
AWS CLI version 1\.16\.12 or later must be installed on your local machine in order to use the Session Manager plugin\.

**Topics**
+ [Install the Session Manager Plugin on Windows](#install-plugin-windows)
+ [Install and uninstall the Session Manager Plugin on macOS](#install-plugin-macos)
+ [Install Session Manager Plugin on Linux](#install-plugin-linux)
+ [Install the Session Manager Plugin on Ubuntu Server](#install-plugin-debian)
+ [Verify the Session Manager Plugin installation](#install-plugin-verify)
+ [\(Optional\) enable Session Manager Plugin logging](#install-plugin-configure-logs)
+ [Session Manager Plugin latest version and release history](#plugin-version-history)

## Install the Session Manager Plugin on Windows<a name="install-plugin-windows"></a>

You can install the Session Manager plugin on Microsoft Windows Vista or later using the standalone installer\.

When updates are released, you must repeat the installation process to get the latest version of the Session Manager plugin\.

**Note**  
For best results, we recommend starting sessions on Windows clients using the Windows PowerShell application version 5 or later\. On Microsoft Windows 10, the Command Prompt also provides reliable support for Session Manager operations\.

**To install the Session Manager plugin using the EXE installer**

1. Download the installer using the following URL:

   ```
   https://s3.amazonaws.com/session-manager-downloads/plugin/latest/windows/SessionManagerPluginSetup.exe
   ```

1. Run the downloaded installer and follow the on\-screen instructions\.

   Leave the install location box blank to install the plugin to the default directory:
   +  `C:\%PROGRAMFILES%\Amazon\SessionManagerPlugin\bin\` 

1. Verify that the installation was successful\. For information, see [Verify the Session Manager Plugin installation](#install-plugin-verify)\.
**Note**  
If Windows is unable to find the executable, you might need to re\-open the command prompt or add the installation directory to your `PATH` environment variable manually\. For information, see the troubleshooting topic [Session Manager Plugin not automatically added to command line path \(Windows\)](session-manager-troubleshooting.md#windows-plugin-env-var-not-set)\.

## Install and uninstall the Session Manager Plugin on macOS<a name="install-plugin-macos"></a>

You can install the Session Manager plugin on macOS using the bundled installer\.

**Important**  
The bundled installer does not support installing to paths that contain spaces\.

**To install the Session Manager plugin using the bundled installer \(macOS\)**

1. Download the bundled installer:

   ```
   curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac/sessionmanager-bundle.zip" -o "sessionmanager-bundle.zip"
   ```

1. Unzip the package:

   ```
   unzip sessionmanager-bundle.zip
   ```

1. Run the install command:

   ```
   sudo ./sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin
   ```
**Note**  
The plugin requires Python 2\.6\.5 or later or Python 3\.3\. By default, the install script runs under the system default version of Python\. If you have installed an alternative version of Python and want to use that to install the Session Manager plugin, run the install script with that version by absolute path to the Python executable\. The following is an example\.  

   ```
   sudo /usr/local/bin/python3.6 sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin
   ```

   The installer installs the Session Manager plugin at `/usr/local/sessionmanagerplugin` and creates the symlink `session-manager-plugin` in the `/usr/local/bin` directory\. This eliminates the need to specify the install directory in the user's `$PATH` variable\.

   To see an explanation of the \-i and \-b options, use the \-h option:

   ```
   ./sessionmanager-bundle/install -h
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager Plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run the following two commands, one at a time:  

```
sudo rm -rf /usr/local/sessionmanagerplugin
```

```
sudo rm /usr/local/bin/session-manager-plugin
```

## Install Session Manager Plugin on Linux<a name="install-plugin-linux"></a>

1. Download the Session Manager plugin RPM package:
   + 64\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```
   + 32\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_32bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```
   + ARM 64\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_arm64/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```

1. Run the install command:

   ```
   sudo yum install -y session-manager-plugin.rpm
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager Plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run `sudo yum erase session-manager-plugin -y`

## Install the Session Manager Plugin on Ubuntu Server<a name="install-plugin-debian"></a>

1. Download the Session Manager plugin deb package:
   + 64\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```
   + 32\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_32bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```
   + ARM 64\-bit:

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_arm64/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```

1. Run the install command:

   ```
   sudo dpkg -i session-manager-plugin.deb
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager Plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run `sudo dpkg -r session-manager-plugin`

## Verify the Session Manager Plugin installation<a name="install-plugin-verify"></a>

Run the following commands to verify that the Session Manager plugin installed successfully:

```
session-manager-plugin
```

If the installation was successful, the following message is returned:

```
The Session Manager plugin is installed successfully. Use the AWS CLI to start a session.
```

You can also test the installation by running the following command in the AWS CLI:

**Note**  
This command will work only if your Session Manager administrator has granted you the necessary IAM permissions to access the target instance using Session Manager\.

```
aws ssm start-session --target id-of-an-instance-you-have-permissions-to-access
```

## \(Optional\) enable Session Manager Plugin logging<a name="install-plugin-configure-logs"></a>

The Session Manager plugin includes an option to enable logging for sessions that you run\. By default, logging is disabled\.

If you enable logging, the Session Manager plugin creates log files for both application activity \(`session-manager-plugin.log`\) and errors \(`errors.log`\) on your local machine\.

**Topics**
+ [Enable logging for the Session Manager Plugin \(Windows\)](#configure-logs-windows)
+ [Enable logging for the Session Manager Plugin \(Linux and macOS\)](#configure-logs-linux)

### Enable logging for the Session Manager Plugin \(Windows\)<a name="configure-logs-windows"></a>

1. Locate the `seelog.xml.template` file for the plugin\. 

   The default location is `C:\Program Files\Amazon\SessionManagerPlugin\seelog.xml.template`\.

1. Change the name of the file to `seelog.xml`\.

1. Open the file and change `minlevel="off"` to `minlevel="info"` or `minlevel="debug"`\.
**Note**  
By default, log entries about opening a data channel and reconnecting sessions are recorded at the INFO level\. Data flow \(packets and acknowledgement\) entries are recorded at the DEBUG level\.

1. Change other configuration options you want to modify\. Options you can change include: 
   + **Debug level**: You can change the debug level from `formatid="fmtinfo"` to `outputs formatid="fmtdebug"`\.
   + **Log file options**: You can make changes to the log file options, including where the logs are stored, with the exception of the log file names\. 
**Important**  
Do not change the file names or logging will not work correctly\.

     ```
     <rollingfile type="size" filename="C:\%PROGRAMDATA%\Amazon\SessionManagerPlugin\Logs\session-manager-plugin.log" maxsize="30000000" maxrolls="5"/>
     <filter levels="error,critical" formatid="fmterror">
     <rollingfile type="size" filename="C:\%PROGRAMDATA%\Amazon\SessionManagerPlugin\Logs\errors.log" maxsize="10000000" maxrolls="5"/>
     ```

1. Save the file\.

### Enable logging for the Session Manager Plugin \(Linux and macOS\)<a name="configure-logs-linux"></a>

1. Locate the `seelog.xml.template` file for the plugin\. 

   The default location is `/usr/local/sessionmanagerplugin/seelog.xml.template`\.

1. Change the name of the file to `seelog.xml`\.

1. Open the file and change `minlevel="off"` to `minlevel="info"` or `minlevel="debug"`\.
**Note**  
By default, log entries about opening data channels and reconnecting sessions are recorded at the INFO level\. Data flow \(packets and acknowledgement\) entries are recorded at the DEBUG level\.

1. Change other configuration options you want to modify\. Options you can change include: 
   + **Debug level**: You can change the debug level from `formatid="fmtinfo"` to `outputs formatid="fmtdebug"`
   + **Log file options**: You can make changes to the log file options, including where the logs are stored, with the exception of the log file names\. 
**Important**  
Do not change the file names or logging will not work correctly\.

     ```
     <rollingfile type="size" filename="/usr/local/sessionmanagerplugin/logs/session-manager-plugin.log" maxsize="30000000" maxrolls="5"/>
     <filter levels="error,critical" formatid="fmterror">
     <rollingfile type="size" filename="/usr/local/sessionmanagerplugin/logs/errors.log" maxsize="10000000" maxrolls="5"/>
     ```
**Important**  
If you use the specified default directory for storing logs, you must either run session commands using sudo or give the directory where the plugin is installed full read and write permissions\. To bypass these restrictions, change the location where logs are stored\.

1. Save the file\.

## Session Manager Plugin latest version and release history<a name="plugin-version-history"></a>

Your local machine must be running a supported version of the Session Manager plugin\. If you are running an earlier version, your Session Manager operations might not succeed\. 

The current minimum supported version is 1\.1\.17\.0\. 

To see if you have the latest version, run the following command in the AWS CLI:

**Note**  
The command returns results only if the plugin is located in the default installation directory for your operating system type\. You can also check the version in the contents of the `VERSION` file in the directory where you have installed the plugin\.

```
session-manager-plugin --version
```

The following table lists all releases of the Session Manager plugin and the features and enhancements included with each version\.


| Version | Release date | Details | 
| --- | --- | --- | 
| 1\.1\.61\.0 |  April 17, 2020  |  **Enhancement**: Added ARM support for Linux and Ubuntu Server\.   | 
| 1\.1\.54\.0 |  January 6, 2020  |  **Bug fix**: Handle race condition scenario of packets being dropped when the Session Manager plugin is not ready\.   | 
|  1\.1\.50\.0  | November 19, 2019 |  **Enhancement**: Added support for forwarding a port to a local unix socket\.  | 
|  1\.1\.35\.0  | November 7, 2019 |  **Enhancement**: \(Port forwarding sessions only\) Send a TerminateSession command to SSM Agent when the local user presses Ctrl\+C\.  | 
| 1\.1\.33\.0 | September 26, 2019 | Enhancement: \(Port forwarding sessions only\) Send a disconnect signal to the server when the client drops the TCP connection\.  | 
| 1\.1\.31\.0 | September 6, 2019 | Enhancement: Update to keep port forwarding session open until remote server closes the connection\. | 
|  1\.1\.26\.0  | July 30, 2019 |  **Enhancement**: Update to limit the rate of data transfer during a session\.  | 
|  1\.1\.23\.0  | July 9, 2019 |  **Enhancement**: Added support for running SSH sessions using Session Manager\.  | 
| 1\.1\.17\.0 | April 4, 2019 |  **Enhancement**: Added support for further encryption of session data using AWS Key Management Service \(AWS KMS\)\.  | 
| 1\.0\.37\.0 | September 20, 2018 |  **Enhancement**: Bug fix for Windows version\.  | 
| 1\.0\.0\.0 | September 11, 2018 |  Initial release of the Session Manager plugin\.  | 