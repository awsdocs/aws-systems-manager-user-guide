# Step 6: \(Optional\) Disable or Enable ssm\-user Account Administrative Permissions<a name="session-manager-getting-started-ssm-user-permissions"></a>

When a version of SSM Agent that supports Session Manager starts on an instance, it creates a user account with root or administrator privileges called *ssm\-user*\. On Linux machines, the account is added to `/etc/sudoers`\. On Windows machines, it is added to the Administrators group\. Sessions are launched using the credentials of this user account\.

If you want to prevent Session Manager users from running administrative commands on an instance, you can update its *ssm\-user* permissions\. You can also restore these permissions after they have been removed\.

**Topics**
+ [Managing ssm\-user sudo Account Permissions on Linux](#ssm-user-permissions-linux)
+ [Managing ssm\-user Administrator Account Permissions on Windows Server](#ssm-user-permissions-windows)

## Managing ssm\-user sudo Account Permissions on Linux<a name="ssm-user-permissions-linux"></a>

Use one of the following procedures to disable or enable the ssm\-user account sudo permissions on Linux instances:

**Use Run Command to modify ssm\-user sudo permissions \(console\)**
+ Use the procedure in [Running Commands from the Console](rc-console.md) with the following values:
  + For **Command document**, choose `AWS-RunShellScript`\.
  + To remove sudo access, in the **Command parameters** area, paste the following in the **Commands** box:

    ```
    cd /etc/sudoers.d
    echo "#User rules for ssm-user" > ssm-agent-users
    ```

    \-or\-

    To restore sudo access, in the **Command parameters** area, paste the following in the **Commands** box:

    ```
    cd /etc/sudoers.d 
    echo "ssm-user ALL=(ALL) NOPASSWD:ALL" > ssm-agent-users
    ```

**Use the command line to modify ssm\-user sudo permissions \(AWS CLI\)**

1. Connect to the instance and run the following command:

   ```
   sudo -s
   ```

1. Change the working directory using the following command:

   ```
   cd /etc/sudoers.d
   ```

1. Open the file named `ssm-agent-users` for editing\.

1. To remove sudo access, delete the following line:

   ```
   ssm-user ALL=(ALL) NOPASSWD:ALL
   ```

   \-or\-

   To restore sudo access, add the following line:

   ```
   ssm-user ALL=(ALL) NOPASSWD:ALL
   ```

1. Save the file\.

## Managing ssm\-user Administrator Account Permissions on Windows Server<a name="ssm-user-permissions-windows"></a>

Use one of the following procedures to disable or enable the ssm\-user account Administrator permissions on Windows Server instances:

**Use Run Command to modify Administrator permissions \(console\)**
+ Use the procedure in [Running Commands from the Console](rc-console.md) with the following values:

  For **Command document**, choose `AWS-RunPowerShellScript`\.

  To remove administrative access, in the **Command parameters** area, paste the following in the **Commands** box:

  ```
  net localgroup "Administrators" "ssm-user" /delete
  ```

  \-or\-

  To restore administrative access, in the **Command parameters** area, paste the following in the **Commands** box:

  ```
  net localgroup "Administrators" "ssm-user" /add
  ```

**Use the PowerShell or Command Prompt window to modify Administrator permissions**

1. Connect to the instance and open the PowerShell or Command Prompt window\.

1. To remove administrative access, run the following command:

   ```
   net localgroup "Administrators" "ssm-user" /delete
   ```

   \-or\-

   To restore administrative access, run the following command:

   ```
   net localgroup "Administrators" "ssm-user" /add
   ```

**Use the Windows console to modify Administrator permissions**

1. Connect to the instance and open the PowerShell or Command Prompt window\.

1. From the command line, run `lusrmgr.msc` to open the **Local Users and Groups** console\.

1. Open the **Users** directory, and then open **ssm\-user**\.

1. On the **Member Of** tab, do one of the following:
   + To remove administrative access, select **Administrators**, and then choose **Remove**\.

     \-or\-

     To restore administrative access, type `Administrators` in the text box, and then choose **Add**\.

1. Choose **OK**\.