# Additional Automation CLI Examples<a name="automation-addcli"></a>

You can manage other aspects of Automation execution using the following tasks\.

**Stop an Execution**  
Run the following to stop a workflow\. The command doesn't terminate associated instances\.

```
aws ssm stop-automation-execution --automation-execution-id ID
```

**Create Versions of Automation Documents**  
You can't change an existing automation document, but you can create a new version using the following command:

```
aws ssm update-document --name "patchWindowsAmi" --content file:///Users/test-user/Documents/patchWindowsAmi.json --document-version "\$LATEST"
```

Run the following command to view details about the existing document versions:

```
aws ssm list-document-versions --name "patchWindowsAmi"
```

The command returns information like the following:

```
{
    "DocumentVersions": [
        {
            "IsDefaultVersion": false, 
            "Name": "patchWindowsAmi", 
            "DocumentVersion": "2", 
            "CreatedDate": 1475799950.484
        }, 
        {
            "IsDefaultVersion": false, 
            "Name": "patchWindowsAmi", 
            "DocumentVersion": "1", 
            "CreatedDate": 1475799931.064
        }
    ]
}
```

Run the following command to update the default version for execution\. The default execution version only changes when you explicitly set it to a new version\. Creating a new document version does not change the default version\.

```
aws ssm update-document-default-version --name patchWindowsAmi --document-version 2
```

**Delete a Document**  
Run the following command to delete an automation document:

```
aws ssm delete-document --name patchWindowsAMI
```