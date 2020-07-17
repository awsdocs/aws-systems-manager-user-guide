# Working with Patch Manager \(AWS CLI\)<a name="patch-manager-cli-commands"></a>

The section includes examples of CLI commands that you can use to perform Patch Manager configuration tasks\.

For an illustration of using the AWS CLI to patch a server environment by using a custom patch baseline, see [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.

For more information about using the CLI for AWS Systems Manager tasks, see the [AWS Systems Manager section of the AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\. 

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
+ [Set a custom patch baseline as the default](#patch-manager-cli-commands-register-default-patch-baseline)
+ [Reset an AWS patch baseline as the default](#patch-manager-cli-commands-register-aws-patch-baseline)
+ [Create a patch group](#patch-manager-cli-commands-create-patch-group)
+ [Tag a patch baseline](#patch-manager-cli-commands-add-tags-to-resource)
+ [Register a patch group "web servers" with a patch baseline](#patch-manager-cli-commands-register-patch-baseline-for-patch-group-web-servers)
+ [Register a patch group "Backend" with the AWS\-provided patch baseline](#patch-manager-cli-commands-register-patch-baseline-for-patch-group-backend)
+ [Display patch group registrations](#patch-manager-cli-commands-describe-patch-groups)
+ [Deregister a patch group from a patch baseline](#patch-manager-cli-commands-deregister-patch-baseline-for-patch-group)
+ [Get all patches defined by a patch baseline](#patch-manager-cli-commands-describe-effective-patches-for-patch-baseline)
+ [Get all patches for Windows Server 2012 that have a MSRC severity of Critical](#patch-manager-cli-commands-describe-available-patches)
+ [Get all available patches](#patch-manager-cli-commands-describe-available-patches)
+ [List the tags for a patch baseline](#patch-manager-cli-commands-list-tags-for-resource)
+ [Remove a tag from a patch baseline](#patch-manager-cli-commands-remove-tags-from-resource)
+ [Get patch summary states per\-instance](#patch-manager-cli-commands-describe-instance-patch-states)
+ [Get patch compliance details for an instance](#patch-manager-cli-commands-describe-instance-patches)

## Create a patch baseline<a name="patch-manager-cli-commands-create-patch-baseline"></a>

The following command creates a patch baseline that approves all critical and important security updates for Windows Server 2012 R2 five days after they are released\. Patches have also been specified for the Approved and Rejected patch lists\. In addition, the patch baseline has been tagged to indicate that it is for a production environment\.

------
#### [ Linux ]

```
aws ssm create-patch-baseline \
    --name "Windows-Server-2012R2" \
    --tags "Key=Environment,Value=Production" \
    --description "Windows Server 2012 R2, Important and Critical security updates" \
    --approved-patches "KB2032276,MS10-048" \
    --rejected-patches "KB2124261" \
    --rejected-patches-action "ALLOW_AS_DEPENDENCY" \
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Important,Critical]},{Key=CLASSIFICATION,Values=SecurityUpdates},{Key=PRODUCT,Values=WindowsServer2012R2}]},ApproveAfterDays=5}]"
```

------
#### [ Windows ]

```
aws ssm create-patch-baseline ^
    --name "Windows-Server-2012R2" ^
    --tags "Key=Environment,Value=Production" ^
    --description "Windows Server 2012 R2, Important and Critical security updates" ^
    --approved-patches "KB2032276,MS10-048" ^
    --rejected-patches "KB2124261" ^
    --rejected-patches-action "ALLOW_AS_DEPENDENCY" ^
    --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Important,Critical]},{Key=CLASSIFICATION,Values=SecurityUpdates},{Key=PRODUCT,Values=WindowsServer2012R2}]},ApproveAfterDays=5}]"
```

------

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"
}
```

## Create a patch baseline with custom repositories for different OS versions<a name="patch-manager-cli-commands-create-patch-baseline-mult-sources"></a>

Applies to Linux instances only\. The following command shows how to specify the patch repository to use for a particular version of the Amazon Linux operating system\. This sample uses a source repository enabled by default on Amazon Linux 2017\.09, but could be adapted to a different source repository that you have configured for an instance\.

**Note**  
To better demonstrate this more complex command, we are using the `--cli-input-json` option with additional options stored an external JSON file\.

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
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

------
#### [ Linux ]

```
aws ssm update-patch-baseline \
    --baseline-id pb-0c10e65780EXAMPLE \
    --rejected-patches "KB2032276" "MS10-048" \
    --approved-patches "KB2124261"
```

------
#### [ Windows ]

```
aws ssm update-patch-baseline ^
    --baseline-id pb-0c10e65780EXAMPLE ^
    --rejected-patches "KB2032276" "MS10-048" ^
    --approved-patches "KB2124261"
```

------

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE",
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

------
#### [ Linux ]

```
aws ssm update-patch-baseline \
    --baseline-id pb-0c10e65780EXAMPLE \
    --name "Windows-Server-2012-R2-Important-and-Critical-Security-Updates"
```

------
#### [ Windows ]

```
aws ssm update-patch-baseline ^
    --baseline-id pb-0c10e65780EXAMPLE ^
    --name "Windows-Server-2012-R2-Important-and-Critical-Security-Updates"
```

------

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE",
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
aws ssm delete-patch-baseline --baseline-id "pb-0c10e65780EXAMPLE"
```

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"
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
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
      },
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE"
      }
   ]
}
```

Here is another command that lists all patch baselines in a Region\.

------
#### [ Linux ]

```
aws ssm describe-patch-baselines \
    --region us-east-2 \
    --filters "Key=OWNER,Values=[All]"
```

------
#### [ Windows ]

```
aws ssm describe-patch-baselines ^
    --region us-east-2 ^
    --filters "Key=OWNER,Values=[All]"
```

------

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"AWS-DefaultPatchBaseline",
         "DefaultBaseline":true,
         "BaselineDescription":"Default Patch Baseline Provided by AWS.",
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
      },
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE"
      }
   ]
}
```

## List all AWS\-provided patch baselines<a name="patch-manager-cli-commands-describe-patch-baselines-aws"></a>

------
#### [ Linux ]

```
aws ssm describe-patch-baselines \
    --region us-east-2 \
    --filters "Key=OWNER,Values=[AWS]"
```

------
#### [ Windows ]

```
aws ssm describe-patch-baselines ^
    --region us-east-2 ^
    --filters "Key=OWNER,Values=[AWS]"
```

------

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"AWS-DefaultPatchBaseline",
         "DefaultBaseline":true,
         "BaselineDescription":"Default Patch Baseline Provided by AWS.",
         "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
      }
   ]
}
```

## List my patch baselines<a name="patch-manager-cli-commands-describe-patch-baselines-custom"></a>

------
#### [ Linux ]

```
aws ssm describe-patch-baselines \
    --region us-east-2 \
    --filters "Key=OWNER,Values=[Self]"
```

------
#### [ Windows ]

```
aws ssm describe-patch-baselines ^
    --region us-east-2 ^
    --filters "Key=OWNER,Values=[Self]"
```

------

The system returns information like the following\.

```
{
   "BaselineIdentities":[
      {
         "BaselineName":"Windows-Server-2012R2",
         "DefaultBaseline":false,
         "BaselineDescription":"Windows Server 2012 R2, Important and Critical security updates",
         "BaselineId":"pb-0c10e65780EXAMPLE"
      }
   ]
}
```

## Display a patch baseline<a name="patch-manager-cli-commands-get-patch-baseline"></a>

```
aws ssm get-patch-baseline --baseline-id pb-0c10e65780EXAMPLE
```

**Note**  
For custom patch baselines, you can specify either the patch baseline ID or the full ARN\. For AWS\-provided patch baseline, you must specify the full ARN\. For example, `arn:aws:ssm:us-east-1:075727635805:patchbaseline/pb-03e3f588eec25344c`\.

The system returns information like the following\.

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE",
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
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
}
```

## Set a custom patch baseline as the default<a name="patch-manager-cli-commands-register-default-patch-baseline"></a>

------
#### [ Linux ]

```
aws ssm register-default-patch-baseline \
    --region us-east-2 \
    --baseline-id "pb-0c10e65780EXAMPLE"
```

------
#### [ Windows ]

```
aws ssm register-default-patch-baseline ^
    --region us-east-2 ^
    --baseline-id "pb-0c10e65780EXAMPLE"
```

------

The system returns information like the following:

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"
}
```

## Reset an AWS patch baseline as the default<a name="patch-manager-cli-commands-register-aws-patch-baseline"></a>

------
#### [ Linux ]

```
aws ssm register-default-patch-baseline \
    --region us-east-2 \
    --baseline-id "arn:aws:ssm:us-east-2:733109147000:patchbaseline/pb-0574b43a65ea646ed"
```

------
#### [ Windows ]

```
aws ssm register-default-patch-baseline ^
    --region us-east-2 ^
    --baseline-id "arn:aws:ssm:us-east-2:733109147000:patchbaseline/pb-0574b43a65ea646ed"
```

------

The system returns information like the following:

```
{
   "BaselineId":"pb-0c10e65780EXAMPLE"
}
```

## Create a patch group<a name="patch-manager-cli-commands-create-patch-group"></a>

To help you organize your patching efforts, we recommend that you add instances to patch groups by using tags\. Patch groups require use of the tag key **Patch Group**\. You can specify any tag value, but the tag key must be **Patch Group**\. For more information about patch groups, see [About patch groups](sysman-patch-patchgroups.md)\.

After you group your instances using tags, you add the patch group value to a patch baseline\. By registering the patch group with a patch baseline, you ensure that the correct patches are installed during the patching operation\.

### Task 1:Add EC2 instances to a patch group using tags<a name="create-patch-group-cli-1"></a>

**Note**  
When using the Amazon EC2 console and AWS CLI, it's possible to apply `Key = Patch Group` tags to instances that aren't yet configured for use with Systems Manager\. Ensure that SSM Agent is installed and running on instances that you want to manage using Systems Manager\. For more information, see [Working with SSM Agent](ssm-agent.md)\.

Run the following command to add the `Patch Group` tag to an EC2 instance\.

```
aws ec2 create-tags --resources "i-1234567890abcdef0" --tags "Key=Patch Group,Value=GroupValue"
```

### Task 2: Add managed instances to a patch group using tags<a name="create-patch-group-cli-2"></a>

Run the following command to add the `Patch Group` tag to a managed instance\.

------
#### [ Linux ]

```
aws ssm add-tags-to-resource \
    --resource-type "ManagedInstance" \
    --resource-id "mi-0123456789abcdefg" \
    --tags "Key=Patch Group,Value=GroupValue"
```

------
#### [ Windows ]

```
aws ssm add-tags-to-resource ^
    --resource-type "ManagedInstance" ^
    --resource-id "mi-0123456789abcdefg" ^
    --tags "Key=Patch Group,Value=GroupValue"
```

------

### Task 3: Add a patch group to a patch baseline<a name="create-patch-group-cli-3"></a>

Run the following command to associate a `Patch Group` tag value to the specified patch baseline\.

------
#### [ Linux ]

```
aws ssm register-patch-baseline-for-patch-group \
    --baseline-id "pb-0123456789abcdef0" \
    --patch-group "Development"
```

------
#### [ Windows ]

```
aws ssm register-patch-baseline-for-patch-group ^
    --baseline-id "pb-0123456789abcdef0" ^
    --patch-group "Development"
```

------

The system returns information like the following:

```
{
  "PatchGroup": "Development",
  "BaselineId": "pb-0123456789abcdef0"
}
```

## Tag a patch baseline<a name="patch-manager-cli-commands-add-tags-to-resource"></a>

------
#### [ Linux ]

```
aws ssm add-tags-to-resource \
    --resource-type "PatchBaseline" \
    --resource-id "pb-0c10e65780EXAMPLE" \
    --tags "Key=Project,Value=Testing"
```

------
#### [ Windows ]

```
aws ssm add-tags-to-resource ^
    --resource-type "PatchBaseline" ^
    --resource-id "pb-0c10e65780EXAMPLE" ^
    --tags "Key=Project,Value=Testing"
```

------

## Register a patch group "web servers" with a patch baseline<a name="patch-manager-cli-commands-register-patch-baseline-for-patch-group-web-servers"></a>

------
#### [ Linux ]

```
aws ssm register-patch-baseline-for-patch-group \
    --baseline-id "pb-0c10e65780EXAMPLE" \
    --patch-group "Web Servers"
```

------
#### [ Windows ]

```
aws ssm register-patch-baseline-for-patch-group ^
    --baseline-id "pb-0c10e65780EXAMPLE" ^
    --patch-group "Web Servers"
```

------

The system returns information like the following\.

```
{
   "PatchGroup":"Web Servers",
   "BaselineId":"pb-0c10e65780EXAMPLE"
}
```

## Register a patch group "Backend" with the AWS\-provided patch baseline<a name="patch-manager-cli-commands-register-patch-baseline-for-patch-group-backend"></a>

------
#### [ Linux ]

```
aws ssm register-patch-baseline-for-patch-group \
    --region us-east-2 \
    --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE" \
    --patch-group "Backend"
```

------
#### [ Windows ]

```
aws ssm register-patch-baseline-for-patch-group ^
    --region us-east-2 ^
    --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE" ^
    --patch-group "Backend"
```

------

The system returns information like the following\.

```
{
   "PatchGroup":"Backend",
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
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
            "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
         }
      },
      {
         "PatchGroup":"Web Servers",
         "BaselineIdentity":{
            "BaselineName":"Windows-Server-2012R2",
            "DefaultBaseline":true,
            "BaselineDescription":"Windows Server 2012 R2, Important and Critical updates",
            "BaselineId":"pb-0c10e65780EXAMPLE"
         }
      }
   ]
}
```

## Deregister a patch group from a patch baseline<a name="patch-manager-cli-commands-deregister-patch-baseline-for-patch-group"></a>

------
#### [ Linux ]

```
aws ssm deregister-patch-baseline-for-patch-group \
    --region us-east-2 \
    --patch-group "Production" \
    --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
```

------
#### [ Windows ]

```
aws ssm deregister-patch-baseline-for-patch-group ^
    --region us-east-2 ^
    --patch-group "Production" ^
    --baseline-id "arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
```

------

The system returns information like the following\.

```
{
   "PatchGroup":"Production",
   "BaselineId":"arn:aws:ssm:us-east-2:111122223333:patchbaseline/pb-0c10e65780EXAMPLE"
}
```

## Get all patches defined by a patch baseline<a name="patch-manager-cli-commands-describe-effective-patches-for-patch-baseline"></a>

------
#### [ Linux ]

*This command is supported for Windows Server patch baselines only\.*

------
#### [ Windows ]

```
aws ssm describe-effective-patches-for-patch-baseline ^
    --region us-east-2 ^
    --baseline-id "pb-0c10e65780EXAMPLE"
```

------

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
            "Description":"A security issue has been identified in a Microsoft software 
               product that could affect your system. You can help protect your system 
               by installing this update from Microsoft. For a complete listing of the 
               issues that are included in this update, see the associated Microsoft 
               Knowledge Base article. After you install this update, you may have to 
               restart your system.",
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
            "Description":"Windows Server 2012 R2 Update is a cumulative 
               set of security updates, critical updates and updates. You 
               must install Windows Server 2012 R2 Update to ensure that 
               your computer can continue to receive future Windows Updates, 
               including security updates. For a complete listing of the 
               issues that are included in this update, see the associated 
               Microsoft Knowledge Base article for more information. After 
               you install this item, you may have to restart your computer.",
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

------
#### [ Linux ]

```
aws ssm describe-available-patches \
    --region us-east-2 \
    --filters Key=PRODUCT,Values=WindowsServer2012 Key=MSRC_SEVERITY,Values=Critical
```

------
#### [ Windows ]

```
aws ssm describe-available-patches ^
    --region us-east-2 ^
    --filters Key=PRODUCT,Values=WindowsServer2012 Key=MSRC_SEVERITY,Values=Critical
```

------

The system returns information like the following\.

```
{
   "Patches":[
      {
         "ContentUrl":"https://support.microsoft.com/en-us/kb/2727528",
         "ProductFamily":"Windows",
         "Product":"WindowsServer2012",
         "Vendor":"Microsoft",
         "Description":"A security issue has been identified that could 
           allow an unauthenticated remote attacker to compromise your 
           system and gain control over it. You can help protect your 
           system by installing this update from Microsoft. After you 
           install this update, you may have to restart your system.",
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
         "Description":"A security issue has been identified that could 
           allow an unauthenticated remote attacker to compromise your 
           system and gain control over it. You can help protect your 
           system by installing this update from Microsoft. After you 
           install this update, you may have to restart your system.",
         "Classification":"SecurityUpdates",
         "Title":"Security Update for Microsoft .NET Framework 3.5 on 
           Windows 8 and Windows Server 2012 for x64-based Systems (KB2729462)",
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
         "Description":"A security issue has been identified that could allow an 
           unauthenticated remote attacker to compromise your system and gain 
           control over it. You can help protect your system by installing this 
           update from Microsoft. After you install this update, you may have to
           restart your system.",
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
         "Description":"A security issue has been identified that could allow 
           an unauthenticated remote attacker to compromise your system and gain 
           control over it. You can help protect your system by installing this 
           update from Microsoft. After you install this update, you may have 
           to restart your system.",
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

## List the tags for a patch baseline<a name="patch-manager-cli-commands-list-tags-for-resource"></a>

------
#### [ Linux ]

```
aws ssm list-tags-for-resource \
    --resource-type "PatchBaseline" \
    --resource-id "pb-0c10e65780EXAMPLE"
```

------
#### [ Windows ]

```
aws ssm list-tags-for-resource ^
    --resource-type "PatchBaseline" ^
    --resource-id "pb-0c10e65780EXAMPLE"
```

------

## Remove a tag from a patch baseline<a name="patch-manager-cli-commands-remove-tags-from-resource"></a>

------
#### [ Linux ]

```
aws ssm remove-tags-from-resource \
    --resource-type "PatchBaseline" \
    --resource-id "pb-0c10e65780EXAMPLE" \
    --tag-keys "Project"
```

------
#### [ Windows ]

```
aws ssm remove-tags-from-resource ^
    --resource-type "PatchBaseline" ^
    --resource-id "pb-0c10e65780EXAMPLE" ^
    --tag-keys "Project"
```

------

## Get patch summary states per\-instance<a name="patch-manager-cli-commands-describe-instance-patch-states"></a>

The per\-instance summary gives you a number of patches in the following states per instance: "NotApplicable", "Missing", "Failed", "InstalledOther" and "Installed"\. 

------
#### [ Linux ]

```
aws ssm describe-instance-patch-states \
    --instance-ids i-08ee91c0b17045407 i-09a618aec652973a9
```

------
#### [ Windows ]

```
aws ssm describe-instance-patch-states ^
    --instance-ids i-08ee91c0b17045407 i-09a618aec652973a9
```

------

The system returns information like the following\.

```
{
   "InstancePatchStates":[
      {
            "InstanceId": "i-08ee91c0b17045407",
            "PatchGroup": "",
            "BaselineId": "pb-0e392de35e7c563b7",
            "SnapshotId": "6d03d6c5-f79d-41d0-8d0e-00a9aEXAMPLE",
            "InstalledCount": 50,
            "InstalledOtherCount": 353,
            "InstalledPendingRebootCount": 0,
            "InstalledRejectedCount": 0,
            "MissingCount": 0,
            "FailedCount": 0,
            "UnreportedNotApplicableCount": -1,
            "NotApplicableCount": 671,
            "OperationStartTime": "2020-01-24T12:37:56-08:00",
            "OperationEndTime": "2020-01-24T12:37:59-08:00",
            "Operation": "Scan",
            "RebootOption": "NoReboot"
        },
        {
            "InstanceId": "i-09a618aec652973a9",
            "PatchGroup": "",
            "BaselineId": "pb-07e6d4e9bc703f2e3",
            "SnapshotId": "c7e0441b-1eae-411b-8aa7-973e6EXAMPLE",
            "InstalledCount": 36,
            "InstalledOtherCount": 396,
            "InstalledPendingRebootCount": 0,
            "InstalledRejectedCount": 0,
            "MissingCount": 3,
            "FailedCount": 0,
            "UnreportedNotApplicableCount": -1,
            "NotApplicableCount": 420,
            "OperationStartTime": "2020-01-24T12:37:34-08:00",
            "OperationEndTime": "2020-01-24T12:37:37-08:00",
            "Operation": "Scan",
            "RebootOption": "NoReboot"
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
            "Title": "bind-libs.x86_64:32:9.8.2-0.68.rc1.60.amzn1",
            "KBId": "bind-libs.x86_64",
            "Classification": "Security",
            "Severity": "Important",
            "State": "Installed",
            "InstalledTime": "2019-08-26T11:05:24-07:00"
        },
        {
            "Title": "bind-utils.x86_64:32:9.8.2-0.68.rc1.60.amzn1",
            "KBId": "bind-utils.x86_64",
            "Classification": "Security",
            "Severity": "Important",
            "State": "Installed",
            "InstalledTime": "2019-08-26T11:05:32-07:00"
        },
        {
            "Title": "dhclient.x86_64:12:4.1.1-53.P1.28.amzn1",
            "KBId": "dhclient.x86_64",
            "Classification": "Security",
            "Severity": "Important",
            "State": "Installed",
            "InstalledTime": "2019-08-26T11:05:31-07:00"
        },
    ---output truncated---
```