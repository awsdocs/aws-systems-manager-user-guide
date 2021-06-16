# Using the BaselineOverride parameter<a name="patch-manager-about-baselineoverride"></a>

You can define patching preferences at runtime using the baseline override feature in Patch Manager, a capability of AWS Systems Manager\. Do this by specifying an Amazon Simple Storage Service \(Amazon S3\) bucket containing a JSON object with a list of patch baselines\. The patching operation uses the baselines provided in the JSON object that match the host operating system instead of applying the rules from the default patch baseline\.

**Note**  
Using the `BaselineOverride` parameter doesn't overwrite the patch compliance of the baseline provided in the parameter\. The output results are recorded in the Stdout logs from Run Command, a capability of AWS Systems Manager\. The results only print out packages that are marked as `NON_COMPLIANT`\. This means the package is marked as `Missing`, `Failed`, `InstalledRejected`, or `InstalledPendingReboot`\.

## Using the patch baseline override with Snapshot Id or Install Override List parameters<a name="patch-manager-about-baselineoverride-other-parameters"></a>

There are two cases where the patch baseline override has noteworthy behavior\.

**Using baseline override and Snapshot Id at the same time**  
Snapshot Ids ensure that all instances in a particular patching command all apply the same thing\. For example, if you patch 1,000 instances at one time, the patches will be the same\.

When using both a Snapshot Id and a patch baseline override, the Snapshot Id takes precedence over the patch baseline override\. The baseline override rules will still be used, but they will only be evaluated once\. In the earlier example, the patches across your 1,000 instances will still always be the same\. If, midway through the patching operation, you changed the JSON file in the referenced S3 bucket to be something different, the patches applied will still be the same\. This is because the Snapshot Id was provided\.

**Using baseline override and Install Override List at the same time**  
You can't use these two parameters at the same time\. The patching document fails if both parameters are supplied, and it doesn't perform any scans or installs on the instance\.

## Code examples<a name="patch-manager-about-baselineoverride-code"></a>

The following code example for Python shows how to generate the patch baseline override\.

```
import boto3
import json

ssm = boto3.client('ssm')
s3 = boto3.resource('s3')
s3_bucket_name = 'my-baseline-override-bucket'
s3_file_name = 'MyBaselineOverride.json'
baseline_ids_to_export = ['pb-0000000000000000', 'pb-0000000000000001']

baseline_overrides = []
for baseline_id in baseline_ids_to_export:
    baseline_overrides.append(ssm.get_patch_baseline(
        BaselineId=baseline_id
    ))

json_content = json.dumps(baseline_overrides, indent=4, sort_keys=True, default=str)
s3.Object(bucket_name=s3_bucket_name, key=s3_file_name).put(Body=json_content)
```

This produces a patch baseline override like the following\.

```
[
    {
        "ApprovalRules": {
            "PatchRules": [
                {
                    "ApproveAfterDays": 0, 
                    "ComplianceLevel": "UNSPECIFIED", 
                    "EnableNonSecurity": false, 
                    "PatchFilterGroup": {
                        "PatchFilters": [
                            {
                                "Key": "PRODUCT", 
                                "Values": [
                                    "*"
                                ]
                            }, 
                            {
                                "Key": "CLASSIFICATION", 
                                "Values": [
                                    "*"
                                ]
                            }, 
                            {
                                "Key": "SEVERITY", 
                                "Values": [
                                    "*"
                                ]
                            }
                        ]
                    }
                }
            ]
        }, 
        "ApprovedPatches": [], 
        "ApprovedPatchesComplianceLevel": "UNSPECIFIED", 
        "ApprovedPatchesEnableNonSecurity": false, 
        "GlobalFilters": {
            "PatchFilters": []
        }, 
        "OperatingSystem": "AMAZON_LINUX_2", 
        "RejectedPatches": [], 
        "RejectedPatchesAction": "ALLOW_AS_DEPENDENCY", 
        "Sources": []
    }, 
    {
        "ApprovalRules": {
            "PatchRules": [
                {
                    "ApproveUntilDate": "2021-01-06", 
                    "ComplianceLevel": "UNSPECIFIED", 
                    "EnableNonSecurity": true, 
                    "PatchFilterGroup": {
                        "PatchFilters": [
                            {
                                "Key": "PRODUCT", 
                                "Values": [
                                    "*"
                                ]
                            }, 
                            {
                                "Key": "CLASSIFICATION", 
                                "Values": [
                                    "*"
                                ]
                            }, 
                            {
                                "Key": "SEVERITY", 
                                "Values": [
                                    "*"
                                ]
                            }
                        ]
                    }
                }
            ]
        }, 
        "ApprovedPatches": [
            "open-ssl*"
        ], 
        "ApprovedPatchesComplianceLevel": "UNSPECIFIED", 
        "ApprovedPatchesEnableNonSecurity": false, 
        "GlobalFilters": {
            "PatchFilters": []
        }, 
        "OperatingSystem": "CENTOS", 
        "RejectedPatches": [
            "python*"
        ], 
        "RejectedPatchesAction": "ALLOW_AS_DEPENDENCY", 
        "Sources": []
    }
]
```
