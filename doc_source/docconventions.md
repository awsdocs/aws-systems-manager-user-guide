# Document conventions<a name="docconventions"></a>

The following are the common typographical conventions for the *AWS Systems Manager User Guide*\. For conventions common to all AWS documentation, see [Document conventions](https://docs.aws.amazon.com/general/latest/gr/docconventions.html) in the *Amazon Web Services General Reference*\.

**Differentiated examples for local operating systems or command line languages**  
Formatting: Tabs to present commands based on a user's local operating system type\. The backslash \(`\` \) character is used to break long commands into multiple lines on Linux and macOS machines\. On Windows Server, line breaks are made using the caret \(`^`\) character\.  
Example:  

```
aws ssm update-service-setting \
    --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier \
    --setting-value advanced
```

```
aws ssm update-service-setting ^
    --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier ^
    --setting-value advanced
```