# aws:copyImage – Copy or encrypt an Amazon Machine Image<a name="automation-action-copyimage"></a>

Copies an AMI from any region into the current region\. This action can also encrypt the new AMI\.

**Input**  
This action supports most CopyImage parameters\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CopyImage.html)\.

The following example creates a copy of an AMI in the Seoul region \(`SourceImageID`: ami\-0fe10819\. `SourceRegion`: ap\-northeast\-2\)\. The new AMI is copied to the region where you initiated the Automation action\. The copied AMI will be encrypted because the optional `Encrypted` flag is set to `true`\.

------
#### [ YAML ]

```
name: createEncryptedCopy
action: aws:copyImage
maxAttempts: 3
onFailure: Abort
inputs:
  SourceImageId: ami-0fe10819
  SourceRegion: ap-northeast-2
  ImageName: Encrypted Copy of LAMP base AMI in ap-northeast-2
  Encrypted: true
```

------
#### [ JSON ]

```
{   
    "name": "createEncryptedCopy",
    "action": "aws:copyImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "SourceImageId": "ami-0fe10819",
        "SourceRegion": "ap-northeast-2",
        "ImageName": "Encrypted Copy of LAMP base AMI in ap-northeast-2",
        "Encrypted": true
    }   
}
```

------

SourceRegion  
The region where the source AMI currently exists\.  
Type: String  
Required: Yes

SourceImageId  
The AMI ID to copy from the source region\.  
Type: String  
Required: Yes

ImageName  
The name for the new image\.  
Type: String  
Required: Yes

ImageDescription  
A description for the target image\.  
Type: String  
Required: No

Encrypted  
Encrypt the target AMI\.  
Type: Boolean  
Required: No

KmsKeyId  
The full Amazon Resource Name \(ARN\) of the AWS Key Management Service CMK to use when encrypting the snapshots of an image during a copy operation\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: No

ClientToken  
A unique, case\-sensitive identifier that you provide to ensure request idempotency\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: NoOutput

ImageId  
The ID of the copied image\.

ImageState  
The state of the copied image\.  
Valid values: `available` \| `pending` \| `failed`