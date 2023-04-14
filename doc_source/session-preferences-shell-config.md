# Allow configurable shell profiles<a name="session-preferences-shell-config"></a>

By default, sessions on EC2 instances for Linux start using the Bourne shell \(sh\)\. However, you might prefer to use another shell like bash\. By allowing configurable shell profiles, you can customize preferences within sessions such as shell preferences, environment variables, working directories, and running multiple commands when a session is started\.

**Important**  
Systems Manager doesn't check the commands or scripts in your shell profile to see what changes they would make to an instance before they're run\. To restrict a user’s ability to modify commands or scripts entered in their shell profile, we recommend the following:  
Create a customized Session\-type document for your AWS Identity and Access Management \(IAM\) users and roles\. Then modify the IAM policy for these users and roles so the `StartSession` API operation can only use the Session\-type document you have created for them\. For information see, [Create a Session Manager preferences document \(command line\)](getting-started-create-preferences-cli.md) and [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.
Modify the IAM policy for your IAM users and roles to deny access to the `UpdateDocument` API operation for the Session\-type document resource you create\. This allows your users and roles to use the document you created for their session preferences without allowing them to modify any of the settings\.

**To turn on configurable shell profiles**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Specify the environment variables, shell preferences, or commands you want to run when your session starts in the fields for the applicable operating systems\.

1. Choose **Save**\.

The following are some example commands that can be added to your shell profile\.

Change to the bash shell and change to the /usr directory on Linux instances\.

```
exec /bin/bash
cd /usr
```

Output a timestamp and welcome message at the start of a session\.

------
#### [ Linux & macOS ]

```
timestamp=$(date '+%Y-%m-%dT%H:%M:%SZ')
user=$(whoami)
echo $timestamp && echo "Welcome $user"'!'
echo "You have logged in to a production instance. Note that all session activity is being logged."
```

------
#### [ Windows ]

```
$timestamp = (Get-Date).ToString("yyyy-MM-ddTH:mm:ssZ")
$splitName = (whoami).Split("\")
$user = $splitName[1]
Write-Host $timestamp
Write-Host "Welcome $user!"
Write-Host "You have logged in to a production instance. Note that all session activity is being logged."
```

------

View dynamic system activity at the start of a session\.

------
#### [ Linux & macOS ]

```
top
```

------
#### [ Windows ]

```
while ($true) { Get-Process | Sort-Object -Descending CPU | Select-Object -First 30; `
Start-Sleep -Seconds 2; cls
Write-Host "Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName"; 
Write-Host "-------  ------    -----      ----- -----   ------     -- -----------"}
```

------