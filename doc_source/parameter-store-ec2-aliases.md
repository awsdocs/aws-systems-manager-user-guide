# Native parameter support for Amazon Machine Image IDs<a name="parameter-store-ec2-aliases"></a>

When you create a String parameter, you can specify the *data type* as `aws:ec2:image` to ensure that the parameter value you enter is a valid Amazon Machine Image \(AMI\) ID format\. 

Support for AMI ID formats lets you avoid updating all your scripts and templates with a new ID each time the AMI that you want to use in your processes changes\. You can create a parameter with the data type `aws:ec2:image`, and for its value, enter the ID of an AMI\. This is the AMI from which you currently want new instances to be created\. You then reference this parameter in your templates, commands, and scripts\. 

For example, you can specify the parameter that contains your preferred AMI ID when you run the Amazon EC2 `run-instances` command\.

**Note**  
The user who runs this command must have IAM permissions that include the `ssm:GetParameters` API action in order for the parameter value to be validated\. Otherwise, the parameter creation process fails\.

```
aws ec2 run-instances \
    --image-id resolve:ssm:/golden-ami \
    --count 1 \
    --instance-type t2.micro \
    --key-name my-key-pair \
    --security-groups my-security-group
```

You can also choose the Systems Manager containing your preferred AMI when you create an instance using the Amazon EC2 console\. For more information, see [Using a Systems Manager parameter to find an AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html#using-systems-manager-parameter-to-find-AMI) in the *Amazon EC2 User Guide for Linux Instances*\.

When it's time to use a different AMI in your instance creation workflow, you need only update the parameter with the new AMI value, and Parameter Store again validates that you have entered an ID in the proper format\.

## How AMI format validation works<a name="parameter-ami-validation"></a>

When you specify `aws:ec2:image` as the data type for a parameter, Systems Manager does not create the parameter immediately\. It instead performs an asynchronous validation operation to ensure that the parameter value meets the formatting requirements for an AMI ID, and that the specified AMI is available in your AWS account\.

It is important to note that a parameter version number may be generated before the validation operation is complete\. That is, a parameter version number being generated alone isn't an indication that the operation has completed successfully\.

To monitor whether your parameters are created successfully, we recommend using Amazon EventBridge to send you notifications about your create and update parameter operations\. These notifications report whether a parameter operation was successful or not\. If an operation fails, the notification includes an error message that indicates the reason for the failure\. 

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

## Troubleshooting `aws:ec2:image` parameter creation<a name="ps-ec2-aliases-troubleshooting"></a>

Use the following information to help troubleshoot problems with creating `aws:ec2:image` data type parameters\.

**Topics**
+ [EventBridge reports the failure message "Unable to Describe Resource"](#ps-ec2-aliases-1)
+ [New `aws:ec2:image` parameter isn't available](#ps-ec2-aliases-2)

### EventBridge reports the failure message "Unable to Describe Resource"<a name="ps-ec2-aliases-1"></a>

**Problem**: You ran a command to create an `aws:ec2:image` parameter, but parameter creation failed\. You receive a notification from EventBridge that reports the exception "Unable to Describe Resource"\. 

**Solution**: This message can indicate the following: 
+ You haven't been granted the IAM permission for the `ec2:DescribeImages` API action, or you lack permission to access the specific image referenced in the parameter\. Contact an IAM user with administrator permissions in your organization to request the necessary permissions\.
+ The AMI ID you entered as a parameter value is not valid\. Make sure you are entering the ID of an AMI that is available in the current AWS Region and account you are working in\.

### New `aws:ec2:image` parameter isn't available<a name="ps-ec2-aliases-2"></a>

**Problem**: You just ran a command to create an `aws:ec2:image` parameter and a version number was reported, but the parameter isn't available\.
+ **Solution**: When you run the command to create a parameter that uses the `aws:ec2:image` data type, a version number is generated for the parameter right away, but the parameter format must be validated before the parameter is available\. This process can take up to a few minutes\. To monitor the parameter creation and validation process, you can do the following:
  + Use Amazon EventBridge to send you notifications about your create and update parameter operations\. These notifications report whether a parameter operation was successful or not\. For information about subscribing to Parameter Store events in EventBridge, see [Setting up notifications or trigger actions based on Parameter Store events](sysman-paramstore-cwe.md)\.
  + In the Parameter Store section of the Systems Manager console, refresh the list of parameters periodically to check for the new or updated parameter details\.
  + Use the GetParameter command to check for the new or updated parameter\. For example, using the AWS CLI:

    ```
    aws ssm get-parameter --name MyParameter
    ```

    For a new parameter, a `ParameterNotFound` message is returned until the parameter is validated\. For an existing parameter that you are updating, information about the new version isn't included until the parameter is validated\.

  If you attempt to create or update the parameter again before the validation process completes, the system reports that validation is still in process\. If the parameter isn't created or updated successfully, you can try again after five minutes have passed from the original attempt\. 