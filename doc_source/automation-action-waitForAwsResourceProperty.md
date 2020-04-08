# aws:waitForAwsResourceProperty – Wait on an AWS resource property<a name="automation-action-waitForAwsResourceProperty"></a>

The aws:waitForAwsResourceProperty action enables your Automation workflow to wait for a specific resource state or event state before continuing the workflow\. For more information and examples of how to use this action, see [Invoking other AWS services from a Systems Manager Automation workflow](automation-aws-apis-calling.md)\.

**Input**  
Inputs are defined by the API action that you choose\.

------
#### [ YAML ]

```
action: aws:waitForAwsResourceProperty
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
  PropertySelector: Response object
  DesiredValues:
  - Desired property value
```

------
#### [ JSON ]

```
{
  "action": "aws:waitForAwsResourceProperty",
  "inputs": {
    "Service":"The official namespace of the service",
    "Api":"The API action or method name",
    "API action inputs or parameters":"A value",
    "PropertySelector": "Response object",
    "DesiredValues": [
      "Desired property value"
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
The name of the API action that you want to run\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API action inputs  
One or more API action inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\.  

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
The expected status or state on which to continue the Automation workflow\.  
Type: Varies  
Required: Yes