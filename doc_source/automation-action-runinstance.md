# aws:runInstances – Launch an EC2 instance<a name="automation-action-runinstance"></a>

Launches a new instance\.

**Input**  
The action supports most API parameters\. For more information, see the [RunInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html) API documentation\.

------
#### [ YAML ]

```
name: launchInstance
action: aws:runInstances
maxAttempts: 3
timeoutSeconds: 1200
onFailure: Abort
inputs:
  ImageId: ami-12345678
  InstanceType: t2.micro
  MinInstanceCount: 1
  MaxInstanceCount: 1
  IamInstanceProfileName: myRunCmdRole
  TagSpecifications:
  - ResourceType: instance
    Tags:
    - Key: LaunchedBy
      Value: SSMAutomation
    - Key: Category
      Value: HighAvailabilityFleetHost
```

------
#### [ JSON ]

```
{
   "name":"launchInstance",
   "action":"aws:runInstances",
   "maxAttempts":3,
   "timeoutSeconds":1200,
   "onFailure":"Abort",
   "inputs":{
      "ImageId":"ami-12345678",
      "InstanceType":"t2.micro",
      "MinInstanceCount":1,
      "MaxInstanceCount":1,
      "IamInstanceProfileName":"myRunCmdRole",
      "TagSpecifications":[
         {
            "ResourceType":"instance",
            "Tags":[
               {
                  "Key":"LaunchedBy",
                  "Value":"SSMAutomation"
               },
               {
                  "Key":"Category",
                  "Value":"HighAvailabilityFleetHost"
               }
            ]
         }
      ]
   }
}
```

------

ImageId  
The ID of the Amazon Machine Image \(AMI\)\.  
Type: String  
Required: Yes

InstanceType  
The instance type\.  
If an instance type value is not provided, the m1\.small instance type is used\.
Type: String  
Required: No

MinInstanceCount  
The minimum number of instances to be launched\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to be launched\.  
Type: String  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: MapList  
Required: No

ClientToken  
The identifier to ensure idempotency of the request\.  
Type: String  
Required: No

DisableApiTermination  
Enables or disables instance API termination  
Type: Boolean  
Required: No

EbsOptimized  
Enables or disabled EBS optimization\.  
Type: Boolean  
Required: No

IamInstanceProfileArn  
The ARN of the IAM instance profile for the instance\.  
Type: String  
Required: No

IamInstanceProfileName  
The name of the IAM instance profile for the instance\.  
Type: String  
Required: No

InstanceInitiatedShutdownBehavior  
Indicates whether the instance stops or terminates on system shutdown\.  
Type: String  
Required: No

KernelId  
The ID of the kernel\.  
Type: String  
Required: No

KeyName  
The name of the key pair\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

MinInstanceCount  
The minimum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

Monitoring  
Enables or disables detailed monitoring\.  
Type: Boolean  
Required: No

NetworkInterfaces  
The network interfaces\.  
Type: MapList  
Required: No

Placement  
The placement for the instance\.  
Type: StringMap  
Required: No

PrivateIpAddress  
The primary IPv4 address\.  
Type: String  
Required: No

RamdiskId  
The ID of the RAM disk\.  
Type: String  
Required: No

SecurityGroupIds  
The IDs of the security groups for the instance\.  
Type: StringList  
Required: No

SecurityGroups  
The names of the security groups for the instance\.  
Type: StringList  
Required: No

SubnetId  
The subnet ID\.  
Type: String  
Required: No

TagSpecifications  
The tags to apply to the resources during launch\. You can only tag instances and volumes at launch\. The specified tags are applied to all instances or volumes that are created during launch\. To tag an instance after it has been launched, use the [aws:createTags – Create tags for AWS resources](automation-action-createtag.md) action\.  
Type: MapList \(For more information, see [TagSpecification](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_TagSpecification.html)\.\)  
Required: No

UserData  
An execution script provided as a string literal value\. If a literal value is entered, then it must be Base64\-encoded\.  
Type: String  
Required: NoOutput

InstanceIds  
The IDs of the instances\.