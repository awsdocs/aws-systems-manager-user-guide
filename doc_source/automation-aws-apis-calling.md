# Invoking other AWS services from a Systems Manager Automation workflow<a name="automation-aws-apis-calling"></a>

You can invoke other AWS services and other Systems Manager capabilities in your Automation workflow by using the following Automation actions in your Automation documents\. 
+ **aws:executeAwsApi**: This Automation action calls and runs AWS API actions\. Most API actions are supported, although not all API actions have been tested\. For example, the following API actions are supported: [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html), [Delete bucket](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html), [RebootDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html), and [CreateGroups](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateGroup.html), to name a few\. Streaming API actions, such as the [Get Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) action, aren't supported\. 
+ **aws:waitForAwsResourceProperty**: This Automation action enables your workflow to wait for a specific resource state or event state before continuing the workflow\. For example, you can use this action with the Amazon Relational Database Service \(Amazon RDS\) [DescribeDBInstances](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API action to pause an Automation workflow so that a database instance has time to start\.
+ **aws:assertAwsResourceProperty**: This Automation action enables you to assert a specific resource state or event state for a specific Automation step\. For example, you can specify that an Automation step must wait for an EC2 instance to start\. Then it will call the Amazon EC2 [DescribeInstanceStatus](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html) API action with the DesiredValue property of `running`\. This ensures that the Automation workflow waits for a running instance and then continues when the instance is, in fact, running\.

Here is a sample Automation document in YAML that uses the aws:executeAwsApi action to disable read and write permissions on an S3 bucket\.

```
---
description: Disable S3-Bucket's public WriteRead access via private ACL
schemaVersion: "0.3"
assumeRole: "{{ AutomationAssumeRole }}"
parameters:
  S3BucketName:
    type: String
    description: (Required) S3 Bucket subject to access restriction
  AutomationAssumeRole:
    type: String
    description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
    default: ""
mainSteps:
- name: DisableS3BucketPublicReadWrite
  action: aws:executeAwsApi
  inputs:
    Service: s3
    Api:  PutBucketAcl
    Bucket: "{{S3BucketName}}"
    ACL: private
  isEnd: true
...
```

Here is a sample Automation document in YAML that uses all three actions\. The document does the following:
+ Uses the aws:executeAwsApi action to call the Amazon EC2 DescribeImages API action to get the name of a specific Windows Server 2016 AMI\. It outputs the image ID as `ImageId`\.
+ Uses the aws:executeAwsApi action to call the Amazon EC2 RunInstances API action to launch one instance that uses the `ImageId` from the previous step\. It outputs the instance ID as `InstanceId`\.
+ Uses the aws:waitForAwsResourceProperty action to poll the Amazon EC2 DescribeInstanceStatus API action to wait for the instance to reach the `running` state\. The action times out in 60 seconds\. The step times out if the instance state failed to reach `running` after 60 seconds of polling\.
+ Uses the aws:assertAwsResourceProperty action to call the Amazon EC2 DescribeInstanceStatus API action to assert that the instance is in the `running` state\. The step fails if the instance state is not `running`\.

```
---
description: Sample Automation Document Using AWS API Actions
schemaVersion: '0.3'
assumeRole: "{{ AutomationAssumeRole }}"
parameters:
  AutomationAssumeRole:
    type: String
    description: "(Optional) The ARN of the role that allows Automation to perform the actions on your behalf."
    default: ''
  ImageName:
    type: String
    description: "(Optional) Image Name to launch EC2 instance with."
    default: "Windows_Server-2016-English-Full-Base-2018.07.11"
mainSteps:
- name: getImageId
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: DescribeImages
    Filters:  
    - Name: "name"
      Values: 
      - "{{ ImageName }}"
  outputs:
  - Name: ImageId
    Selector: "$.Images[0].ImageId"
    Type: "String"
- name: launchOneInstance
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: RunInstances
    ImageId: "{{ getImageId.ImageId }}"
    MaxCount: 1
    MinCount: 1
  outputs:
  - Name: InstanceId
    Selector: "$.Instances[0].InstanceId"
    Type: "String"
- name: waitUntilInstanceStateRunning
  action: aws:waitForAwsResourceProperty
  # timeout is strongly encouraged for action - aws:waitForAwsResourceProperty
  timeoutSeconds: 60
  inputs:
    Service: ec2
    Api: DescribeInstanceStatus
    InstanceIds:
    - "{{ launchOneInstance.InstanceId }}"
    PropertySelector: "$.InstanceStatuses[0].InstanceState.Name"
    DesiredValues:
    - running
- name: assertInstanceStateRunning
  action: aws:assertAwsResourceProperty
  inputs:
    Service: ec2
    Api: DescribeInstanceStatus
    InstanceIds:
    - "{{ launchOneInstance.InstanceId }}"
    PropertySelector: "$.InstanceStatuses[0].InstanceState.Name"
    DesiredValues:
    - running
outputs:
- "launchOneInstance.InstanceId"
...
```

## Working with inputs and outputs<a name="automation-aws-apis-calling-input-output"></a>

Each of the previously described Automation actions enables you to call a specific API action by specifying the service namespace, the API action name, the input parameters, and the output parameters\. Inputs are defined by the API action that you choose\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.

You can view the schema for each Automation action in the following locations:
+ [aws:assertAwsResourceProperty – Assert an AWS resource state or event state](automation-action-assertAwsResourceProperty.md)
+ [aws:executeAwsApi – Call and run AWS API actions](automation-action-executeAwsApi.md)
+ [aws:waitForAwsResourceProperty – Wait on an AWS resource property](automation-action-waitForAwsResourceProperty.md)

The schemas include descriptions of the required fields for using each action\.

**Using the Selector/PropertySelector Fields**  
Each Automation action requires that you specify either an output `Selector` \(for aws:executeAwsApi\) or a `PropertySelector` \(for aws:assertAwsResourceProperty and aws:waitForAwsResourceProperty\)\. These fields are used to process the JSON response from an AWS API action\. These fields use the JSONPath syntax\.

Here is an example to help illustrate this concept for the `aws:executeAwsAPi` action:

```
---
mainSteps:
- name: getImageId
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: DescribeImages
    Filters:  
      - Name: "name"
        Values: 
          - "{{ ImageName }}"
  outputs:
    - Name: ImageId
      Selector: "$.Images[0].ImageId"
      Type: "String"
...
```

In the `aws:executeAwsApi` step `getImageId`, the workflow invokes the `DescribeImages` API action and receives a response from `ec2`\. The workflow then applies `Selector - "$.Images[0].ImageId"` to the API response and assigns the selected value to the output `ImageId` variable\. Other steps in the same Automation workflow can use the value of `ImageId` by specifying `"{{ getImageId.ImageId }}"`\.

Here is an example to help illustrate this concept for the `aws:waitForAwsResourceProperty` action:

```
---
- name: waitUntilInstanceStateRunning
  action: aws:waitForAwsResourceProperty
  # timeout is strongly encouraged for action - aws:waitForAwsResourceProperty
  timeoutSeconds: 60
  inputs:
    Service: ec2
    Api: DescribeInstanceStatus
    InstanceIds:
    - "{{ launchOneInstance.InstanceId }}"
    PropertySelector: "$.InstanceStatuses[0].InstanceState.Name"
    DesiredValues:
    - running
...
```

In the `aws:waitForAwsResourceProperty` step `waitUntilInstanceStateRunning`, the workflow invokes the `DescribeInstanceStatus` API action and receives a response from `ec2`\. The workflow then applies `PropertySelector - "$.InstanceStatuses[0].InstanceState.Name"` to the response and checks if the specified returned value matches a value in the `DesiredValues` list \(in this case `running`\)\. The step repeats the process until the response returns an instance state of `running`\. 

## Using JSONPath in an Automation workflow<a name="automation-aws-apis-calling-json-path"></a>

A JSONPath expression is a string beginning with "$\." that is used to select one of more components within a JSON element\. The following list includes information about JSONPath operators that are supported by Systems Manager Automation:
+ **Dot\-notated child \(\.\)**: Use with a JSON object\. This operator selects the value of a specific key\.
+ **Deep\-scan \(\.\.\)**: Use with a JSON element\. This operator scans the JSON element level by level and selects a list of values with the specific key\. Note that the return type of this operator is always a JSON array\. In the context of an Automation step output type, the operator can be either StringList or MapList\.
+ **Array\-Index \(\[ \]\)**: Use with a JSON array\. This operator gets the value of a specific index\.

To better understand JSONPath operators, review the following JSON response from the ec2 `DescribeInstances` API action\. Below this response are several examples that show different results by applying different JSONPath expressions to the response from the `DescribeInstances` API action\.

```
{
    "NextToken": "abcdefg",
    "Reservations": [
        {
            "OwnerId": "123456789012",
            "ReservationId": "r-abcd12345678910",
            "Instances": [
                {
                    "ImageId": "ami-12345678",
                    "BlockDeviceMappings": [
                        {
                            "Ebs": {
                                "DeleteOnTermination": true,
                                "Status": "attached",
                                "VolumeId": "vol-000000000000"
                            },
                            "DeviceName": "/dev/xvda"
                        }
                    ],
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    }
                }
            ],
            "Groups": []
        },
        {
            "OwnerId": "123456789012",
            "ReservationId": "r-12345678910abcd",
            "Instances": [
                {
                    "ImageId": "ami-12345678",
                    "BlockDeviceMappings": [
                        {
                            "Ebs": {
                                "DeleteOnTermination": true,
                                "Status": "attached",
                                "VolumeId": "vol-111111111111"
                            },
                            "DeviceName": "/dev/xvda"
                        }
                    ],
                    "State": {
                        "Code": 80,
                        "Name": "stopped"
                    }
                }
            ],
            "Groups": []
        }
    ]
}
```

**JSONPath Example 1: Get a specific String from a JSON response**

```
JSONPath: 
$.Reservations[0].Instances[0].ImageId 

Returns:
"ami-12345678"

Type: String
```

**JSONPath Example 2: Get a specific Boolean from a JSON response**

```
JSONPath:
$.Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.DeleteOnTermination
        
Returns:
true

Type: Boolean
```

**JSONPath Example 3: Get a specific Integer from a JSON response**

```
JSONPath:
$.Reservations[0].Instances[0].State.Code
        
Returns:
16

Type: Integer
```

**JSONPath Example 4: Deep scan a JSON response, then get all of the values for VolumeId as a StringList** 

```
JSONPath:
$.Reservations..BlockDeviceMappings..VolumeId
        
Returns:
[
   "vol-000000000000",
   "vol-111111111111"
]

Type: StringList
```

**JSONPath Example 5: Get a specific BlockDeviceMappings object as a StringMap**

```
JSONPath:
$.Reservations[0].Instances[0].BlockDeviceMappings[0]
        
Returns:
{
   "Ebs" : {
      "DeleteOnTermination" : true,
      "Status" : "attached",
      "VolumeId" : "vol-000000000000"
   },
   "DeviceName" : "/dev/xvda"
}

Type: StringMap
```

**JSONPath Example 6: Deep scan a JSON response, then get all of the State objects as a MapList**

```
JSONPath:
$.Reservations..Instances..State 
    
Returns:
[
   {
      "Code" : 16,
      "Name" : "running"
   },
   {
      "Code" : 80,
      "Name" : "stopped"
   }
]

Type: MapList
```

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

## Sample walkthrough: Start an Amazon RDS instance from a Systems Manager Automation workflow<a name="automation-aws-apis-calling-sample"></a>

This sample walkthrough shows you how to create and run an Automation document in YAML that uses all three API actions to see if an Amazon Relational Database Service \(Amazon RDS\) database instance is running\. If the instance isn't running, the workflow starts it\. 

**To invoke an Amazon RDS API action from a Systems Manager Automation**

1. Open a text editor and paste the following Automation document content into the file\. Specify an Automation role and the instance ID to check\. You will add the mainSteps actions later\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "The_Automation_role_to_use_when_running_the_document"
   parameters:
     InstanceId: The_instance_ID_to_start
       type: String
       description: (Required) RDS instance ID to start
     AutomationAssumeRole:
       type: String
       description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
       default: ""
   mainSteps:
   ```

1. For the first step of the workflow, you need to determine if the instance is already running\. You can use the aws:assertAwsResourceProperty action to determine and assert a specific instance status\. Before you can add the aws:assertAwsResourceProperty action to the document, you must determine and specify the required inputs\. The following list describes how to determine and specify the required inputs\. You can view an example of how to enter this information in the Automation document following the list\.

   1. View the schema to see all available inputs for the [aws:assertAwsResourceProperty – Assert an AWS resource state or event state](automation-action-assertAwsResourceProperty.md) action\.

   1. Determine the namespace of the service to invoke\. You can view a list of AWS service namespaces in [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\. The namespace for Amazon RDS is `rds`\.

   1. Determine which Amazon RDS API action enables you to view the status of a database instance\. You can view the API actions \(also called methods\) on the [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html) page\. 

   1. Specify one or more request parameters for the DescribeDBInstances API action\. For example, this action uses the `DBInstanceIdentifier` request parameter\.

   1. Determine one or more PropertySelectors\. A PropertySelector is a response object that is returned by the request of this API action\. For example, on the [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\. For the purposes of this walkthrough, specify `DBInstances` and `DBInstanceStatus` as the PropertySelectors\. Remember that PropertySelectors are entered by using JSONPath\. This means that you format the information in the Automation document like this:

      `PropertySelector: "$.DBInstances[0].DBInstanceStatus"`\.

   1. Specify one or more DesiredValues\. If you don't know the values you want to specify, then run the [DescribeDBInstances](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API action to determine possible values\. For this walkthrough, specify *available* and *starting*\. 

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "The_Automation_role_to_use_when_running_the_document"
   parameters:
     InstanceId: The_instance_ID_to_start
       type: String
       description: (Required) RDS Instance Id to stop
     AutomationAssumeRole:
       type: String
       description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
       default: ""
   mainSteps:
     -
       name: AssertNotStartingOrAvailable
       action: aws:assertAwsResourceProperty
       isCritical: false
       onFailure: step:StartInstance
       nextStep: CheckStart
       inputs:
         Service: rds
         Api: DescribeDBInstances
         DBInstanceIdentifier: "{{InstanceId}}"
         PropertySelector: "$.DBInstances[0].DBInstanceStatus"
         DesiredValues: ["available", "starting"]
   ```

1. Specify an aws:executeAwsApi action in the mainSteps section to start the instance if the previous action determined that it is not started\.

   1. View the schema to see all available inputs for [aws:executeAwsApi – Call and run AWS API actions](automation-action-executeAwsApi.md)\. 

   1. Specify the Amazon RDS [StartDBInstance](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.start_db_instance) API action to start the instance\. 

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ The_Automation_role_to_use_when_running_the_document }}"
   parameters:
     InstanceId:
       type: String
       description: (Required) RDS Instance Id to stop
     AutomationAssumeRole:
       type: String
       description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
       default: ""
   mainSteps:
     -
       name: AssertNotStartingOrAvailable
       action: aws:assertAwsResourceProperty
       isCritical: false
       onFailure: step:StartInstance
       nextStep: CheckStart
       inputs:
         Service: rds
         Api: DescribeDBInstances
         DBInstanceIdentifier: "{{InstanceId}}"
         PropertySelector: "$.DBInstances[0].DBInstanceStatus"
         DesiredValues: ["available", "starting"]
     -
       name: StartInstance
       action: aws:executeAwsApi
       inputs:
         Service: rds
         Api: StartDBInstance
         DBInstanceIdentifier: "{{InstanceId}}"
   ```

1. Specify an aws:waitForAwsResourceProperty action in the mainSteps section to wait for the instance to start before finishing the Automation workflow\.

   1. View the schema to see all available inputs for the [aws:waitForAwsResourceProperty – Wait on an AWS resource property](automation-action-waitForAwsResourceProperty.md)\. 

   1. Specify the Amazon RDS [DescribeDBInstances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) API action to determine the instance status\. 

   1. Specify `$.DBInstances[0].DBInstanceStatus` as the `PropertySelector`

   1. Specify *available* as the `DesiredValue`\.

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ The_Automation_role_to_use_when_running_the_document }}"
   parameters:
     InstanceId:
       type: String
       description: (Required) RDS Instance Id to stop
     AutomationAssumeRole:
       type: String
       description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
       default: ""
   mainSteps:
     -
       name: AssertNotStartingOrAvailable
       action: aws:assertAwsResourceProperty
       isCritical: false
       onFailure: step:StartInstance
       nextStep: CheckStart
       inputs:
         Service: rds
         Api: DescribeDBInstances
         DBInstanceIdentifier: "{{InstanceId}}"
         PropertySelector: "$.DBInstances[0].DBInstanceStatus"
         DesiredValues: ["available", "starting"]
     -
       name: StartInstance
       action: aws:executeAwsApi
       inputs:
         Service: rds
         Api: StartDBInstance
         DBInstanceIdentifier: "{{InstanceId}}"
     -
       name: CheckStart
       action: aws:waitForAwsResourceProperty
       onFailure: Abort
       maxAttempts: 10
       timeoutSeconds: 600
       inputs:
         Service: rds
         Api: DescribeDBInstances
         DBInstanceIdentifier: "{{InstanceId}}"
         PropertySelector: "$.DBInstances[0].DBInstanceStatus"
         DesiredValues: ["available"]
       isEnd: true
   ...
   ```

