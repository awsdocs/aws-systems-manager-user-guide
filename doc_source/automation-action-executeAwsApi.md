# `aws:executeAwsApi` – Call and run AWS API operations<a name="automation-action-executeAwsApi"></a>

Calls and runs AWS API operations\. Most API operations are supported, although not all API operations have been tested\. Streaming API operations, such as the [GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) operation, aren't supported\. If you're not sure if an API operation you want to use is a streaming operation, review the [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) documentation for the service to determine if an API requires streaming inputs\. Each `aws:executeAwsApi` action can run up to a maximum duration of 25 seconds\. For more information and examples of how to use this action, see [Invoking other AWS services from a Systems Manager Automation runbook](automation-aws-apis-calling.md)\.

**Inputs**  
Inputs are defined by the API operation that you choose\. 

------
#### [ YAML ]

```
action: aws:executeAwsApi
inputs:
  Service: The official namespace of the service
  Api: The API operation or method name
  API operation inputs or parameters: A value
outputs: # These are user-specified outputs
- Name: The name for a user-specified output key
  Selector: A response object specified by using jsonpath format
  Type: The data type
```

------
#### [ JSON ]

```
{
   "action":"aws:executeAwsApi",
   "inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API operation or method name",
      "API operation inputs or parameters":"A value"
   },
   "outputs":[ These are user-specified outputs
      {
         "Name":"The name for a user-specified output key",
         "Selector":"A response object specified by using JSONPath format",
         "Type":"The data type"
      }
   ]
}
```

------

Service  
The AWS service namespace that contains the API operation that you want to run\. You can view a list of supported AWS service namespaces in [Available services](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) of the AWS SDK for Python \(Boto3\)\. The namespace can be found in the **Client** section\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon Elastic Compute Cloud \(Amazon EC2\) is `ec2`\.  
Type: String  
Required: Yes

Api  
The name of the API operation that you want to run\. You can view the API operations \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API operations \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API operation inputs  
One or more API operation inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\.  

```
inputs:
  Service: The official namespace of the service
  Api: The API operation name
  API input 1: A value
  API Input 2: A value
  API Input 3: A value
```

```
"inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API operation name",
      "API input 1":"A value",
      "API Input 2":"A value",
      "API Input 3":"A value"
}
```
Type: Determined by chosen API operation  
Required: Yes

**Outputs**  
Outputs are specified by the user based on the response from the chosen API operation\.

Name  
A name for the output\.  
Type: String  
Required: Yes

Selector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

Type  
The data type for the response element\.  
Type: Varies  
Required: Yes