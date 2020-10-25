# Referencing AWS Secrets Manager secrets from Parameter Store parameters<a name="integration-ps-secretsmanager"></a>

Secrets Manager helps you organize and manage important configuration data such as credentials, passwords, and license keys\. Parameter Store is integrated with Secrets Manager so that you can retrieve Secrets Manager secrets when using other AWS services that already support references to Parameter Store parameters\. These services include Amazon EC2, Amazon Elastic Container Service, AWS Lambda, AWS CloudFormation, AWS CodeBuild, AWS CodeDeploy, and other Systems Manager capabilities\. By using Parameter Store to reference Secrets Manager secrets, you create a consistent and secure process for calling and using secrets and reference data in your code and configuration scripts\. 

For more information about Secrets Manager, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

**Important**  
Parameter Store functions as a pass\-through service for references to Secrets Manager secrets\. Parameter Store doesn't retain data or metadata about secrets\. The reference is stateless\.

## Restrictions<a name="integration-ps-secretsmanager-restrictions"></a>

Note the following restrictions when using Parameter Store to reference Secrets Manager secrets:
+ You can only retrieve Secrets Manager secrets by using the [GetParameter](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameter.html) and [GetParameters](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameters.html) API actions\. Modification operations and advance querying API actions, such as [DescribeParameters](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeParameters.html) and [GetParametersByPath](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParametersByPath.html), are not supported for Secrets Manager\. 
+ You can use the AWS CLI, AWS Tools for Windows PowerShell, and the SDKs to retrieve a secret by using Parameter Store\.
+ When you retrieve a Secrets Manager secret from Parameter Store, the parameter name must begin with the following reserved path: /aws/reference/secretsmanager/*secret\_ID\_in\_Secrets\_Manager*\.

  Here is an example: /aws/reference/secretsmanager/CFCreds1
+ Parameter Store honors IAM policies attached to Secrets Manager secrets\. For example, if User 1 doesn't have access to Secret A, then User 1 can't retrieve Secret A by using Parameter Store\.
+ Parameters that reference Secrets Manager secrets can't use the Parameter Store versioning or history features\.
+ Parameter Store honors Secrets Manager version stages\. If you reference a version stage, it can only use letters, numbers, a period \(\.\), a hyphen \(\-\), or an underscore \(\_\)\. All other symbols specified in the version stage cause the reference to fail\.

## How to reference a Secrets Manager secret by using Parameter Store<a name="integration-ps-secretsmanager-create"></a>

The following procedure describes how to reference a Secrets Manager secret by using Parameter Store APIs\. The procedure references other procedures in the *AWS Secrets Manager User Guide*\.

**Note**  
Before you begin, verify that you have permission to reference Secrets Manager secrets in Parameter Store parameters\. If you have administrator privileges in Secrets Manager and Systems Manager, then you can reference or retrieve secrets by using Parameter Store APIs\. If you reference a Secrets Manager secret in a Parameter Store parameter, and you don't have permission to access that secret, then the reference fails\. For more information, see [Authentication and access control for AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/auth-and-access.html) in the *AWS Secrets Manager User Guide*\.

**To reference a Secrets Manager secret by using Parameter Store**

1. Create a secret in Secrets Manager\. For more information, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html)\.

1. Reference a secret by using the AWS CLI, AWS Tools for Windows PowerShell, or the SDK\. When you reference a Secrets Manager secret, the parameter name must begin with the following reserved path: /aws/reference/secretsmanager/\. By specifying this path, Systems Manager knows to retrieve the secret from Secrets Manager instead of Parameter Store\. Here are some example parameters that correctly reference Secrets Manager secrets:
   + /aws/reference/secretsmanager/CFCreds1
   + /aws/reference/secretsmanager/DBPass

   Here is a Java code example that references an access key and a secret key that are stored in Secrets Manager\. This code example sets up an Amazon DynamoDB client\. The code retrieves configuration data and credentials from Parameter Store\. The configuration data is stored as a string parameter in Parameter Store and the credentials are stored in Secrets Manager\. Even though the configuration data and credentials are stored in separate services, both sets of data can be accessed from Parameter Store by using the `GetParameter` API\.

   ```
   /**
   * Initialize AWS System Manager Client with default credentials
   */
   AWSSimpleSystemsManagement ssm = AWSSimpleSystemsManagementClientBuilder.defaultClient();
    
   ...
    
   /**
   * Example method to launch DynamoDB client with credentials different from default
   * @return DynamoDB client
   */
   AmazonDynamoDB getDynamoDbClient() {
       //Getting AWS credentials from Secrets manager using GetParameter
       BasicAWSCredentials differentAWSCreds = new BasicAWSCredentials(
               getParameter("/aws/reference/secretsmanager/access-key"),
               getParameter("/aws/reference/secretsmanager/secret-key"));
    
       //Initialize the DDB Client with different credentials
       final AmazonDynamoDB client = AmazonDynamoDBClient.builder()
               .withCredentials(new AWSStaticCredentialsProvider(differentAWSCreds))
               .withRegion(getParameter("region")) //Getting config from Parameter Store
               .build();
       return client;
   }
    
   /**
   * Helper method to retrieve SSM Parameter's value
   * @param parameterName identifier of the SSM Parameter
   * @return decrypted parameter value
   */
   public GetParameterResult getParameter(String parameterName) {
       GetParameterRequest request = new GetParameterRequest();
       request.setName(parameterName);
       request.setWithDecryption(true);
       return ssm.newGetParameterCall().call(request).getParameter().getValue();
   }
   ```

   Here are some AWS CLI examples\.

   **AWS CLI Example 1: Reference by using the name of the secret**

------
#### [ Linux ]

   ```
   aws ssm get-parameter \
       --name /aws/reference/secretsmanager/s1-secret \
       --with-decryption
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameter ^
       --name /aws/reference/secretsmanager/s1-secret ^
       --with-decryption
   ```

------

   The command returns information like the following\.

   ```
   {
       "Parameter": {
           "Name": "/aws/reference/secretsmanager/s1-secret",
           "Value": "Fl*MEishm!al875",
           "Type": "SecureString",
           "LastModifiedDate": 2018-05-14T21:47:14.743Z,
           "ARN": "arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-
                  E18LRP",
           "SourceResult": 
                 "{
                      \"CreatedDate\": 1526334434.743,
                      \"Name\": \"s1-secret\",
                      \"VersionId\": \"aaabbbccc-1111-222-333-123456789\",
                      \"SecretString\": \"Fl*MEishm!al875\",
                      \"VersionStages\": [\"AWSCURRENT\"],
                      \"ARN\": \"arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-E18LRP\"
                  }"
         }
   }
   ```

   **AWS CLI Example 2: Reference that includes the version ID**

------
#### [ Linux ]

   ```
   aws ssm get-parameter \
       --name /aws/reference/secretsmanager/s1-secret:11111-aaa-bbb-ccc-123456789 \
       --with-decryption
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameter ^
       --name /aws/reference/secretsmanager/s1-secret:11111-aaa-bbb-ccc-123456789 ^
       --with-decryption
   ```

------

   The command returns information like the following\.

   ```
   {
       "Parameter": {
           "Name": "/aws/reference/secretsmanager/s1-secret",
           "Value": "Fl*MEishm!al875",
           "Type": "SecureString",
           "LastModifiedDate": 2018-05-14T21:47:14.743Z,
           "ARN": "arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-
                  E18LRP",
           "SourceResult": 
                 "{
                      \"CreatedDate\": 1526334434.743,
                      \"Name\": \"s1-secret\",
                      \"VersionId\": \"11111-aaa-bbb-ccc-123456789\",
                      \"SecretString\": \"Fl*MEishm!al875\",
                      \"VersionStages\": [\"AWSCURRENT\"],
                      \"ARN\": \"arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-E18LRP\"
                  }"
           "Selector": ":11111-aaa-bbb-ccc-123456789"
         }
   }
   ```

   **AWS CLI Example 3: Reference that includes the version stage**

------
#### [ Linux ]

   ```
   aws ssm get-parameter \
       --name /aws/reference/secretsmanager/s1-secret:AWSCURRENT \
       --with-decryption
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameter ^
       --name /aws/reference/secretsmanager/s1-secret:AWSCURRENT ^
       --with-decryption
   ```

------

   The command returns information like the following\.

   ```
   {
       "Parameter": {
           "Name": "/aws/reference/secretsmanager/s1-secret",
           "Value": "Fl*MEishm!al875",
           "Type": "SecureString",
           "LastModifiedDate": 2018-05-14T21:47:14.743Z,
           "ARN": "arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-
                   E18LRP",
           "SourceResult": 
                 "{
                      \"CreatedDate\": 1526334434.743,
                      \"Name\": \"s1-secret\",
                      \"VersionId\": \"11111-aaa-bbb-ccc-123456789\",
                      \"SecretString\": \"Fl*MEishm!al875\",
                      \"VersionStages\": [\"AWSCURRENT\"],
                      \"ARN\": \"arn:aws:secretsmanager:us-east-2:123456789012:secret:s1-secret-E18LRP\"
                  }"
           "Selector": ":AWSCURRENT"
         }
   }
   ```