# Invoking Other AWS Services from a Systems Manager Automation Workflow<a name="automation-aws-apis-calling"></a>

You can invoke other AWS services and other Systems Manager capabilities in your Automation workflow by using the following Automation actions in your Automation documents\. 
+ **aws:executeAwsApi**: This Automation action calls and executes AWS API actions\. Most API actions are supported, although not all API actions have been tested\. For example, the following API actions are supported: [CreateImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html), [Delete bucket](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html), [RebootDBInstance](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html), and [CreateGroups](http://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateGroup.html), to name a few\. Streaming API actions, such as the [Get Object](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) action, aren't supported\. 
+ **aws:waitForAwsResourceProperty**: This Automation action enables your workflow to wait for a specific resource state or event state before continuing the workflow\. For example, you can use this action with the Amazon Relational Database Service \(Amazon RDS\) [DescribeDBInstances](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API action to pause an Automation workflow so that a database instance has time to start\.
+ **aws:assertAwsResourceProperty**: This Automation action enables you to assert a specific resource state or event state for a specific Automation step\. For example, you can specify that an Automation step must wait for an Amazon EC2 instance to start\. Then it will call the Amazon EC2 [DescribeInstanceStatus](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html) API action with the DesiredValue property of `running`\. This ensures that the Automation workflow waits for a running instance and then continues when the instance is, in fact, running\.

Here is a sample Automation document in YAML that uses the aws:executeAwsApi action to disable read and write permissions on an Amazon S3 bucket\.

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
```

Here is a sample Automation document in YAML that uses all three actions\. The document does the following:
+ Uses the aws:executeAwsApi action to call the Amazon EC2 DescribeImages API action to get the name of a specific Windows Server 2016 AMI\. It outputs the image ID number\.
+ Uses the aws:executeAwsApi action to call the Amazon EC2 RunInstances API action to launch one instance that uses the AMI ID from the previous step\. It outputs the instance ID number\.
+ Uses the aws:waitForAwsResourceProperty action to call the Amazon EC2 DescribeInstanceStatus API action to wait for the instance to start\. It outputs the instance state\.
+ Uses the aws:assertAwsResourceProperty action to call the Amazon EC2 DescribeInstanceStatus API action to ensure that the instance started\. It outputs the instance ID\.

```
---
description: Sample Automation Document Using AWS API Actions
schemaVersion: '0.3'
assumeRole: "{{ AutomationAssumeRole }}"
parameters:
  AutomationAssumeRole:
    type: String
    description: "(Optional) The ARN of the role that allows Automation to perform
      the actions on your behalf."
    default: ''
mainSteps:
- name: getImageId
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: DescribeImages
    Filters:  
    - Name: "name"
      Values: 
      - "Windows_Server-2016-English-Full-Base-2018.07.11"
  outputs:
  - Name: Value
    Selector: "$.Images..ImageId"
    Type: "String"
- name: getInstanceId
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: RunInstances
    ImageId: "{{ getImageId.Value }}"
    MaxCount: 1
    MinCount: 1
  outputs:
  - Name: Value
    Selector: "$.Instances..InstanceId"
    Type: "String"
- name: waitStep
  action: aws:waitForAwsResourceProperty
  inputs:
    Service: ec2
    Api: DescribeInstanceStatus
    InstanceIds:
    - "{{ getInstanceId.Value }}"
    PropertySelector: "$.InstanceStatuses..InstanceState.Name"
    DesiredValues:
    - running
- name: assertStep
  action: aws:assertAwsResourceProperty
  inputs:
    Service: ec2
    Api: DescribeInstanceStatus
    InstanceIds:
    - "{{ getInstanceId.Value }}"
    PropertySelector: "$.InstanceStatuses..InstanceState.Name"
    DesiredValues:
    - running
outputs:
- "getInstanceId.Value"
...
```

## Working with Inputs and Outputs<a name="automation-aws-apis-calling-input-output"></a>

Each of the previously described Automation actions enables you to call a specific API action by specifying the service namespace, the API action name, the input parameters, and the output parameters\. Inputs are defined by the API action that you choose\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.

You can view the schema for each Automation action in the following locations:
+ [aws:assertAwsResourceProperty](automation-actions.md#automation-action-assertAwsResourceProperty)
+ [aws:executeAwsApi](automation-actions.md#automation-action-executeAwsApi)
+ [aws:waitForAwsResourceProperty](automation-actions.md#automation-action-waitForAwsResourceProperty)

The schemas include descriptions of the required fields for using each action\.

**Using the Selector/PropertySelector Fields**  
Each Automation action requires that you specify an output Selector or a PropertySelector\. These fields use the jsonpath syntax\.

For example, in the example document shown earlier, the aws:executeAwsApi action specifies the following Selector in the outputs section:

```
mainSteps:
- name: getImageId
  action: aws:executeAwsApi
  inputs:
    Service: ec2
    Api: DescribeImages
    Filters:  
    - Name: "name"
      Values: 
      - "Windows_Server-2016-English-Full-Base-2018.07.11"
  outputs:
  - Name: Value
    Selector: "$.Images..ImageId"
    Type: "String"
```

A jsonpath is a string beginning with $ that you can use to identify components within JSON text\. Note the following details about JSON path selectors for Automation documents:
+ You can access object fields using only dot \(\.\) and square bracket \(\[ \]\) notation\.
+ The operators @ , : ? \* aren't supported\.
+ Functions such as length\(\) aren't supported\. For example, input data contains the following values:

```
{
   "foo":123,
   "bar":[
      "a",
      "b",
      "c"
   ],
   "car":{
      "cdr":true
   }
}
```

In this case, the following jsonpaths would return:

```
$.foo => 123
        $.bar => ["a", "b", "c"]
        $.car.cdr => true
```

For more information about jsonpath, see the [specification](http://goessner.net/articles/JsonPath/) on the internet\.

The aws:waitForAwsResourceProperty action also requires that you specify one or more desired values for the PropertySelector field\. This means the Automation workflow must wait for the desired value before the workflow can continue\. Here is an example\.

```
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
```

If you are unsure about what to specify for DesiredValue, then you can execute the API by using the aws:executeAwsApi action and view the output to see the possible values that you can specify\.

## Sample Walkthrough: Start an Amazon RDS Instance from a Systems Manager Automation Workflow<a name="automation-aws-apis-calling-sample"></a>

This sample walkthrough shows you how to create and execute an Automation document in YAML that uses all three API actions to see if an Amazon Relational Database Service \(Amazon RDS\) database instance is running\. If the instance isn't running, the workflow starts it\. 

**To invoke an Amazon RDS API action from a Systems Manager Automation**

1. Open a text editor and paste the following Automation document content into the file\. Specify an Automation role and the instance ID to check\. You will add the mainSteps actions later\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "The_Automation_role_to_use_when_executing_the_document"
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

   1. View the schema to see all available inputs for the [aws:assertAwsResourceProperty](automation-actions.md#automation-action-assertAwsResourceProperty) action\.

   1. Determine the namespace of the service to invoke\. You can view a list of AWS service namespaces in the [AWS General Reference](http://docs.aws.amazon.com/general/latest/gr//aws-arns-and-namespaces.html)\. The namespace for Amazon RDS is `rds`\.

   1. Determine which Amazon RDS API action enables you to view the status of a database instance\. You can view the API actions \(also called methods\) on the [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html) page\. 

   1. Specify one or more request parameters for the DescribeDBInstances API action\. For example, this action uses the `DBInstanceIdentifier` request parameter\.

   1. Determine one or more PropertySelectors\. A PropertySelector is a response object that is returned by the request of this API action\. For example, on the [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\. For the purposes of this walkthrough, specify `DBInstances` and `DBInstanceStatus` as the PropertySelectors\. Remember that PropertySelectors are entered by using jsonpath\. This means that you format the information in the Automation document like this:

      `PropertySelector: "$.DBInstances[0].DBInstanceStatus"`\.

   1. Specify one or more DesiredValues\. If you don't know the values you want to specify, then execute the [DescribeDBInstances](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html) API action to determine possible values\. For this walkthrough, specify *available* and *starting*\. 

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "The_Automation_role_to_use_when_executing_the_document"
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

   1. View the schema to see all available inputs for [aws:executeAwsApi](automation-actions.md#automation-action-executeAwsApi)\. 

   1. Specify the Amazon RDS [StartDBInstance](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.start_db_instance) API action to start the instance\. 

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ The_Automation_role_to_use_when_executing_the_document }}"
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

   1. View the schema to see all available inputs for the [aws:waitForAwsResourceProperty](automation-actions.md#automation-action-waitForAwsResourceProperty)\. 

   1. Specify the Amazon RDS [DescribeDBInstances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) API action to determine the instance status\. 

   1. Specify `$.DBInstances[0].DBInstanceStatus` as the `PropertySelector`

   1. Specify *available* as the `DesiredValue`\.

   1. Enter the information you collected into the Automation document as shown in the following example\.

   ```
   ---
   description: Start RDS instance
   schemaVersion: "0.3"
   assumeRole: "{{ The_Automation_role_to_use_when_executing_the_document }}"
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

1. Execute the following command in the AWS CLI to add the document to your AWS account\.

   ```
   aws ssm create-document --name sampleDoc --document-type Automation --document-format YAML --content file://sample.yaml
   ```

1. Execute the following command to run the Automation execution by using the document you just created\. Make a note of the execution ID returned by Systems Manager after you start the execution\.

   ```
   aws ssm start-automation-execution --document-name sampleDoc 
   ```

1. Execute the following command to view the execution status\.

   ```
   aws ssm get-automation-execution --automation-execution-id automation_execution_id
   ```
