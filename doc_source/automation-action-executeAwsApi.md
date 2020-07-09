# aws:executeAwsApi – Call and run AWS API actions<a name="automation-action-executeAwsApi"></a>

Calls and runs AWS API actions\. Most API actions are supported, although not all API actions have been tested\. For example, the following API actions are supported: [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html), [Delete bucket](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html), [RebootDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html), and [CreateGroups](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateGroup.html), to name a few\. Streaming API actions, such as the [Get Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) action, aren't supported\. For more information and examples of how to use this action, see [Invoking other AWS services from a Systems Manager Automation workflow](automation-aws-apis-calling.md)\.

**Input**  
Inputs are defined by the API action that you choose\. 

------
#### [ YAML ]

```
action: aws:executeAwsApi
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
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
      "Api":"The API action or method name",
      "API action inputs or parameters":"A value"
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
The AWS service namespace that contains the API action that you want to run\. You can view a list of supported AWS service namespaces in the [Available services](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) of the AWS SDK for Python \(Boto3\)\. The namespace can be found in the **Client** section\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon EC2 is `ec2`\.  
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

Name  
A name for the output\.  
Type: String  
Required: Yes

Selector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

Type  
The data type for the response element\.  
Type: Varies  
Required: Yes