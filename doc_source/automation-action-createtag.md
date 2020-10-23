# aws:createTags – Create tags for AWS resources<a name="automation-action-createtag"></a>

Create new tags for EC2 instances or Systems Manager managed instances\.

**Input**  
This action supports most EC2 CreateTags and SSM AddTagsToResource parameters\. For more information, see [CreateTags](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_createtags.html) and [AddTagsToResource](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_addtagstoresource.html)\.

The following example shows how to tag an AMI and an instance as being production resources for a particular department\.

------
#### [ YAML ]

```
name: createTags
action: aws:createTags
maxAttempts: 3
onFailure: Abort
inputs:
  ResourceType: EC2
  ResourceIds:
  - ami-9a3768fa
  - i-02951acd5111a8169
  Tags:
  - Key: production
    Value: ''
  - Key: department
    Value: devops
```

------
#### [ JSON ]

```
{
    "name": "createTags",
    "action": "aws:createTags",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "ResourceType": "EC2",
        "ResourceIds": [
            "ami-9a3768fa",
            "i-02951acd5111a8169"
        ],
        "Tags": [
            {
                "Key": "production",
                "Value": ""
            },
            {
                "Key": "department",
                "Value": "devops"
            }
        ]
    }
}
```

------

ResourceIds  
The IDs of the resource\(s\) to be tagged\. If resource type is not “EC2”, this field can contain only a single item\.  
Type: String List  
Required: Yes

Tags  
The tags to associate with the resource\(s\)\.  
Type: List of Maps  
Required: Yes

ResourceType  
The type of resource\(s\) to be tagged\. If not supplied, the default value of “EC2” is used\.  
Type: String  
Required: No  
Valid Values: `EC2` \| `ManagedInstance` \| `MaintenanceWindow` \| `Parameter`

**Output**  
None