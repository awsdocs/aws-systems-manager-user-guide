# Optional Access Configurations<a name="sysman-create-iam"></a>

Task 1 in this section enabled you to grant access to a user by choosing a pre\-existing or *managed* IAM user policy\. If you want to limit user access to Systems Manager and SSM documents, you can create your own restrictive user policies, as described in this section\. For more information about how to create a custom policy, see [Creating a New Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.

The following example IAM policy allows a user to do the following\.
+ List Systems Manager documents and document versions\.
+ View details about documents\.
+ Send a command using the document specified in the policy\.

  The name of the document is determined by this entry:

  ```
  arn:aws:ssm:us-east-2:*:document/name_of_restrictive_document
  ```
+ Send a command to three instances\.

  The instances are determined by the following entries in the second `Resource` section:

  ```
  "arn:aws:ec2:us-east-2:*:instance/i-1234567890abcdef0",
  "arn:aws:ec2:us-east-2:*:instance/i-0598c7d356eba48d7",
  "arn:aws:ec2:us-east-2:*:instance/i-345678abcdef12345",
  ```
+ View details about a command after it has been sent\.
+ Start and stop Automation executions\.
+ Get information about Automation executions\.

If you want to give a user permission to use this document to send commands on any instance for which the user currently has access \(as determined by their AWS user account\), you could specify the following entry in the `Resource` section and remove the other instance entries\.

```
"arn:aws:ec2:us-east-2:*:instance/*"
```

Note that the `Resource` section includes an Amazon S3 ARN entry:

```
arn:aws:s3:::bucket_name
```

You can also format this entry as follows:

```
arn:aws:s3:::bucket_name/*

-or-

arn:aws:s3:::bucket_name/key_prefix_name
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:ListDocuments",
                "ssm:ListDocumentsVersions",
                "ssm:DescribeDocument",
                "ssm:GetDocument",
                "ssm:DescribeInstanceInformation",
                "ssm:DescribeDocumentParameters",
                "ssm:DescribeInstanceProperties"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ssm:SendCommand",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ec2:us-east-2:*:instance/i-1234567890abcdef0",
                "arn:aws:ec2:us-east-2:*:instance/i-0598c7d356eba48d7",
                "arn:aws:ec2:us-east-2:*:instance/i-345678abcdef12345",
                "arn:aws:s3:::bucket_name",
                "arn:aws:ssm:us-east-2:*:document/name_of_restrictive_document"
            ]
        },
        {
            "Action": [
                "ssm:CancelCommand",
                "ssm:ListCommands",
                "ssm:ListCommandInvocations"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ec2:DescribeInstanceStatus",
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ssm:StartAutomationExecution",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:::automation-definition/"
            ]
        },
        {
            "Action": "ssm:DescribeAutomationExecutions ",
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        },
        {
            "Action": [
                "ssm:StopAutomationExecution",
                "ssm:GetAutomationExecution"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:::automation-execution/"
            ]
        }
    ]
}
```