1. Save the file as sample\.yaml\.

1. Run the following command in the AWS CLI to add the document to your AWS account\.

   ```
   aws ssm create-document --name sampleDoc --document-type Automation --document-format YAML --content file://sample.yaml
   ```

1. Run the following command to run the Automation execution by using the document you just created\. Make a note of the execution ID returned by Systems Manager after you start the execution\.

   ```
   aws ssm start-automation-execution --document-name sampleDoc 
   ```

1. Run the following command to view the execution status\.

   ```
   aws ssm get-automation-execution --automation-execution-id automation_execution_id
   ```

## Predefined Automation documents that invoke AWS APIs<a name="automation-aws-apis-calling-docs"></a>

Systems Manager Automation includes the following predefined SSM Automation documents that invoke AWS APIs\. 


****  

| Document name | Purpose | 
| --- | --- | 
|  [AWS\-StartRdsInstance](https://console.aws.amazon.com/systems-manager/documents/AWS-StartRdsInstance/description)  |  Start an Amazon RDS instance\.  | 
|  [AWS\-StopRdsInstance](https://console.aws.amazon.com/systems-manager/documents/AWS-StopRdsInstance/description)  |  Stop an Amazon RDS instance\.  | 
|  [AWS\-RebootRdsInstance](https://console.aws.amazon.com/systems-manager/documents/AWS-RebootRdsInstance/description)  |  Reboot an Amazon RDS instance\.  | 
|  [AWS\-CreateSnapshot](https://console.aws.amazon.com/systems-manager/documents/AWS-CreateSnapshot/description)  |  Create an Amazon Elastic Block Store \(Amazon EBS\) volume snapshot\.  | 
|  [AWS\-DeleteSnapshot](https://console.aws.amazon.com/systems-manager/documents/AWS-DeleteSnapshot/description)  |  Delete an Amazon EBS volume snapshot\.  | 
|  [AWS\-ConfigureS3BucketLogging](https://console.aws.amazon.com/systems-manager/documents/AWS-ConfigureS3BucketLogging/description)  |  Enable logging on an Amazon Simple Storage Service \(Amazon S3\) bucket\.   | 
|  [AWS\-DisableS3BucketPublicReadWrite](https://console.aws.amazon.com/systems-manager/documents/AWS-DisableS3BucketPublicReadWrite/description)  |  Disable read and write permissions on an S3 bucket by using a private ACL\.  | 
|  [AWS\-ConfigureS3BucketVersioning](https://console.aws.amazon.com/systems-manager/documents/AWS-ConfigureS3BucketVersioning/description)  |  Enable or suspend versioning on an S3 bucket\.   | 
|  [AWS\-DeleteDynamoDbBackup](https://console.aws.amazon.com/systems-manager/documents/AWS-DeleteDynamoDbBackup/description)  |  Delete a Amazon DynamoDB \(DynamoDB\) table backup\.   | 

Either click the links in the table above, or use the following procedure to view more details about these Automation documents in the Systems Manager console\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a document, and then choose **View details**\.

1. Choose the **Content** tab\.