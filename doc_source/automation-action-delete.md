# aws:deleteImage – Delete an Amazon Machine Image<a name="automation-action-delete"></a>

Deletes the specified image and all related snapshots\.

**Input**  
This action supports only one parameter\. For more information, see the documentation for [DeregisterImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeregisterImage.html) and [DeleteSnapshot](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeleteSnapshot.html)\.

------
#### [ YAML ]

```
name: deleteMyImage
action: aws:deleteImage
maxAttempts: 3
timeoutSeconds: 180
onFailure: Abort
inputs:
  ImageId: ami-12345678
```

------
#### [ JSON ]

```
{
    "name": "deleteMyImage",
    "action": "aws:deleteImage",
    "maxAttempts": 3,
    "timeoutSeconds": 180,
    "onFailure": "Abort",
    "inputs": {
        "ImageId": "ami-12345678"
    }
}
```

------

ImageId  
The ID of the image to be deleted\.  
Type: String  
Required: Yes

**Output**  
None