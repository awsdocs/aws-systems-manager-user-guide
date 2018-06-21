# AWS CLI Commands for Patch Manager<a name="patch-manager-cli-commands"></a>

The section includes examples of CLI commands that you can use to perform Patch Manager configuration tasks\.

For an illustration of using the AWS CLI to patch a server environment by using a custom patch baseline, see [Walkthrough: Patch a Server Environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.

For more information about using the CLI forAWS Systems Manager tasks, see the [AWS Systems Manager section of the AWS CLI Command Reference](http://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\. 

**Topics**
+ [Create a patch baseline](#patch-manager-cli-commands-create-patch-baseline)
+ [Create a patch baseline with custom repositories for different OS versions](#patch-manager-cli-commands-create-patch-baseline-mult-sources)
+ [Update a patch baseline](#patch-manager-cli-commands-update-patch-baseline)
+ [Rename a patch baseline](#patch-manager-cli-commands-rename-patch-baseline)
+ [Delete a patch baseline](#patch-manager-cli-commands-delete-patch-baseline)
+ [List all patch baselines](#patch-manager-cli-commands-describe-patch-baselines)
+ [List all AWS\-provided patch baselines](#patch-manager-cli-commands-describe-patch-baselines-aws)
+ [List my patch baselines](#patch-manager-cli-commands-describe-patch-baselines-custom)
+ [Display a patch baseline](#patch-manager-cli-commands-get-patch-baseline)
+ [Get the default patch baseline](#patch-manager-cli-commands-get-default-patch-baseline)
+ [Set the default patch baseline](#patch-manager-cli-commands-register-default-patch-baseline)
+ [Register a patch group "Web Servers" with a patch baseline](#patch-manager-cli-commands-register-patch-baseline-for-patch-group-web-servers)
+ [Register a patch group "Backend" with the AWS\-provided patch baseline](#patch-manager-cli-commands-register-patch-baseline-for-patch-group-backend)
+ [Display patch group registrations](#patch-manager-cli-commands-describe-patch-groups)
+ [Deregister a patch group from a patch baseline](#patch-manager-cli-commands-deregister-patch-baseline-for-patch-group)
+ [Get all patches defined by a patch baseline](#patch-manager-cli-commands-describe-effective-patches-for-patch-baseline)
+ [Get all patches for Windows Server 2012 that have a MSRC severity of Critical](#patch-manager-cli-commands-describe-available-patches)
+ [Get all available patches](#patch-manager-cli-commands-describe-available-patches)
+ [Tag a patch baseline](#patch-manager-cli-commands-add-tags-to-resource)
+ [List the tags for a patch baseline](#patch-manager-cli-commands-list-tags-for-resource)
+ [Remove a tag from a patch baseline](#patch-manager-cli-commands-remove-tags-from-resource)
+ [Get patch summary states per\-instance](#patch-manager-cli-commands-describe-instance-patch-states)
+ [Get patch compliance details for an instance](#patch-manager-cli-commands-describe-instance-patches)

## Create a patch baseline<a name="patch-manager-cli-commands-create-patch-baseline"></a>

The following command creates a patch baseline that approves all critical and important security updates for Windows Server 2012 R2 five days after they are released\.

```
aws ssm create-patch-baseline --name "Windows-Server-2012R2" --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Important,Critical]},{Key=CLASSIFICATION,Values=SecurityUpdates},{Key=PRODUCT,Values=WindowsServer2012R2}]},ApproveAfterDays=5}]" --description "Windows Server 2012 R2, Important and Critical security updates"
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE""
}
```

## Create a patch baseline with custom repositories for different OS versions<a name="patch-manager-cli-commands-create-patch-baseline-mult-sources"></a>

Applies to Linux instances only\. The following command shows how to specify the patch repository to use for a particular version of the Amazon Linux operating system\. This sample uses a source repository enabled by default on Amazon Linux 2017\.09, but could be adapted to a different source repository that you have configured for an instance\.

**Note**  
To better demonstrate this more complex command, we are using the \-\-cli\-input\-json option with additional options stored an external JSON file\.

1. Create a JSON file with a name like `my-patch-repository.json` and add the following content to it:

   ```
   {
       "Description": "My patch repository for Amazon Linux 2017.09",
       "Name": "Amazon-Linux-2017.09",
       "OperatingSystem": "AMAZON_LINUX",
       "ApprovalRules": {
           "PatchRules": [
               {
                   "ApproveAfterDays": 7,
                   "EnableNonSecurity": true,
                   "PatchFilterGroup": {
                       "PatchFilters": [
                           {
                               "Key": "SEVERITY",
                               "Values": [
                                   "Important",
                                   "Critical"
                               ]
                           },
                           {
                               "Key": "CLASSIFICATION",
                               "Values": [
                                   "Security",
                                   "Bugfix"
                               ]
                           },
                           {
                               "Key": "PRODUCT",
                               "Values": [
                                   "AmazonLinux2017.09"
                               ]
                           }
                       ]
                   }
               }
           ]
       },
       "Sources": [
           {
               "Name": "My-AL2017.09",
               "Products": [
                   "AmazonLinux2017.09"
               ],
               "Configuration": "[amzn-main] \nname=amzn-main-Base\nmirrorlist=http://repo./$awsregion./$awsdomain//$releasever/main/mirror.list //nmirrorlist_expire=300//nmetadata_expire=300 \npriority=10 \nfailovermethod=priority \nfastestmirror_enabled=0 \ngpgcheck=1 \ngpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-amazon-ga \nenabled=1 \nretries=3 \ntimeout=5\nreport_instanceid=yes"
           }
       ]
   }
   ```

1. In the directory where you saved the file, run the following command:

   ```
   aws ssm create-patch-baseline --cli-input-json file://my-patch-repository.json
   ```

   The system returns information like the following:

   ```
   {
       "BaselineId": "pb-12343b962ba63wxya"
   }
   ```

## Update a patch baseline<a name="patch-manager-cli-commands-update-patch-baseline"></a>

The following command adds two patches as rejected and one patch as approved to an existing patch baseline\.

**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

```
aws ssm update-patch-baseline --baseline-id pb-0c10e65780EXAMPLE" --rejected-patches "KB2032276" "MS10-048" --approved-patches "KB2124261"
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"",
   "Name":"Windows-Server-2012R2",
   "RejectedPatches":[
      "KB2032276",
      "MS10-048"
   ],
   "GlobalFilters":{
      "PatchFilters":[

      ]
   },
   "ApprovalRules":{
      "PatchRules":[
         {
            "PatchFilterGroup":{
               "PatchFilters":[
                  {
                     "Values":[
                        "Important",
                        "Critical"
                     ],
                     "Key":"MSRC_SEVERITY"
                  },
                  {
                     "Values":[
                        "SecurityUpdates"
                     ],
                     "Key":"CLASSIFICATION"
                  },
                  {
                     "Values":[
                        "WindowsServer2012R2"
                     ],
                     "Key":"PRODUCT"
                  }
               ]
            },
            "ApproveAfterDays":5
         }
      ]
   },
   "ModifiedDate":1481001494.035,
   "CreatedDate":1480997823.81,
   "ApprovedPatches":[
      "KB2124261"
   ],
   "Description":"Windows Server 2012 R2, Important and Critical security updates"
}
```

## Rename a patch baseline<a name="patch-manager-cli-commands-rename-patch-baseline"></a>

```
aws ssm update-patch-baseline --baseline-id pb-0c10e65780EXAMPLE" --name "Windows-Server-2012-R2-Important-and-Critical-Security-Updates"
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"",
   "Name":"Windows-Server-2012-R2-Important-and-Critical-Security-Updates",
   "RejectedPatches":[
      "KB2032276",
      "MS10-048"
   ],
   "GlobalFilters":{
      "PatchFilters":[

      ]
   },
   "ApprovalRules":{
      "PatchRules":[
         {
            "PatchFilterGroup":{
               "PatchFilters":[
                  {
                     "Values":[
                        "Important",
                        "Critical"
                     ],
                     "Key":"MSRC_SEVERITY"
                  },
                  {
                     "Values":[
                        "SecurityUpdates"
                     ],
                     "Key":"CLASSIFICATION"
                  },
                  {
                     "Values":[
                        "WindowsServer2012R2"
                     ],
                     "Key":"PRODUCT"
                  }
               ]
            },
            "ApproveAfterDays":5
         }
      ]
   },
   "ModifiedDate":1481001795.287,
   "CreatedDate":1480997823.81,
   "ApprovedPatches":[
      "KB2124261"
   ],
   "Description":"Windows Server 2012 R2, Important and Critical security updates"
}
```

## Delete a patch baseline<a name="patch-manager-cli-commands-delete-patch-baseline"></a>

```
aws ssm delete-patch-baseline --baseline-id "pb-0c10e65780EXAMPLE""
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE""
}
```

## List all patch baselines<a name="patch-manager-cli-commands-describe-patch-baselines"></a>

```
aws ssm describe-patch-baselines
```

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"AWS-DefaultPatchBaseline",
         "DefaultBaseline":true,
         "BaselineDescription":"Default Patch Baseline Provided by AWS.",
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
      },
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE""
      }
   ]
}
```

Here is another command that lists all patch baselines in a Region\.

```
aws ssm describe-patch-baselines --region us-east-2 --filters "Key=OWNER,Values=[All]"
```

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"AWS-DefaultPatchBaseline",
         "DefaultBaseline":true,
         "BaselineDescription":"Default Patch Baseline Provided by AWS.",
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
      },
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE""
      }
   ]
}
```

## List all AWS\-provided patch baselines<a name="patch-manager-cli-commands-describe-patch-baselines-aws"></a>

```
aws ssm describe-patch-baselines --region us-east-2 --filters "Key=OWNER,Values=[AWS]"
```

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"AWS-DefaultPatchBaseline",
         "DefaultBaseline":true,
         "BaselineDescription":"Default Patch Baseline Provided by AWS.",
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
      }
   ]
}
```

## List my patch baselines<a name="patch-manager-cli-commands-describe-patch-baselines-custom"></a>

```
aws ssm describe-patch-baselines --region us-east-2 --filters "Key=OWNER,Values=[Self]"
```

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE""
      }
   ]
}
```

## Display a patch baseline<a name="patch-manager-cli-commands-get-patch-baseline"></a>

```
aws ssm get-patch-baseline --baseline-id pb-0c10e65780EXAMPLE"
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"",
   "Name":"Windows-Server-2012R2",
   "PatchGroups":[
      "Web Servers"
   ],
   "RejectedPatches":[

   ],
   "GlobalFilters":{
      "PatchFilters":[

      ]
   },
   "ApprovalRules":{
      "PatchRules":[
         {
            "PatchFilterGroup":{
               "PatchFilters":[
                  {
                     "Values":[
                        "Important",
                        "Critical"
                     ],
                     "Key":"MSRC_SEVERITY"
                  },
                  {
                     "Values":[
                        "SecurityUpdates"
                     ],
                     "Key":"CLASSIFICATION"
                  },
                  {
                     "Values":[
                        "WindowsServer2012R2"
                     ],
                     "Key":"PRODUCT"
                  }
               ]
            },
            "ApproveAfterDays":5
         }
      ]
   },
   "ModifiedDate":1480997823.81,
   "CreatedDate":1480997823.81,
   "ApprovedPatches":[

   ],
   "Description":"Windows Server 2012 R2, Important and Critical security updates"
}
```

## Get the default patch baseline<a name="patch-manager-cli-commands-get-default-patch-baseline"></a>

```
aws ssm get-default-patch-baseline --region us-east-2
```

The system returns information like the following\.

```
{
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
}
```

## Set the default patch baseline<a name="patch-manager-cli-commands-register-default-patch-baseline"></a>

```
aws ssm register-default-patch-baseline --region us-east-2 --baseline-id "pb-0c10e65780EXAMPLE""
```

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE""
}
```

## Register a patch group "Web Servers" with a patch baseline<a name="patch-manager-cli-commands-register-patch-baseline-for-patch-group-web-servers"></a>

```
aws ssm register-patch-baseline-for-patch-group --baseline-id "pb-0c10e65780EXAMPLE"" --patch-group "Web Servers"
```

The system returns information like the following\.

```
{
   "PatchGroup":"Web Servers",
   "BaselineId":"pb-0c10e65780EXAMPLE""
}
```

## Register a patch group "Backend" with the AWS\-provided patch baseline<a name="patch-manager-cli-commands-register-patch-baseline-for-patch-group-backend"></a>

```
aws ssm register-patch-baseline-for-patch-group --region us-east-2 --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"" --patch-group "Backend"
```

The system returns information like the following\.

```
{
   "PatchGroup":"Backend",
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
}
```

## Display patch group registrations<a name="patch-manager-cli-commands-describe-patch-groups"></a>

```
aws ssm describe-patch-groups --region us-east-2
```

The system returns information like the following\.

```
{
   "PatchGroupPatchBaselineMappings":[
      {
         "PatchGroup":"Backend",
         "BaselineIdentity":{
            "BaselineName":"AWS-DefaultPatchBaseline",
            "DefaultBaseline":false,
            "BaselineDescription":"Default Patch Baseline Provided by AWS.",
            "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
         }
      },
      {
         "PatchGroup":"Web Servers",
         "BaselineIdentity":{
            "BaselineName":"Windows-Server-2012R2",
            "DefaultBaseline":true,
            "BaselineDescription":"Windows Server 2012 R2, Important and Critical updates",
            "BaselineId":"pb-0c10e65780EXAMPLE""
         }
      }
   ]
}
```

## Deregister a patch group from a patch baseline<a name="patch-manager-cli-commands-deregister-patch-baseline-for-patch-group"></a>

```
aws ssm deregister-patch-baseline-for-patch-group --region us-east-2 --patch-group "Production" --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
```

The system returns information like the following\.

```
{
   "PatchGroup":"Production",
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE""
}
```

## Get all patches defined by a patch baseline<a name="patch-manager-cli-commands-describe-effective-patches-for-patch-baseline"></a>

```
aws ssm describe-effective-patches-for-patch-baseline --region us-east-2 --baseline-id "pb-0c10e65780EXAMPLE""
```

The system returns information like the following\.

```
{
   "NextToken":"--token string truncated--",
   "EffectivePatches":[
      {
         "PatchStatus":{
            "ApprovalDate":1384711200.0,
            "DeploymentStatus":"APPROVED"
         },
         "Patch":{
            "ContentUrl":"https://support.microsoft.com/en-us/kb/2876331",
            "ProductFamily":"Windows",
            "Product":"WindowsServer2012R2",
            "Vendor":"Microsoft",
            "Description":"A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "Classification":"SecurityUpdates",
            "Title":"Security Update for Windows Server 2012 R2 Preview (KB2876331)",
            "ReleaseDate":1384279200.0,
            "MsrcClassification":"Critical",
            "Language":"All",
            "KbNumber":"KB2876331",
            "MsrcNumber":"MS13-089",
            "Id":"e74ccc76-85f0-4881-a738-59e9fc9a336d"
         }
      },
      {
         "PatchStatus":{
            "ApprovalDate":1428858000.0,
            "DeploymentStatus":"APPROVED"
         },
         "Patch":{
            "ContentUrl":"https://support.microsoft.com/en-us/kb/2919355",
            "ProductFamily":"Windows",
            "Product":"WindowsServer2012R2",
            "Vendor":"Microsoft",
            "Description":"Windows Server 2012 R2 Update is a cumulative set of security updates, critical updates and updates. You must install Windows Server 2012 R2 Update to ensure that your computer can continue to receive future Windows Updates, including security updates. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article for more information. After you install this item, you may have to restart your computer.",
            "Classification":"SecurityUpdates",
            "Title":"Windows Server 2012 R2 Update (KB2919355)",
            "ReleaseDate":1428426000.0,
            "MsrcClassification":"Critical",
            "Language":"All",
            "KbNumber":"KB2919355",
            "MsrcNumber":"MS14-018",
            "Id":"8452bac0-bf53-4fbd-915d-499de08c338b"
         }
      }
     ---output truncated---
```

## Get all patches for Windows Server 2012 that have a MSRC severity of Critical<a name="patch-manager-cli-commands-describe-available-patches"></a>

```
aws ssm describe-available-patches --region us-east-2 --filters Key=PRODUCT,Values=WindowsServer2012 Key=MSRC_SEVERITY,Values=Critical
```

The system returns information like the following\.

```
{
   "Patches":[
      {
         "ContentUrl":"https://support.microsoft.com/en-us/kb/2727528",
         "ProductFamily":"Windows",
         "Product":"WindowsServer2012",
         "Vendor":"Microsoft",
         "Description":"A security issue has been identified that could allow an unauthenticated remote attacker to compromise your system and gain control over it. You can help protect your system by installing this update from Microsoft. After you install this update, you may have to restart your system.",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Windows Server 2012 (KB2727528)",
         "ReleaseDate":1352829600.0,
         "MsrcClassification":"Critical",
         "Language":"All",
         "KbNumber":"KB2727528",
         "MsrcNumber":"MS12-072",
         "Id":"1eb507be-2040-4eeb-803d-abc55700b715"
      },
      {
         "ContentUrl":"https://support.microsoft.com/en-us/kb/2729462",
         "ProductFamily":"Windows",
         "Product":"WindowsServer2012",
         "Vendor":"Microsoft",
         "Description":"A security issue has been identified that could allow an unauthenticated remote attacker to compromise your system and gain control over it. You can help protect your system by installing this update from Microsoft. After you install this update, you may have to restart your system.",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Microsoft .NET Framework 3.5 on Windows 8 and Windows Server 2012 for x64-based Systems (KB2729462)",
         "ReleaseDate":1352829600.0,
         "MsrcClassification":"Critical",
         "Language":"All",
         "KbNumber":"KB2729462",
         "MsrcNumber":"MS12-074",
         "Id":"af873760-c97c-4088-ab7e-5219e120eab4"
      }
     
---output truncated---
```

## Get all available patches<a name="patch-manager-cli-commands-describe-available-patches"></a>

```
aws ssm describe-available-patches --region us-east-2
```

The system returns information like the following\.

```
{
   "NextToken":"--token string truncated--",
   "Patches":[
      {
         "ContentUrl":"https://support.microsoft.com/en-us/kb/2032276",
         "ProductFamily":"Windows",
         "Product":"WindowsServer2008R2",
         "Vendor":"Microsoft",
         "Description":"A security issue has been identified that could allow an unauthenticated remote attacker to compromise your system and gain control over it. You can help protect your system by installing this update from Microsoft. After you install this update, you may have to restart your system.",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Windows Server 2008 R2 x64 Edition (KB2032276)",
         "ReleaseDate":1279040400.0,
         "MsrcClassification":"Important",
         "Language":"All",
         "KbNumber":"KB2032276",
         "MsrcNumber":"MS10-043",
         "Id":"8692029b-a3a2-4a87-a73b-8ea881b4b4d6"
      },
      {
         "ContentUrl":"https://support.microsoft.com/en-us/kb/2124261",
         "ProductFamily":"Windows",
         "Product":"Windows7",
         "Vendor":"Microsoft",
         "Description":"A security issue has been identified that could allow an unauthenticated remote attacker to compromise your system and gain control over it. You can help protect your system by installing this update from Microsoft. After you install this update, you may have to restart your system.",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Windows 7 (KB2124261)",
         "ReleaseDate":1284483600.0,
         "MsrcClassification":"Important",
         "Language":"All",
         "KbNumber":"KB2124261",
         "MsrcNumber":"MS10-065",
         "Id":"12ef1bed-0dd2-4633-b3ac-60888aa8ba33"
      }
      ---output truncated---
```

## Tag a patch baseline<a name="patch-manager-cli-commands-add-tags-to-resource"></a>

```
aws ssm add-tags-to-resource --resource-type "PatchBaseline" --resource-id "pb-0c10e65780EXAMPLE"" --tags "Key=Project,Value=Testing"
```

## List the tags for a patch baseline<a name="patch-manager-cli-commands-list-tags-for-resource"></a>

```
aws ssm list-tags-for-resource --resource-type "PatchBaseline" --resource-id "pb-0c10e65780EXAMPLE""
```

## Remove a tag from a patch baseline<a name="patch-manager-cli-commands-remove-tags-from-resource"></a>

```
aws ssm remove-tags-from-resource --resource-type "PatchBaseline" --resource-id "pb-0c10e65780EXAMPLE"" --tag-keys "Project"
```

## Get patch summary states per\-instance<a name="patch-manager-cli-commands-describe-instance-patch-states"></a>

The per\-instance summary gives you a number of patches in the following states per instance: "NotApplicable", "Missing", "Failed", "InstalledOther" and "Installed"\. 

```
aws ssm describe-instance-patch-states --instance-ids i-08ee91c0b17045407 i-09a618aec652973a9 i-0a00def7faa94f1c i-0fff3aab684d01b23
```

The system returns information like the following\.

```
{
   "InstancePatchStates":[
      {
         "OperationStartTime":"2016-12-09T05:00:00Z",
         "FailedCount":0,
         "InstanceId":"i-08ee91c0b17045407",
         "OwnerInformation":"",
         "NotApplicableCount":2077,
         "OperationEndTime":"2016-12-09T05:02:37Z",
         "PatchGroup":"Production",
         "InstalledOtherCount":186,
         "MissingCount":7,
         "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
         "Operation":"Scan",
         "InstalledCount":72
      },
      {
         "OperationStartTime":"2016-12-09T04:59:09Z",
         "FailedCount":0,
         "InstanceId":"i-09a618aec652973a9",
         "OwnerInformation":"",
         "NotApplicableCount":1637,
         "OperationEndTime":"2016-12-09T05:03:57Z",
         "PatchGroup":"Production",
         "InstalledOtherCount":388,
         "MissingCount":2,
         "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
         "Operation":"Scan",
         "InstalledCount":141
      }
     ---output truncated---
```

## Get patch compliance details for an instance<a name="patch-manager-cli-commands-describe-instance-patches"></a>

```
aws ssm describe-instance-patches --instance-id i-08ee91c0b17045407
```

The system returns information like the following\.

```
{
   "NextToken":"--token string truncated--",
   "Patches":[
      {
         "KBId":"KB2919355",
         "Severity":"Critical",
         "Classification":"SecurityUpdates",
         "Title":"Windows 8.1 Update for x64-based Systems (KB2919355)",
         "State":"Installed",
         "InstalledTime":"2014-03-18T12:00:00Z"
      },
      {
         "KBId":"KB2977765",
         "Severity":"Important",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Microsoft .NET Framework 4.5.1 and 4.5.2 on Windows 8.1 and Windows Server 2012 R2 x64-based Systems (KB2977765)",
         "State":"Installed",
         "InstalledTime":"2014-10-15T12:00:00Z"
      },
      {
         "KBId":"KB2978126",
         "Severity":"Important",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Microsoft .NET Framework 4.5.1 and 4.5.2 on Windows 8.1 (KB2978126)",
         "State":"Installed",
         "InstalledTime":"2014-11-18T12:00:00Z"
      },
    ---output truncated---
```