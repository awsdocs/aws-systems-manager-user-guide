# Walkthrough: Create and Update a String Parameter \(AWS CLI\)<a name="sysman-paramstore-cli"></a>

The following procedure walks you through the process of creating and storing a parameter of the type `String` using the AWS CLI\.

**To create and use a String parameter in a command \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md)\.

1. Run the following command to create a parameter that uses the String data type\. The `--name` option supports hierarchies\. For information about hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value" --type String
   ```

   Here is an example that uses a parameter hierarchy in the name\. For more information about parameter hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "My1stParameter" --type String
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
               "LastModifiedUser": "arn:aws:iam::123456789012:user/User's name",
               "LastModifiedDate": 1494529763.156,
               "Type": "String",
               "Name": "helloworld"
           }
       ]
   }
   ```

1. Run the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "good day sunshine" --type String --overwrite
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
               "Type": "String",
               "Name": "/Test/IAD/helloWorld",
               "Value": "good day sunshine"
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