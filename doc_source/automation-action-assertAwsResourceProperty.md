# aws:assertAwsResourceProperty – Assert an AWS resource state or event state<a name="automation-action-assertAwsResourceProperty"></a>

The aws:assertAwsResourceProperty action enables you to assert a specific resource state or event state for a specific Automation step\. For example, you can specify that an Automation step must wait for an EC2 instance to start\. Then it will call the Amazon EC2 [DescribeInstanceStatus](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html) API action with the DesiredValue property of `running`\. This ensures that the Automation workflow waits for a running instance and then continues when the instance is, in fact, running\.

For more information and examples of how to use this action, see [Invoking other AWS services from a Systems Manager Automation workflow](automation-aws-apis-calling.md)\.

**Note**  
The default timeout value for this action is 3600 seconds \(one hour\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an `aws:assertAwsResourceProperty` step\.

**Input**  
Inputs are defined by the API action that you choose\. 

------
#### [ YAML ]

```
action: aws:assertAwsResourceProperty
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
  PropertySelector: Response object
  DesiredValues:
  - Desired property values
```

------
#### [ JSON ]

```
{
  "action": "aws:assertAwsResourceProperty",
  "inputs": {
    "Service":"The official namespace of the service",
    "Api":"The API action or method name",
    "API action inputs or parameters":"A value",
    "PropertySelector": "Response object",
    "DesiredValues": [
      "Desired property values"
    ]
  }
}
```

------

Service  
The AWS service namespace that contains the API action that you want to run\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon EC2 is `ec2`\. You can view a list of supported AWS service namespaces in the [Available Services](https://docs.aws.amazon.com/cli/latest/reference/#available-services) section of the *AWS CLI Command Reference*\.  
Type: String  
Required: Yes

Api  
The name of the API action that you want to run\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API action inputs  
One or more API action inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\. Use the following format to specify more than one input\.  

```
inputs:
  Service: The official namespace of the service
  Api: The API action name
  API input 1: A value
  API Input 2: A value
  API Input 3: A value
```

```
"inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API action name",
      "API input 1":"A value",
      "API Input 2":"A value",
      "API Input 3":"A value"
}
```
Type: Determined by chosen API action  
Required: Yes

PropertySelector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

DesiredValues  
The expected status or state on which to continue the Automation workflow\. If you specify a Boolean value, you must use a capital letter such as True or False\.  
Type: Varies  
Required: Yes