# Walkthrough: Manage parameters using hierarchies \(AWS CLI\)<a name="sysman-paramstore-walk-hierarchies"></a>

This walkthrough shows how to work with parameters and parameter hierarchies by using the AWS CLI\. For more information about parameter hierarchies, see [Organizing parameters into hierarchies](sysman-paramstore-su-organize.md)\.

**To manage parameters using hierarchies**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter that uses the `allowedPattern` parameter and the `String` parameter type\. The allowed pattern in this example means the value for the parameter must be between 1 and 4 digits long\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/MaxConnections" \
       --value 100 --allowed-pattern "\d{1,4}" \
       --type String
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/MaxConnections" ^
       --value 100 --allowed-pattern "\d{1,4}" ^
       --type String
   ```

------

   The command returns the version number of the parameter\.

1. Run the following command to *attempt* to overwrite the parameter you just created with a new value\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/MaxConnections" \
       --value 10,000 \
       --type String \
       --overwrite
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/MaxConnections" ^
       --value 10,000 ^
       --type String ^
       --overwrite
   ```

------

   The system returns the following error because the new value does not meet the requirements of the allowed pattern you specified in the previous step\.

   ```
   An error occurred (ParameterPatternMismatchException) when calling the PutParameter operation: Parameter value, cannot be validated against allowedPattern: \d{1,4}
   ```

1. Run the following command to create a `SecureString` parameter that uses an AWS\-managed customer master key \(CMK\)\. The allowed pattern in this example means the user can specify any character, and the value must be between 8 and 20 characters\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/my-password" \
       --value "p#sW*rd33" \
       --allowed-pattern ".{8,20}" \
       --type SecureString
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/my-password" ^
       --value "p#sW*rd33" ^
       --allowed-pattern ".{8,20}" ^
       --type SecureString
   ```

------

1. Run the following commands to create more parameters that use the hierarchy structure from the previous step\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/DBname" \
       --value "SQLDevDb" \
       --type String
   ```

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/user" \
       --value "SA" \
       --type String
   ```

   ```
   aws ssm put-parameter \
       --name "/MyService/Test/userType" \
       --value "SQLuser" \
       --type String
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/DBname" ^
       --value "SQLDevDb" ^
       --type String
   ```

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/user" ^
       --value "SA" ^
       --type String
   ```

   ```
   aws ssm put-parameter ^
       --name "/MyService/Test/userType" ^
       --value "SQLuser" ^
       --type String
   ```

------

1. Run the following command to get the value of two parameters\.

------
#### [ Linux ]

   ```
   aws ssm get-parameters \
       --names "/MyService/Test/user" "/MyService/Test/userType"
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameters ^
       --names "/MyService/Test/user" "/MyService/Test/userType"
   ```

------

1. Run the following command to query for all parameters within a single level\. 

------
#### [ Linux ]

   ```
   aws ssm get-parameters-by-path \
       --path "/MyService/Test"
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameters-by-path ^
       --path "/MyService/Test"
   ```

------

1. Run the following command to delete two parameters

------
#### [ Linux ]

   ```
   aws ssm delete-parameters \
       --names "/IADRegion/Dev/user" "/IADRegion/Dev/userType"
   ```

------
#### [ Windows ]

   ```
   aws ssm delete-parameters ^
       --names "/IADRegion/Dev/user" "/IADRegion/Dev/userType"
   ```

------