# aws:createImage – Create an Amazon Machine Image<a name="automation-action-create"></a>

Creates a new AMI from an instance that is either running or stopped\. 

**Input**  
This action supports most CreateImage parameters\. For more information, see [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html)\.

------
#### [ YAML ]

```
name: createMyImage
action: aws:createImage
maxAttempts: 3
onFailure: Abort
inputs:
  InstanceId: i-1234567890abcdef0
  ImageName: AMI Created on{{global:DATE_TIME}}
  NoReboot: true
  ImageDescription: My newly created AMI
```

------
#### [ JSON ]

```
{
    "name": "createMyImage",
    "action": "aws:createImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "InstanceId": "i-1234567890abcdef0",
        "ImageName": "AMI Created on{{global:DATE_TIME}}",
        "NoReboot": true,
        "ImageDescription": "My newly created AMI"
    }
}
```

------

InstanceId  
The ID of the instance\.  
Type: String  
Required: Yes

ImageName  
The name for the image\.  
Type: String  
Required: Yes

ImageDescription  
A description of the image\.  
Type: String  
Required: No

NoReboot  
A boolean literal\.  
By default, Amazon EC2 attempts to shut down and reboot the instance before creating the image\. If the **No Reboot** option is set to `true`, Amazon EC2 doesn't shut down the instance before creating the image\. When this option is used, file system integrity on the created image can't be guaranteed\.   
If you do not want the instance to run after you create an AMI image from it, first use the [aws:changeInstanceState – Change or assert instance state](automation-action-changestate.md) action to stop the instance, and then use this `aws:createImage` action with the **NoReboot** option set to `true`\.  
Type: Boolean  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: Map  
Required: NoOutput

ImageId  
The ID of the newly created image\.

ImageState  
The current state of the image\. If the state is available, the image is successfully registered and can be used to launch an instance\.