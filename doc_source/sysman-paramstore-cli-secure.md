# Walkthrough: Create and update a SecureString parameter \(AWS CLI\)<a name="sysman-paramstore-cli-secure"></a>

Use the following procedure to create a `SecureString` parameter\. For more information about `SecureString` parameters, see [SecureString parameters](sysman-paramstore-securestring.md)\.

**To create a SecureString parameter using the AWS CLI**

1. Run *one* of the following commands to create a parameter that uses the `SecureString` datatype\.

------
#### [ Linux ]

   **Create a `SecureString` parameter that uses a customer managed customer master key \(CMK\)**

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "a-parameter-value, for example P@ssW%rd#1" \
       --type "SecureString"
   ```

   **Create a `SecureString` parameter that uses a custom AWS KMS key**

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "a-parameter-value, for example P@ssW%rd#1" \
       --type "SecureString" \
       --key-id "your-AWS-user-account-ID/the-custom-AWS KMS-key"
   ```

------
#### [ Windows ]

   **Create a `SecureString` parameter that uses a customer managed customer master key \(CMK\)**

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "a-parameter-value, for example P@ssW%rd#1" ^
       --type "SecureString"
   ```

   **Create a `SecureString` parameter that uses a custom AWS KMS key**

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "a-parameter-value, for example P@ssW%rd#1" ^
       --type "SecureString" ^
       --key-id "your-AWS-user-account-ID/the-custom-AWS KMS-key"
   ```

------

   Here is an example that uses a customer managed CMK\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "my-password" \
       --value "P@ssW%rd#1" \
       --type "SecureString" \
       --key-id "arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2EXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "my-password" ^
       --value "P@ssW%rd#1" ^
       --type "SecureString" ^
       --key-id "arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2EXAMPLE"
   ```

------

1. Run the following command to view the parameter metadata\.

------
#### [ Linux ]

   ```
   aws ssm describe-parameters \
       --filters "Key=Name,Values=the-name-that-you-specified"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-parameters ^
       --filters "Key=Name,Values=the-name-that-you-specified"
   ```

------

1. Run the following command to change the parameter value if you are *not* using a customer managed customer master key \(CMK\)\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "the-name-that-you-specified" \
       --value "a-new-parameter-value" \
       --type "SecureString" \
       --overwrite
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "the-name-that-you-specified" ^
       --value "a-new-parameter-value" ^
       --type "SecureString" ^
       --overwrite
   ```

------

   \-or\-

   Run one the following commands to change the parameter value if you *are* using a customer managed customer master key \(CMK\)\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "the-name-that-you-specified" \
       --value "a-new-parameter-value" \
       --type "SecureString" \
       --key-id "the-CMK-ID" \
       --overwrite
   ```

   ```
   aws ssm put-parameter \
       --name "the-name-that-you-specified" \
       --value "a-new-parameter-value" \
       --type "SecureString" \
       --key-id "your-AWS-user-account-alias/the-CMK-ID" \
       --overwrite
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "the-name-that-you-specified" ^
       --value "a-new-parameter-value" ^
       --type "SecureString" ^
       --key-id "the-CMK-ID" ^
       --overwrite
   ```

   ```
   aws ssm put-parameter ^
       --name "the-name-that-you-specified" ^
       --value "a-new-parameter-value" ^
       --type "SecureString" ^
       --key-id "your-AWS-user-account-alias/the-CMK-ID" ^
       --overwrite
   ```

------

1. Run the following command to view the latest parameter value\.

------
#### [ Linux ]

   ```
   aws ssm get-parameters \
       --name "the-name-that-you-specified" \
       --with-decryption
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameters ^
       --name "the-name-that-you-specified" ^
       --with-decryption
   ```

------

1. Run the following command to view the parameter value history\.

------
#### [ Linux ]

   ```
   aws ssm get-parameter-history \
       --name "the-name-that-you-specified"
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameter-history ^
       --name "the-name-that-you-specified"
   ```

------

**Important**  
Only the *value* of a `SecureString` parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.