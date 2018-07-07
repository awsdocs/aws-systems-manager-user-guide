# Walkthrough: Manage Parameters Using Hierarchies \(AWS CLI\)<a name="sysman-paramstore-walk-hierarchies"></a>

This walkthrough shows you how to work with parameters and parameter hierarchies by using the AWS CLI\. For more information about parameter hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

**To manage parameters using hierarchies**

1. [Download](https://aws.amazon.com/cli/) the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to create a parameter that uses the `allowedPattern` parameter and the `String` data type\. The allowed pattern in this example means the value for the parameter must be between 1 and 4 digits long\.

   ```
   aws ssm put-parameter --name "/MyService/Test/MaxConnections" --value 100 --allowed-pattern "\d{1,4}" --type String
   ```

   The command returns the version number of the parameter\.

1. Execute the following command to *attempt* to overwrite the parameter you just created with a new value\.

   ```
   aws ssm put-parameter --name "/MyService/Test/MaxConnections" --value 10,000 --type String --overwrite
   ```

   The system throws the following error because the new value does not meet the requirements of the allowed pattern you specified in the previous step\.

   `An error occurred (ParameterPatternMismatchException) when calling the PutParameter operation: Parameter value, cannot be validated against allowedPattern: \d{1,4}`

1. Execute the following command to create a Secure String parameter that uses your default AWS KMS key\. The allowed pattern in this example means the user can specify any character, and the value must be between 8 and 20 characters\.

   ```
   aws ssm put-parameter --name "/MyService/Test/my-password" --value "p#sW*rd33" --allowed-pattern ".{8,20}" --type SecureString
   ```
**Important**  
Only the value of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\. Therefore, to make it less obvious which parameters contain passwords, we recommend using a naming system that avoids the actual word "password" in your parameter names\. For illustration, however, we are using the sample parameter name *my\-password* in our examples\.

1. Execute the following commands to create more parameters that use the hierarchy structure from the previous step\.

   ```
   aws ssm put-parameter --name "/MyService/Test/DBname" --value "SQLDevDb" --type String
   ```

   ```
   aws ssm put-parameter --name "/MyService/Test/user" --value "SA" --type String
   ```

   ```
   aws ssm put-parameter --name "/MyService/Test/userType" --value "SQLuser" --type String
   ```

1. Execute the following command to get the value of two parameters\.

   ```
   aws ssm get-parameters --names "/MyService/Test/user" "/MyService/Test/userType"
   ```

1. Execute the following command to query for all parameters within a single level\. 

   ```
   aws ssm get-parameters-by-path --path "/MyService/Test"
   ```

1. Execute the following command to delete two parameters

   ```
   aws ssm delete-parameters --names "/IADRegion/Dev/user" "/IADRegion/Dev/userType"
   ```