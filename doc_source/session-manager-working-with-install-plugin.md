# \(Optional\) Install the Session Manager plugin for the AWS CLI<a name="session-manager-working-with-install-plugin"></a>

If you want to use the AWS Command Line Interface \(AWS CLI\) to start and end sessions that connect you to your managed instances, you must first install the Session Manager plugin on your local machine\. The plugin can be installed on supported versions of Microsoft Windows, macOS, Linux, and Ubuntu Server\.

**Use the latest version of the Session Manager plugin**  
The Session Manager plugin is updated occasionally with enhanced functionality\. We recommend that you regularly ensure you're using the latest version of the plugin\. For more information, see [ Session Manager plugin latest version and release history](#plugin-version-history)\.

**Installation prerequisite**  
AWS CLI version 1\.16\.12 or later must be installed on your local machine in order to use the Session Manager plugin\.

**Topics**
+ [Install the Session Manager plugin on Windows](#install-plugin-windows)
+ [Install and uninstall the Session Manager plugin on macOS](#install-plugin-macos)
+ [Install the Session Manager plugin on macOS with the signed installer](#install-plugin-macos-signed)
+ [Install Session Manager plugin on Linux](#install-plugin-linux)
+ [Install the Session Manager plugin on Ubuntu Server](#install-plugin-debian)
+ [Verify the Session Manager plugin installation](#install-plugin-verify)
+ [Session Manager plugin on GitHub](#plugin-github)
+ [\(Optional\) Turn on Session Manager plugin logging](#install-plugin-configure-logs)
+ [Session Manager plugin latest version and release history](#plugin-version-history)

## Install the Session Manager plugin on Windows<a name="install-plugin-windows"></a>

You can install the Session Manager plugin on Microsoft Windows Vista or later using the standalone installer\.

When updates are released, you must repeat the installation process to get the latest version of the Session Manager plugin\.

**Note**  
For best results, we recommend that you start sessions on Windows clients using Windows PowerShell, version 5 or later\. Alternatively, you can use the Command shell in Microsoft Windows 10\. The Session Manager plugin only supports PowerShell and the Command shell\. Third\-party command line tools might not be compatible with the plugin\.

**To install the Session Manager plugin using the EXE installer**

1. Download the installer using the following URL\.

   ```
   https://s3.amazonaws.com/session-manager-downloads/plugin/latest/windows/SessionManagerPluginSetup.exe
   ```

   Alternatively, you can download a zipped version of the installer using the following URL\.

   ```
   https://s3.amazonaws.com/session-manager-downloads/plugin/latest/windows/SessionManagerPlugin.zip
   ```

1. Run the downloaded installer, and follow the on\-screen instructions\. If you downloaded the zipped version of the installer, you must unzip the installer first\.

   Leave the install location box blank to install the plugin to the default directory\.
   +  `C:\%PROGRAMFILES%\Amazon\SessionManagerPlugin\bin\` 

1. Verify that the installation was successful\. For information, see [Verify the Session Manager plugin installation](#install-plugin-verify)\.
**Note**  
If Windows is unable to find the executable, you might need to re\-open the command prompt or add the installation directory to your `PATH` environment variable manually\. For information, see the troubleshooting topic [Session Manager plugin not automatically added to command line path \(Windows\)](session-manager-troubleshooting.md#windows-plugin-env-var-not-set)\.

## Install and uninstall the Session Manager plugin on macOS<a name="install-plugin-macos"></a>

You can install the Session Manager plugin on macOS using the bundled installer\.

**Important**  
The bundled installer doesn't support installing to paths that contain spaces\.

**To install the Session Manager plugin using the bundled installer \(macOS\)**

1. Download the bundled installer\.

   ```
   curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac/sessionmanager-bundle.zip" -o "sessionmanager-bundle.zip"
   ```

1. Unzip the package\.

   ```
   unzip sessionmanager-bundle.zip
   ```

1. Run the install command\.

   ```
   sudo ./sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin
   ```
**Note**  
 The plugin requires either Python 2\.6\.5 or later, or Python 3\.3 or later\. By default, the install script runs under the system default version of Python\. If you have installed an alternative version of Python and want to use that to install the Session Manager plugin, run the install script with that version by absolute path to the Python executable\. The following is an example\.  

   ```
   sudo /usr/local/bin/python3.6 sessionmanager-bundle/install -i /usr/local/sessionmanagerplugin -b /usr/local/bin/session-manager-plugin
   ```

   The installer installs the Session Manager plugin at `/usr/local/sessionmanagerplugin` and creates the symlink `session-manager-plugin` in the `/usr/local/bin` directory\. This eliminates the need to specify the install directory in the user's `$PATH` variable\.

   To see an explanation of the \-i and \-b options, use the \-h option\.

   ```
   ./sessionmanager-bundle/install -h
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run the following two commands in the order shown\.  

```
sudo rm -rf /usr/local/sessionmanagerplugin
```

```
sudo rm /usr/local/bin/session-manager-plugin
```

## Install the Session Manager plugin on macOS with the signed installer<a name="install-plugin-macos-signed"></a>

You can install the Session Manager plugin on macOS using the signed installer\.

**To install the Session Manager plugin using the signed installer \(macOS\)**

1. Download the signed installer\.

   ```
   curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac/session-manager-plugin.pkg" -o "session-manager-plugin.pkg"
   ```

1. Run the install command\.

   ```
   sudo installer -pkg session-manager-plugin.pkg -target /
   sudo ln -s /usr/local/sessionmanagerplugin/bin/session-manager-plugin /usr/local/bin/session-manager-plugin
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager plugin installation](#install-plugin-verify)\.

## Install Session Manager plugin on Linux<a name="install-plugin-linux"></a>

1. Download the Session Manager plugin RPM package\.
   + Intel 64\-bit \(x86\_64\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_64bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```
   + Intel 32\-bit \(x86\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_32bit/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```
   + ARM 64\-bit \(arm64\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/linux_arm64/session-manager-plugin.rpm" -o "session-manager-plugin.rpm"
     ```

1. Run the install command\.

   ```
   sudo yum install -y session-manager-plugin.rpm
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run `sudo yum erase session-manager-plugin -y`

## Install the Session Manager plugin on Ubuntu Server<a name="install-plugin-debian"></a>

1. Download the Session Manager plugin deb package\.
   + Intel 64\-bit \(x86\_64\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```
   + Intel 32\-bit \(x86\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_32bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```
   + ARM 64\-bit \(arm64\)

     ```
     curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_arm64/session-manager-plugin.deb" -o "session-manager-plugin.deb"
     ```

1. Run the install command\.

   ```
   sudo dpkg -i session-manager-plugin.deb
   ```

1. Verify that the installation was successful\. For information, see [Verify the Session Manager plugin installation](#install-plugin-verify)\.

**Note**  
If you ever want to uninstall the plugin, run `sudo dpkg -r session-manager-plugin`

## Verify the Session Manager plugin installation<a name="install-plugin-verify"></a>

Run the following commands to verify that the Session Manager plugin installed successfully\.

```
session-manager-plugin
```

If the installation was successful, the following message is returned\.

```
The Session Manager plugin is installed successfully. Use the AWS CLI to start a session.
```

You can also test the installation by running the following command in the AWS CLI\.

**Note**  
This command will work only if your Session Manager administrator has granted you the necessary IAM permissions to access the target instance using Session Manager\.

```
aws ssm start-session --target id-of-an-instance-you-have-permissions-to-access
```

## Session Manager plugin on GitHub<a name="plugin-github"></a>

The source code for Session Manager plugin is available on [GitHub](https://github.com/aws/session-manager-plugin) so that you can adapt the plugin to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/session-manager-plugin/blob/mainline/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services doesn't provide support for running modified copies of this software\.

## \(Optional\) Turn on Session Manager plugin logging<a name="install-plugin-configure-logs"></a>

The Session Manager plugin includes an option to allow logging for sessions that you run\. By default, logging is turned off\.

If you allow logging, the Session Manager plugin creates log files for both application activity \(`session-manager-plugin.log`\) and errors \(`errors.log`\) on your local machine\.

**Topics**
+ [Turn on logging for the Session Manager plugin \(Windows\)](#configure-logs-windows)
+ [Enable logging for the Session Manager plugin \(Linux and macOS\)](#configure-logs-linux)

### Turn on logging for the Session Manager plugin \(Windows\)<a name="configure-logs-windows"></a>

1. Locate the `seelog.xml.template` file for the plugin\. 

   The default location is `C:\Program Files\Amazon\SessionManagerPlugin\seelog.xml.template`\.

1. Change the name of the file to `seelog.xml`\.

1. Open the file and change `minlevel="off"` to `minlevel="info"` or `minlevel="debug"`\.
**Note**  
By default, log entries about opening a data channel and reconnecting sessions are recorded at the **INFO** level\. Data flow \(packets and acknowledgement\) entries are recorded at the **DEBUG** level\.

1. Change other configuration options you want to modify\. Options you can change include: 
   + **Debug level**: You can change the debug level from `formatid="fmtinfo"` to `outputs formatid="fmtdebug"`\.
   + **Log file options**: You can make changes to the log file options, including where the logs are stored, with the exception of the log file names\. 
**Important**  
Don't change the file names or logging won't work correctly\.

     ```
     <rollingfile type="size" filename="C:\Program Files\Amazon\SessionManagerPlugin\Logs\session-manager-plugin.log" maxsize="30000000" maxrolls="5"/>
     <filter levels="error,critical" formatid="fmterror">
     <rollingfile type="size" filename="C:\Program Files\Amazon\SessionManagerPlugin\Logs\errors.log" maxsize="10000000" maxrolls="5"/>
     ```

1. Save the file\.

### Enable logging for the Session Manager plugin \(Linux and macOS\)<a name="configure-logs-linux"></a>

1. Locate the `seelog.xml.template` file for the plugin\. 

   The default location is `/usr/local/sessionmanagerplugin/seelog.xml.template`\.

1. Change the name of the file to `seelog.xml`\.

1. Open the file and change `minlevel="off"` to `minlevel="info"` or `minlevel="debug"`\.
**Note**  
By default, log entries about opening data channels and reconnecting sessions are recorded at the **INFO** level\. Data flow \(packets and acknowledgement\) entries are recorded at the **DEBUG** level\.

1. Change other configuration options you want to modify\. Options you can change include: 
   + **Debug level**: You can change the debug level from `formatid="fmtinfo"` to `outputs formatid="fmtdebug"`
   + **Log file options**: You can make changes to the log file options, including where the logs are stored, with the exception of the log file names\. 
**Important**  
Don't change the file names or logging won't work correctly\.

     ```
     <rollingfile type="size" filename="/usr/local/sessionmanagerplugin/logs/session-manager-plugin.log" maxsize="30000000" maxrolls="5"/>
     <filter levels="error,critical" formatid="fmterror">
     <rollingfile type="size" filename="/usr/local/sessionmanagerplugin/logs/errors.log" maxsize="10000000" maxrolls="5"/>
     ```
**Important**  
If you use the specified default directory for storing logs, you must either run session commands using sudo or give the directory where the plugin is installed full read and write permissions\. To bypass these restrictions, change the location where logs are stored\.

1. Save the file\.

## Session Manager plugin latest version and release history<a name="plugin-version-history"></a>

Your local machine must be running a supported version of the Session Manager plugin\. The current minimum supported version is 1\.1\.17\.0\. If you're running an earlier version, your Session Manager operations might not succeed\. 

 

To see if you have the latest version, run the following command in the AWS CLI\.

**Note**  
The command returns results only if the plugin is located in the default installation directory for your operating system type\. You can also check the version in the contents of the `VERSION` file in the directory where you have installed the plugin\.

```
session-manager-plugin --version
```

The following table lists all releases of the Session Manager plugin and the features and enhancements included with each version\.


| Version | Release date | Details | 
| --- | --- | --- | 
| 1\.2\.279\.0 |  October 27, 2021  | Enhancement: Zip packaging for Windows platform\. | 
| 1\.2\.245\.0 |  August 19, 2021  | Enhancement: Upgrade aws\-sdk\-go to latest version \(v1\.40\.17\) to support single sign\-on \(SSO\)\. | 
| 1\.2\.234\.0 |  July 26, 2021  | Bug fix: Handle session abruptly terminated scenario in interactive session type\. | 
| 1\.2\.205\.0 |  June 10, 2021  | Enhancement: Added support for signed macOS installer\. | 
| 1\.2\.54\.0 |  January 29, 2021  | Enhancement: Added support for running sessions in NonInteractiveCommands execution mode\. | 
| 1\.2\.30\.0 |  November 24, 2020  |  **Enhancement**: \(Port forwarding sessions only\) Improved overall performance\.  | 
| 1\.2\.7\.0 |  October 15, 2020  |  **Enhancement**: \(Port forwarding sessions only\) Reduced latency and improved overall performance\.  | 
| 1\.1\.61\.0 |  April 17, 2020  |  **Enhancement**: Added ARM support for Linux and Ubuntu Server\.   | 
| 1\.1\.54\.0 |  January 6, 2020  |  **Bug fix**: Handle race condition scenario of packets being dropped when the Session Manager plugin isn't ready\.   | 
|  1\.1\.50\.0  | November 19, 2019 |  **Enhancement**: Added support for forwarding a port to a local unix socket\.  | 
|  1\.1\.35\.0  | November 7, 2019 |  **Enhancement**: \(Port forwarding sessions only\) Send a TerminateSession command to SSM Agent when the local user presses Ctrl\+C\.  | 
| 1\.1\.33\.0 | September 26, 2019 | Enhancement: \(Port forwarding sessions only\) Send a disconnect signal to the server when the client drops the TCP connection\.  | 
| 1\.1\.31\.0 | September 6, 2019 | Enhancement: Update to keep port forwarding session open until remote server closes the connection\. | 
|  1\.1\.26\.0  | July 30, 2019 |  **Enhancement**: Update to limit the rate of data transfer during a session\.  | 
|  1\.1\.23\.0  | July 9, 2019 |  **Enhancement**: Added support for running SSH sessions using Session Manager\.  | 
| 1\.1\.17\.0 | April 4, 2019 |  **Enhancement**: Added support for further encryption of session data using AWS Key Management Service \(AWS KMS\)\.  | 
| 1\.0\.37\.0 | September 20, 2018 |  **Enhancement**: Bug fix for Windows version\.  | 
| 1\.0\.0\.0 | September 11, 2018 |  Initial release of the Session Manager plugin\.  | 