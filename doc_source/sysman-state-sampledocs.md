# Sample State Manager Documents<a name="sysman-state-sampledocs"></a>

This section includes sample State Manager documents\. These samples are kept simple for demonstration purposes\. For more information about creating custom documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

**Sample 1: Get the instance name using the 'aws:runPowerShellScript' plugin**  
The following document includes one step that invokes the `aws:runPowerShellScript ` plugin to return the instance host name\. This document can be run on both Windows and Linux instances\. For more information about Systems Manager plugins, see [Top\-level Elements](ssm-plugins.md#top-level)\.

```
{
   "schemaVersion":"2.2",
   "description":"Sample document",
   "mainSteps":[
      {
         "action":"aws:runPowerShellScript",
         "name":"runPowerShellScript",
         "inputs":{
            "runCommand":[
               "hostname"
            ]
         }
      }
   ]
}
```

**Sample 2: Get the instance name using the 'aws:runShellScript' plugin**  
The following document includes one step that invokes the `aws:runShellScript` plugin to return the instance host name\. This document can be run on Linux instances only\.

```
{
   "schemaVersion":"2.2",
   "description":"Sample document",
   "mainSteps":[
      {
         "action":"aws:runShellScript",
         "name":"runShellScript",
         "inputs":{
            "runCommand":[
               "hostname"
            ]
         }
      }
   ]
}
```

**Sample 3: Enable CloudWatch Logs using the 'aws:cloudWatch' plugin**  
The following document includes one step that invokes the `aws:cloudWatch` plugin to enable Amazon CloudWatch Logs\. This document can be run on Windows instances only\.

```
{
   "schemaVersion":"2.2",
   "description":"Sample document",
   "mainSteps":[
      {
         "action":"aws:cloudWatch",
         "name":"cloudWatch",
         "settings":{
            "startType":"Enabled"
         }
      }
   ]
}
```

**Sample 4: Get the instance name using the 'aws:runPowerShellScript' and 'aws:runShellScript' plugins**  
The following document includes two steps that invoke the `aws:runPowerShellScript` and `aws:runShellScript` plugins to return the instance host name\. This document can be run on Linux instances only\.

```
{
   "schemaVersion":"2.2",
   "description":"Sample document",
   "mainSteps":[
      {
         "action":"aws:runPowerShellScript",
         "name":"runPowerShellScript",
         "inputs":{
            "runCommand":[
               "hostname"
            ]
         }
      },
      {
         "action":"aws:runShellScript",
         "name":"runShellScript",
         "inputs":{
            "runCommand":[
               "hostname"
            ]
         }
      }
   ]
}
```