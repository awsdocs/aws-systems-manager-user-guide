# Walkthrough: Create and update a String parameter \(AWS CLI\)<a name="sysman-paramstore-cli"></a>

The following procedure walks you through the process of creating and storing a parameter of the type `String` using the AWS CLI\.

**To create and use a String parameter in a command \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a String\-type parameter\. The `--name` option supports hierarchies\. For information about hierarchies, see [Organizing parameters into hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "parameter-name" --type String --value "parameter-value"
   ```

   Here is an example that specifies an Amazon Machine Image \(AMI\) as the data type:

   ```
   aws ssm put-parameter --name "golden-ami" --type String --data-type "aws:ec2:image" --value ami-0dbf5ea29aEXAMPLE
   ```

   The option `--data-type` applies to String\-type parameters only and is required only if you are specifying `aws:ec2:image` as the data type\. You do not need to explicitly specify a data type if the String data type is `text`\.

   Here is an example that uses a parameter hierarchy in the name\. For more information about parameter hierarchies, see [Organizing parameters into hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "My 1st parameter" --type String
   ```

   The command returns the version number of the parameter\.

1. Run the following command to view the parameter metadata\.

   ```
   aws ssm describe-parameters --filters "Key=Name,Values=/Test/IAD/helloWorld"
   ```
**Note**  
*Name* must be capitalized\.

   The system returns information like the following\.

   ```
   {
       "Parameters": [
           {
               "Name": "helloworld",
               "Type": "String",
               "LastModifiedUser": "arn:aws:iam::123456789012:user/user-name",
               "LastModifiedDate": 1494529763.156,
               "Version": 1,
               "Tier": "Standard",
               "Policies": []           
           }
       ]
   }
   ```

1. Run the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "My updated 1st parameter" --type String --overwrite
   ```

   The command returns the version number of the parameter\.

1. Run the following command to view the latest parameter value\.

   ```
   aws ssm get-parameters --names "/Test/IAD/helloWorld"
   ```

   The system returns information like the following\.

   ```
   {
       "InvalidParameters": [],
       "Parameters": [
           {            
               "Name": "/Test/IAD/helloWorld",
               "Type": "String",
               "Value": "My updated parameter value",
               "Version": 2,
               "LastModifiedDate": "2020-02-25T15:55:33.677000-08:00",
               "ARN": "arn:aws:ssm:us-east-2:123456789012:parameter/Test/IAD/helloWorld"
               
           }
       ]
   }
   ```

1. Run the following command to view the parameter value history\.

   ```
   aws ssm get-parameter-history --name "/Test/IAD/helloWorld"
   ```

1. Run the following command to use this parameter in a command\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --parameters '{"commands":["echo {{ssm:/Test/IAD/helloWorld}}"]}' --targets "Key=instanceids,Values=instance-ids"
   ```