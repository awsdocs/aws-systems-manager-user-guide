# Invoking other AWS services from a Systems Manager Automation runbook<a name="automation-aws-apis-calling"></a>

You can invoke other AWS services and other Systems Manager capabilities by using the following automation actions in your runbooks: 
+ **`aws:executeAwsApi`**: This automation action calls and runs AWS API operations\. Most API operations are supported, although not all API operations have been tested\. For example, the following API operations are supported: [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html), [Delete bucket](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html), [RebootDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html), and [CreateGroups](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateGroup.html)\. Streaming API operations, such as the [Get Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) action, aren't supported\. 
+ **`aws:waitForAwsResourceProperty`**: This automation action allows your automation to wait for a specific resource state or event state before continuing the automation\. For example, you can use this action with the Amazon Relational Database Service \(Amazon RDS\) [DescribeDBInstances](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API operation to pause an automation so that a database instance has time to start\.
+ **`aws:assertAwsResourceProperty`**: This automation action allows you to assert a specific resource state or event state for a specific step\. For example, you can specify that a step must wait for an EC2 instance to start\. Then it will call the Amazon Elastic Compute Cloud \(Amazon EC2\) [DescribeInstanceStatus](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html) API operation with the DesiredValue property of `running`\. This ensures that the automation waits for a running instance and then continues when the instance is, in fact, running\.

Here is a sample runbook in YAML that uses the `aws:executeAwsApi` action to turn off read and write permissions on an S3 bucket\.

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

Here is a sample runbook in YAML that uses all three actions\. The automation does the following:
+ Uses the `aws:executeAwsApi` action to call the Amazon EC2 DescribeImages API operation to get the name of a specific Windows Server 2016 AMI\. It outputs the image ID as `ImageId`\.
+ Uses the `aws:executeAwsApi` action to call the Amazon EC2 RunInstances API operation to launch one instance that uses the `ImageId` from the previous step\. It outputs the instance ID as `InstanceId`\.
+ Uses the` aws:waitForAwsResourceProperty` action to poll the Amazon EC2 DescribeInstanceStatus API operation to wait for the instance to reach the `running` state\. The action times out in 60 seconds\. The step times out if the instance state failed to reach `running` after 60 seconds of polling\.
+ Uses the `aws:assertAwsResourceProperty` action to call the Amazon EC2 `DescribeInstanceStatus` API operation to assert that the instance is in the `running` state\. The step fails if the instance state isn't `running`\.

```
---
description: Sample runbook using AWS API operations
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
    default: "Windows_Server-2022-English-Full-Base*"
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

Each of the previously described automation actions allows you to call a specific API operation by specifying the service namespace, the API operation name, the input parameters, and the output parameters\. Inputs are defined by the API operation that you choose\. You can view the API operations \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API operations \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.

You can view the schema for each automation action in the following locations:
+ [`aws:assertAwsResourceProperty` – Assert an AWS resource state or event state](automation-action-assertAwsResourceProperty.md)
+ [`aws:executeAwsApi` – Call and run AWS API operations](automation-action-executeAwsApi.md)
+ [`aws:waitForAwsResourceProperty` – Wait on an AWS resource property](automation-action-waitForAwsResourceProperty.md)

The schemas include descriptions of the required fields for using each action\.

**Using the Selector/PropertySelector fields**  
Each Automation action requires that you specify either an output `Selector` \(for `aws:executeAwsApi`\) or a `PropertySelector` \(for `aws:assertAwsResourceProperty` and `aws:waitForAwsResourceProperty`\)\. These fields are used to process the JSON response from an AWS API operation\. These fields use the JSONPath syntax\.

Here is an example to help illustrate this concept for the `aws:executeAwsAPi` action\.

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

In the `aws:executeAwsApi` step `getImageId`, the automation invokes the `DescribeImages` API operation and receives a response from `ec2`\. The automation then applies `Selector - "$.Images[0].ImageId"` to the API response and assigns the selected value to the output `ImageId` variable\. Other steps in the same automation can use the value of `ImageId` by specifying `"{{ getImageId.ImageId }}"`\.

Here is an example to help illustrate this concept for the `aws:waitForAwsResourceProperty` action\.

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

In the `aws:waitForAwsResourceProperty` step `waitUntilInstanceStateRunning`, the automation invokes the `DescribeInstanceStatus` API operation and receives a response from `ec2`\. The automation then applies `PropertySelector - "$.InstanceStatuses[0].InstanceState.Name"` to the response and checks if the specified returned value matches a value in the `DesiredValues` list \(in this case `running`\)\. The step repeats the process until the response returns an instance state of `running`\. 

## Using JSONPath in a runbook<a name="automation-aws-apis-calling-json-path"></a>

A JSONPath expression is a string beginning with "$\." that is used to select one of more components within a JSON element\. The following list includes information about JSONPath operators that are supported by Systems Manager Automation:
+ **Dot\-notated child \(\.\)**: Use with a JSON object\. This operator selects the value of a specific key\.
+ **Deep\-scan \(\.\.\)**: Use with a JSON element\. This operator scans the JSON element level by level and selects a list of values with the specific key\. The return type of this operator is always a JSON array\. In the context of an automation action output type, the operator can be either StringList or MapList\.
+ **Array\-Index \(\[ \]\)**: Use with a JSON array\. This operator gets the value of a specific index\.

To better understand JSONPath operators, review the following JSON response from the ec2 `DescribeInstances` API operation\. Following this response are several examples that show different results by applying different JSONPath expressions to the response from the `DescribeInstances` API operation\.

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
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation runbooks \(`AWS-*` runbooks\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` runbooks, to name a few\. This requirement also applies to any custom Automation runbooks you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, configure the service role with permission to invoke those services\. You can give permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

## Start an Amazon RDS instance from a Systems Manager Automation runbook<a name="automation-aws-apis-calling-sample"></a>

This sample walkthrough shows you how to create and run a Systems Manager Automation runbook in YAML that uses all three API operations to see if an Amazon Relational Database Service \(Amazon RDS\) database instance is running\. If the DB instance isn't running, the automation starts it\. 

**To invoke an Amazon RDS API operation from a runbook**

1. Open a text editor and paste the following runbook content into the file\. Replace each *example resource placeholder* with your own information\. You will add the `mainSteps` actions later\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "IAM service role"
   parameters:
     InstanceId: db instance ID
       type: String
       description: (Required) RDS instance ID to start
     AutomationAssumeRole:
       type: String
       description: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.
       default: ""
   mainSteps:
   ```

1. For the first step of the automation, you need to determine if the instance is already running\. You can use the `aws:assertAwsResourceProperty` action to determine and assert a specific instance status\. Before you can add the `aws:assertAwsResourceProperty` action to the runbook, you must determine and specify the required inputs\. The following list describes how to determine and specify the required inputs\. You can view an example of how to enter this information in the runbook following the list\.

   1. View the schema to see all available inputs for the [`aws:assertAwsResourceProperty` – Assert an AWS resource state or event state](automation-action-assertAwsResourceProperty.md) action\.

   1. Determine the namespace of the service to invoke\. To find a service namespace, open the [Service Authorization Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference.html), open the page for the service, and find the phrase "service prefix" in the first sentence\. For example, the following text appears in the first sentence on the page for Amazon RDS:

      \(service prefix: `rds`\)"

   1. Determine which Amazon RDS API operation allows you to view the status of a database instance\. You can view the API operations \(also called methods\) on the [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html) page\. 

   1. Specify one or more request parameters for the DescribeDBInstances API operation\. For example, this action uses the `DBInstanceIdentifier` request parameter\.

   1. Determine one or more PropertySelectors\. A PropertySelector is a response object that is returned by the request of this API operation\. For example, on the [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\. For the purposes of this walkthrough, specify `DBInstances` and `DBInstanceStatus` as the PropertySelectors\. Remember that PropertySelectors are entered by using JSONPath\. This means that you format the information in the runbook like the following\.

      `PropertySelector: "$.DBInstances[0].DBInstanceStatus"`\.

   1. Specify one or more DesiredValues\. If you don't know the values you want to specify, then run the [DescribeDBInstances](AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API operation to determine possible values\. For this walkthrough, specify *available* and *starting*\. 

   1. Enter the information you collected into the runbook as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "IAM service role"
   parameters:
     InstanceId: db instance ID
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

1. Specify an `aws:executeAwsApi` action in the mainSteps section to start the instance if the previous action determined that it isn't started\.

   1. View the schema to see all available inputs for [`aws:executeAwsApi` – Call and run AWS API operations](automation-action-executeAwsApi.md)\. 

   1. Specify the Amazon RDS [StartDBInstance](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.start_db_instance) API operation to start the instance\. 

   1. Enter the information you collected into the runbook as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ IAM service role }}"
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

1. Specify an `aws:waitForAwsResourceProperty` action in the mainSteps section to wait for the instance to start before finishing the automation\.

   1. View the schema to see all available inputs for the [`aws:waitForAwsResourceProperty` – Wait on an AWS resource property](automation-action-waitForAwsResourceProperty.md)\. 

   1. Specify the Amazon RDS [DescribeDBInstances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) API operation to determine the instance status\. 

   1. Specify `$.DBInstances[0].DBInstanceStatus` as the `PropertySelector`

   1. Specify *available* as the `DesiredValue`\.

   1. Enter the information you collected into the runbook as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ IAM service role }}"
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

1. Run the following command in the AWS CLI to add the runbook to your AWS account\. Replace each *example resource placeholder* with your own information\.

   ```
   aws ssm create-document \
       --name runbook name --document-type Automation --document-format YAML --content file://sample.yaml
   ```

1. Run the following command to start the automation by using the runbook you just created\. Make a note of the execution ID returned by Systems Manager after you start the automation\.

   ```
   aws ssm start-automation-execution \
       --document-name runbook name
   ```

1. Run the following command to view the execution status\.

   ```
   aws ssm get-automation-execution \
       --automation-execution-id automation execution ID
   ```