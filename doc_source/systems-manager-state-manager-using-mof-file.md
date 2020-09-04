# Creating associations that run MOF files<a name="systems-manager-state-manager-using-mof-file"></a>

You can run Managed Object Format \(MOF\) files to enforce a desired state on Windows Server managed instances with State Manager by using the AWS\-ApplyDSCMofs SSM document\. The AWS\-ApplyDSCMofs document has two execution modes\. With the first mode, you can configure the association to scan and report if the managed instances are currently in the desired state defined in the specified MOF files\. In the second mode, you can run the MOF files and change the configuration of your instances based on the resources and their values defined in the MOF files\. The AWS\-ApplyDSCMofs document enables you to download and run MOF configuration files from Amazon Simple Storage Service \(Amazon S3\), a local share, or from a secure web site with an HTTPS domain\.

State Manager logs and reports the status of each MOF file execution during each association run\. State Manager also reports the output of each MOF file execution as a compliance event which you can view on the [AWS Systems Manager Compliance](https://console.aws.amazon.com/systems-manager/compliance) page\.

MOF file execution is built on Windows PowerShell Desired State Configuration \(PowerShell DSC\)\. PowerShell DSC is a declarative platform used for configuration, deployment, and management of Windows systems\. PowerShell DSC allows administrators to describe, in simple text documents called DSC configurations, how they want a server to be configured\. A PowerShell DSC configuration is a specialized PowerShell script that states what to do, but not how to do it\. Running the configuration produces a MOF file\. The MOF file can be applied to one or more servers to achieve the desired configuration for those servers\. PowerShell DSC resources do the actual work of enforcing configuration\. For more information, see [Windows PowerShell Desired State Configuration Overview](https://docs.microsoft.com/en-us/powershell/scripting/dsc/overview/overview?view=powershell-6)\.

**Topics**
+ [Using Amazon S3 to store artifacts](#systems-manager-state-manager-using-mof-file-S3-storage)
+ [Resolving credentials in MOF files](#systems-manager-state-manager-using-mof-file-credentials)
+ [Using tokens in MOF files](#systems-manager-state-manager-using-mof-file-tokens)
+ [Prerequisites](#systems-manager-state-manager-using-mof-file-prereqs)
+ [Creating an association that runs MOF files](#systems-manager-state-manager-using-mof-file-creating)
+ [Troubleshooting](#systems-manager-state-manager-using-mof-file-troubleshooting)
+ [Viewing DSC resource compliance details](#systems-manager-state-manager-viewing-mof-file-compliance)

## Using Amazon S3 to store artifacts<a name="systems-manager-state-manager-using-mof-file-S3-storage"></a>

If you are using Amazon S3 to store PowerShell modules, MOF files, compliance reports, or status reports, then the IAM role used by SSM Agent must have `GetObject` and `ListBucket` permissions on the bucket\. If you don't provide these permissions, the system returns an *Access Denied* error\. Also note the following important information about storing artifacts in Amazon S3\.
+ If the bucket is in a different AWS account, then you must create a bucket resource policy that grants the account \(or the IAM role\) `GetObject` and `ListBucket` permissions\.
+ If you want to use custom DSC resources, you can download these resources from an Amazon S3 bucket\. You can also install them automatically from the PowerShell gallery\. 
+ If you are using Amazon S3 as a module source, then you need to upload the module as a Zip file in the following case\-sensitive format: *ModuleName*\_*ModuleVersion*\.zip\. For example: MyModule\_1\.0\.0\.zip\.
+ All files must be in the bucket root\. Folder structures are not supported\.

## Resolving credentials in MOF files<a name="systems-manager-state-manager-using-mof-file-credentials"></a>

Credentials are resolved by using [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) or [AWS Systems Manager Parameter Store](systems-manager-parameter-store.md)\. This allows you to set up automatic credential rotation\. This also enables DSC to automatically propagate credentials to your servers without redeploying MOFs\.

To use a Secrets Manager secret in a configuration, create a PSCredential object where the Username is the SecretId or SecretARN of the secret containing the credential\. You can specify any value for the password\. The value is ignored\. Here is an example:

```
Configuration MyConfig
{
   $ss = ConvertTo-SecureString -String 'a_string' -AsPlaintext -Force
   $credential = New-Object PSCredential('a_secret_or_ARN', $ss)

    Node localhost
    {
       File file_name
       {
           DestinationPath = 'C:\MyFile.txt'
           SourcePath = '\\FileServer\Share\MyFile.txt'
           Credential = $credential
       }
    }
}
```

You must then compile your MOF using the PsAllowPlaintextPassword setting in configuration data\. This is OK because the credential only contains a label\. 

In Secrets Manager, ensure that the instance has GetSecretValue access in an IAM Managed Policy, and optionally in the Secret Resource Policy if one exists\. In order to work with DSC, the secret must be in the following format:

```
{ 'Username': 'a_name', 'Password': 'a_password' }
```

The secret can have other properties \(for example, properties used for rotation\), but it must at least have the username and password properties\.

We recommended that you use a multi\-user rotation method, where you have two different usernames and passwords, and the rotation AWS Lambda function flips between them\. This method allows you to have multiple active accounts while eliminating the risk of locking out a user during rotation\.

## Using tokens in MOF files<a name="systems-manager-state-manager-using-mof-file-tokens"></a>

Tokens give you the ability to modify resource property values *after* the MOF has been compiled\. This enables you to reuse common MOF files on multiple servers that require very similar configurations\.

Token substitution only works for Resource Properties of type `String`\. However, if your resource has a nested CIM instance property, it will also resolve tokens from `String` properties in that CIM instance\. You can't use token substitution for numerals or arrays\.

For example, consider a scenario where you're using the xComputerManagement resource and you want to rename the computer using DSC\. Normally you would need a dedicated MOF file for that machine\. However, with token support, you can create a single MOF file and apply it to all of your instances\. In the `ComputerName` property, instead of hard coding the computer name into the MOF, you can use an Instance Tag type token\. The value is resolved during MOF parsing\. For example:

```
Configuration MyConfig
{
    xComputer Computer
    {
        ComputerName = '{tag:ComputerName}'
    }
}
```

You then set a tag on either the managed instance in the AWS Systems Manager console, or an Amazon EC2 tag in the EC2 console\. When you run the document, the script substitutes the \{tag:ComputerName\} token for the value of the instance tag\.

You can also combine multiple tags into a single property, for example:

```
Configuration MyConfig
{
    File MyFile
    {
        DestinationPath = '{env:TMP}\{tag:ComputerName}'
        Type = 'Directory'
    }
}
```

There are 5 different types of tokens you can use:
+ **tag**: Amazon EC2 or managed instance tags
+ **tagb64**: This is the same as tag, but the system use base64 to decode the value\. This allows you to use special characters in tag values\.
+ **env**: Resolves Environment variables\.
+ **ssm**: Systems Manager Parameter Store values\. Only String and Secure String types are supported\.
+ **tagssm**: This is the same as tag, but if the tag is not set on the instance, the system tries to resolve the value from an SSM Parameter with the same name\. This is useful in situations when you want a 'default global value' but you want to be able to override it on a single instance \(for example, one\-box deployments\)\.

Here is Parameter Store example that uses the `ssm` token type\. 

```
File MyFile
{
    DestinationPath = "C:\ProgramData\ConnectionData.txt"
    Content = "{ssm:%servicePath%/ConnectionData}"
}
```

Tokens play an important role in reducing redundant code by making MOF files generic and reusable\. If you can avoid server\-specific MOF file, then there’s no need for a MOF building service\. A MOF building service increases costs, slows provisioning time, and increases the risk of configuration drift between grouped instances due to differing module versions being installed on the build server when their MOFs were compiled\.

## Prerequisites<a name="systems-manager-state-manager-using-mof-file-prereqs"></a>

Before you create an association that runs MOF files, verify that your managed instances have the following prerequisites installed:
+ Windows PowerShell version 5\.0 or later\. For more information, see [Windows PowerShell System Requirements](https://docs.microsoft.com/en-us/powershell/scripting/install/windows-powershell-system-requirements?view=powershell-6) on Microsoft\.com\.
+ [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/) version 3\.3\.261\.0 or later\.
+ SSM Agent version 2\.2 or later\.

## Creating an association that runs MOF files<a name="systems-manager-state-manager-using-mof-file-creating"></a>

**To create an association that runs MOF files**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\. A name can help you understand the purpose of the association when you created it\. Spaces aren't allowed in the name\.

1. In the **Document** list, choose **AWS\-ApplyDSCMofs**\.

1. In the **Parameters** section, specify your choices for the required and optional input parameters\.

   1. **Mofs To Apply**: Specify one or more MOF files to run when this association runs\. Use commas to separate a list of MOF files\. You can specify the following options for locating MOF file\.
      + An Amazon S3 bucket name\. Bucket names must use lowercase letters\. Specify this information by using the following format:

        ```
        s3:doc-example-bucket:MOF_file_name.mof
        ```

        If you want to specify an AWS Region, then use the following format:

        ```
        s3:bucket_Region:doc-example-bucket:MOF_file_name.mof
        ```
      + A secure web site\. Specify this information by using the following format:

        ```
        https://domain_name/MOF_file_name.mof
        ```

        Here is an example:

        ```
        https://www.example.com/TestMOF.mof
        ```
      + A file system on a local share\. Specify this information by using the following format:

        ```
        \server_name\shared_folder_name\MOF_file_name.mof
        ```

        Here is an example:

        ```
        \StateManagerAssociationsBox\MOFs_folder\MyMof.mof
        ```

   1. **Service Path**: \(Optional\) A service path is either an Amazon S3 bucket prefix where you want to write reports and status information\. Or, a service path is a path for Parameter Store parameter\-based tags\. When resolving parameter\-based tags, the system uses \{ssm:%servicePath%/*parameter\_name*\} to inject the servicePath value into the parameter name\. For example, if your service path is "WebServers/Production" then the systems resolves the parameter as: WebServers/Production/*parameter\_name*\. This is useful for when you are running multiple environments in the same account\.

   1. **Report Bucket Name**: \(Optional\) Enter the name of an Amazon S3 bucket where you want to write compliance data\. Reports are saved in this bucket in JSON format\.
**Note**  
You can prefix the bucket name with a Region where the bucket is located\. Here's an example: us\-west\-2:MyMOFBucket\. If you are using a proxy for Amazon S3 endpoints in a specific region that does not include us\-east\-1, then you must prefix the bucket name with a region\. If the bucket name is not prefixed, it will automatically discover the bucket region using the us\-east\-1 endpoint\.

   1. **Mof Operation Mode**: Choose State Manager behavior when running the **AWS\-ApplyDSCMofs** association:
      + **Apply**: Correct instance configurations that aren't compliant\. 
      + **ReportOnly**: Don't correct instance configurations, but instead log all compliance data and report instances that aren't compliant\.

   1. **Status Bucket Name**: \(Optional\) Enter the name of an Amazon S3 bucket where you want to write MOF execution status information\. These status reports are singleton summaries of the most recent compliance run of an instance\. This means that the report is overwritten the next time the association runs MOF files\.
**Note**  
You can prefix the bucket name with a Region where the bucket is located\. Here's an example: `us-west-2:doc-example-bucket`\. If you are using a proxy for Amazon S3 endpoints in a specific region that does not include us\-east\-1, then you must prefix the bucket name with a region\. If the bucket name is not prefixed, it will automatically discover the bucket region using the us\-east\-1 endpoint\.

   1. **Module Source Bucket Name**: \(Optional\) Enter the name of an Amazon S3 bucket that contains PowerShell module files\. If you specify **None**, then you must choose **True** for the next option, **Allow PS Gallery Module Source**\.
**Note**  
You can prefix the bucket name with a Region where the bucket is located\. Here's an example: `us-west-2:doc-example-bucket`\. If you are using a proxy for Amazon S3 endpoints in a specific region that does not include us\-east\-1, then you must prefix the bucket name with a region\. If the bucket name is not prefixed, it will automatically discover the bucket region using the us\-east\-1 endpoint\.

   1. **Allow PS Gallery Module Source**: \(Optional\) Choose **True** to download PowerShell modules from [https://www\.powershellgallery\.com/](https://www.powershellgallery.com/)\. If you choose **False**, then you must specify a source for the previous option, **ModuleSourceBucketName**\.

   1. **Proxy Uri**: \(Optional\) Use this option to download MOF files from a proxy server\.

   1. **Reboot Behavior**: \(Optional\) Specify one of the following reboot behaviors if your MOF file execution requires rebooting:
      + **AfterMof**: Reboots the instance after all MOF executions are complete\. Even if multiple MOF executions request reboots, the system waits until all MOF executions are complete to reboot\.
      + **Immediately**: Reboots the instance whenever a MOF execution requests it\. If running multiple MOF files that request reboots, then the instance will be rebooted multiple times\.
      + **Never**: Instances are not rebooted, even if the MOF execution explicitly requests a reboot\.

   1. **Use Computer Name For Reporting**: \(Optional\) Enable this option to use the name of the computer when reporting compliance information\. The default value is **false**, which means that the system uses the instance ID when reporting compliance information\.

   1. **Enable Verbose Logging**: \(Optional\) We recommend that you enable verbose logging when deploying MOF files for the first time\.
**Important**  
When enabled, verbose logging writes more data to your Amazon S3 bucket than standard association execution logging\. This can result in slower performance and possibly higher storage charges for Amazon S3\. To mitigate storage size issues, we recommend that you enable lifecycle policies on your Amazon S3 bucket\. For more information, see [How Do I Create a Lifecycle Policy for an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-lifecycle.html) in the *Amazon Simple Storage Service Console User Guide*\.

   1. **Enable Debug Logging**: \(Optional\) We recommend that you enable debug logging if you need to troubleshoot MOF failures\. We also recommend that you disable this option for normal use\.
**Important**  
When enabled, debug logging writes more data to your Amazon S3 bucket than standard association execution logging\. This can result in slower performance and possibly higher storage charges for Amazon S3\. To mitigate storage size issues, we recommend that you enable lifecycle policies on your Amazon S3 bucket\. For more information, see [How Do I Create a Lifecycle Policy for an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-lifecycle.html) in the *Amazon Simple Storage Service Console User Guide*\.

   1. **Compliance Type**: \(Optional\) Specify the compliance type to use when reporting compliance information\. The default compliance type is **Custom:DSC**\. If you create multiple associations that run MOF files, then be sure to specify a different compliance type for each association\. If you don't, each additional association that uses **Custom:DSC** will overwrite the existing compliance data\.

   1. **Pre Reboot Script**: \(Optional\) Specify a script to run if the configuration has indicated that a reboot is necessary\. The script runs before the reboot\. The script must be a single line\. If you need to add additional lines, separate lines by using semicolons\.

1. In the **Targets** section, choose either **Specifying tags** or **Manually Selecting Instance**\. If you choose to target resources by using tags, then enter a tag key and a tag value in the fields provided\. For more information about using targets, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, then use the buttons provided to create a cron or rate schedule for the association\. 

1. In the **Advanced options** section:
   + In **Compliance severity**, choose a severity level for the association\. Compliance reporting will indicate whether the association state is compliant or non\-compliant, along with the severity level you indicate here\. For more information, see [About State Manager association compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, configure options for running State Manager associations across of fleet of managed instances\. For more information about these options, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors allowed before State Manager stops running associations on additional targets\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. Choose **Create Association**\. 

State Manager creates and immediately runs the association on the specified instances or targets\. After the initial execution, the association runs in intervals according to the schedule that you defined and according to the following rules:
+ Associations are only run on instances that are online when the interval starts\. Offline instances are skipped\.
+ State Manager attempts to run the association on all configured instances during an interval\.
+ If an association is not run during an interval \(because, for example, a concurrency value limited the number of instances that could process the association at one time\), then State Manager attempts to run the association during the next interval\.
+ State Manager records history for all skipped intervals\. You can view the history on the **Execution History** tab\.

**Note**  
The AWS\-ApplyDSCMofs is a Systems Manager command document\. This means that you can also run this document by using Run Command\. For more information, see [Running commands using Systems Manager Run Command](run-command.md)\.

## Troubleshooting<a name="systems-manager-state-manager-using-mof-file-troubleshooting"></a>

This section includes information to help you troubleshoot issues creating associations that run MOF files\.

**Enable enhanced logging**  
As a first step to troubleshooting, enable enhanced logging\. More specifically, do the following:
+ Verify that the association is configured to write command output to either Amazon S3 or Amazon CloudWatch Logs\.
+ Set the **Enable Verbose Logging** parameter to True\.
+ Set the **Enable Debug Logging** parameter to True\.

With verbose and debug logging enabled, the **Stdout** output file includes details about the script execution\. This output file can help you identify where the script failed\. The **Stderr** output file contains errors that occurred during the script execution\. 

### Common problems<a name="systems-manager-state-manager-using-mof-file-troubleshooting-common"></a>

This section includes information about common problems that can occur when creating associations that run MOF files and steps to troubleshoot these issues\.

**My MOF was not applied**  
If State Manager failed to apply the association to your instances, then start by reviewing the **Stderr** output file\. This file can help you understand the root cause of the issue\. Also, verify the following:
+ The instance has the required access permissions to all MOF\-related Amazon S3 buckets\. Specifically:
  + **s3:GetObject permissions**: This is required for MOF files in private Amazon S3 buckets as well as custom modules in Amazon S3 buckets\.
  + **s3:PutObject permission**: This is required to write compliance reports and compliance status to Amazon S3 buckets\.
+ If you are using tags, then ensure that the instance has the required IAM policy\. Using tags requires the instance IAM role to have a policy allowing the `ec2:DescribeInstances` and `ssm:ListTagsForResource` actions\.
+ Ensure that the instance has the expected tags or SSM parameters assigned\.
+ Ensure that the tags or SSM parameters aren't misspelled\.
+ Try applying the MOF locally on the instance to make sure there isn't an issue with the MOF file itself\.

**My MOF seemed to fail, but the Systems Manager execution was successful**  
If the AWS\-ApplyDSCMofs document successfully ran, then the Systems Manager execution status shows **Success**\. This status does not reflect the compliance status of your instance against the configuration requirements in the MOF file\. To view the compliance status of your instances, view the compliance reports\. You can view a JSON report in the Amazon S3 Report Bucket\. This applies to Run Command and State Manager executions\. Also, for State Manager, you can view compliance details on the Systems Manager Compliance page\.

**Stderr states: Name resolution failure attempting to reach service**  
This error indicates that the script can't reach a remote service\. Most likely, the script can't reach Amazon S3\. This issue most often occurs when the script attempts to write compliance reports or compliance status to the Amazon S3 bucket supplied in the document parameters\. Typically, this error occurs when a computing environment uses a firewall or transparent proxy that includes an allow list\. To resolve this issue:
+ Use region\-specific bucket syntax for all Amazon S3 bucket parameters\. For example, the **Mofs to Apply** parameter should be formatted as follows:

  s3:*bucket\-region*:*bucket\-name*:*mof\-file\-name*\.mof\.

  Here is an example:` s3:us-west-2:doc-example-bucket:my-mof.mof`

  The Report, Status, and Module Source bucket names should be formatted as follows:

  *bucket\-region*:*bucket\-name*\. Here is an example: `us-west-1:doc-example-bucket`
+ If Region\-specific syntax does not fix the problem, then make sure that the targeted instance\(s\) can access Amazon S3 in the desired region\. To verify this:

  1. Find the endpoint name for Amazon S3 in the appropriate Amazon S3 Region\. For information, see [Amazon S3 Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_region) in the *Amazon Web Services General Reference*\.

  1. Log on to the target instance and run the following ping command:

     ```
     ping s3.s3-region.amazonaws.com
     ```

     If the ping failed, it means that either Amazon S3 is down, or a firewall/transparent proxy is blocking access to the Amazon S3 region, or the instance can't access the internet\.

## Viewing DSC resource compliance details<a name="systems-manager-state-manager-viewing-mof-file-compliance"></a>

Systems Manager captures compliance information about DSC resource failures in the Amazon Simple Storage Service \(Amazon S3\) **Status Bucket** you specified when you ran the AWS\-ApplyDSCMofs document\. Searching for information about DSC resource failures in an Amazon S3 bucket can be time consuming\. Instead, you can quickly view this information in the Systems Manager **Compliance** page\. 

The **Compliance resources summary** section displays a count of resources that failed\. In the following example, the **ComplianceType** is **Custom:DSC** and one resource is non\-compliant\.

**Note**  
Custom:DSC is the default **ComplianceType** value in the AWS\-ApplyDSCMofs document\. This value is customizable\.

![\[Viewing counts in the Compliance resources summary section of the Compliance page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-mof-detailed-status-3.png)

The **Details overview for resources** section displays information about the AWS resource with the non\-compliant DSC resource\. This section also includes the MOF name, script execution steps, and \(when applicable\) a **View output** link to view detailed status information\. 

![\[Viewing compliance details for a MOF execution resource failure\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-mof-detailed-status-1.png)

The **View output** link displays the last 4,000 characters of the detailed status\. Systems Manager starts with the exception as the first element, and then scans back through the verbose messages and prepends as many as it can until it reaches the 4000 character limit\. This process displays the log messages that were output prior to the exception being thrown, which are the most relevant messages for troubleshooting\.

![\[Viewing detailed output for MOF resource compliance issue\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-mof-detailed-status-2.png)

For information about how to view compliance information, see [AWS Systems Manager Configuration Compliance](systems-manager-compliance.md)\.

### Situations that affect compliance reporting<a name="systems-manager-state-manager-viewing-mof-file-compliance-reporting"></a>

If the State Manager association fails, then no compliance data is reported\. More specifically, if a MOF fails to process, then Systems Manager doesn’t report any compliance items because the associations fails\. For example, if Systems Manager attempts to download a MOF from an Amazon S3 bucket that the instance doesn't have permission to access, then the association fails and no compliance data is reported\.

If a resource in a second MOF fails, then Systems Manager *does* report compliance data\. For example, if a MOF tries to create a file on a drive that doesn’t exist, then Systems Manager reports compliance because the AWS\-ApplyDSCMofs document is able to process completely, which means the association successfully runs\. 