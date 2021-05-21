# Native parameter support for Amazon Machine Image IDs<a name="parameter-store-ec2-aliases"></a>

When you create a `String` parameter, you can specify the *data type* as `aws:ec2:image` to ensure that the parameter value you enter is a valid Amazon Machine Image \(AMI\) ID format\. 

Support for AMI ID formats lets you avoid updating all your scripts and templates with a new ID each time the AMI that you want to use in your processes changes\. You can create a parameter with the data type `aws:ec2:image`, and for its value, enter the ID of an AMI\. This is the AMI you want to create new instances from\. You then reference this parameter in your templates, commands, and scripts\. 

For example, you can specify the parameter that contains your preferred AMI ID when you run the Amazon Elastic Compute Cloud \(Amazon EC2\) `run-instances` command\.

**Note**  
The user who runs this command must have AWS Identity and Access Management \(IAM\) permissions that include the `ssm:GetParameters` API operation in order for the parameter value to be validated\. Otherwise, the parameter creation process fails\.

```
aws ec2 run-instances \
    --image-id resolve:ssm:/golden-ami \
    --count 1 \
    --instance-type t2.micro \
    --key-name my-key-pair \
    --security-groups my-security-group
```

You can also choose your preferred AMI when you create an instance using the Amazon EC2 console\. For more information, see [Using a Systems Manager parameter to find an AMI](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/finding-an-ami.html#using-systems-manager-parameter-to-find-AMI) in the *Amazon EC2 User Guide for Windows Instances*\.

When it's time to use a different AMI in your instance creation workflow, you need only update the parameter with the new AMI value, and Parameter Store again validates that you have entered an ID in the proper format\.

## Grant permissions to create a parameter of aws:ec2:image data type<a name="parameter-store-ec2-iam"></a>

Using AWS Identity and Access Management \(IAM\) policies, you can provide or restrict user access to Parameter Store API actions and content\.

The following example policy grants users permission to call the `PutParameter` API operation for `aws:ec2:image`\. This means that the user can add a parameter of data type `aws:ec2:image` to the system\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "ssm:PutParameter",
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "ec2:DescribeImages",
            "Resource": "*"
        }
    ]
}
```

## How AMI format validation works<a name="parameter-ami-validation"></a>

When you specify `aws:ec2:image` as the data type for a parameter, Systems Manager doesn't create the parameter immediately\. It instead performs an asynchronous validation operation to ensure that the parameter value meets the formatting requirements for an AMI ID, and that the specified AMI is available in your Amazon Web Services account\.

A parameter version number might be generated before the validation operation is complete\. The operation may not be complete even if a parameter version number is generated\. 

To monitor whether your parameters are created successfully, we recommend using Amazon EventBridge \(EventBridge\) to send you notifications about your create and update parameter operations\. These notifications report whether a parameter operation was successful or not\. If an operation fails, the notification includes an error message that indicates the reason for the failure\. 

```
{
    "version": "0",
    "id": "eed4a719-0fa4-6a49-80d8-8ac65EXAMPLE",
    "detail-type": "Parameter Store Change",
    "source": "aws.ssm",
    "account": "111122223333",
    "time": "2020-05-26T22:04:42Z",
    "region": "us-east-2",
    "resources": [
        "arn:aws:ssm:us-east-2:111122223333:parameter/golden-ami"
    ],
    "detail": {
        "exception": "Unable to Describe Resource",
        "dataType": "aws:ec2:image",
        "name": "golden-ami",
        "type": "String",
        "operation": "Create"
    }
}
```

For information about subscribing to Parameter Store events in EventBridge, see [Setting up notifications or trigger actions based on Parameter Store events](sysman-paramstore-cwe.md)\.