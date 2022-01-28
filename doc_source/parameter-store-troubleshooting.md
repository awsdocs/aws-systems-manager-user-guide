# Troubleshooting Parameter Store<a name="parameter-store-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Parameter Store, a capability of AWS Systems Manager\.

## Troubleshooting `aws:ec2:image` parameter creation<a name="ps-ec2-aliases-troubleshooting"></a>

Use the following information to help troubleshoot problems with creating `aws:ec2:image` data type parameters\.

### EventBridge reports the failure message "Unable to Describe Resource"<a name="ps-ec2-aliases-1"></a>

**Problem**: You ran a command to create an `aws:ec2:image` parameter, but parameter creation failed\. You receive a notification from Amazon EventBridge that reports the exception "Unable to Describe Resource"\. 

**Solution**: This message can indicate the following: 
+ You don't have the AWS Identity and Access Management \(IAM\) permission for the `ec2:DescribeImages` API operation, or you lack permission to access the specific image referenced in the parameter\. Contact an IAM user with administrator permissions in your organization to request the necessary permissions\.
+ The Amazon Machine Image \(AMI\) ID you entered as a parameter value isn't valid\. Make sure you're entering the ID of an AMI that is available in the current AWS Region and account you're working in\.

### New `aws:ec2:image` parameter isn't available<a name="ps-ec2-aliases-2"></a>

**Problem**: You just ran a command to create an `aws:ec2:image` parameter and a version number was reported, but the parameter isn't available\.
+ **Solution**: When you run the command to create a parameter that uses the `aws:ec2:image` data type, a version number is generated for the parameter right away, but the parameter format must be validated before the parameter is available\. This process can take up to a few minutes\. To monitor the parameter creation and validation process, you can do the following:
  + Use EventBridge to send you notifications about your `create` and `update` parameter operations\. These notifications report whether a parameter operation was successful or not\. For information about subscribing to Parameter Store events in EventBridge, see [Setting up notifications or trigger actions based on Parameter Store events](sysman-paramstore-cwe.md)\.
  + In the Parameter Store section of the Systems Manager console, refresh the list of parameters periodically to search for the new or updated parameter details\.
  + Use the GetParameter command to check for the new or updated parameter\. For example, using the AWS Command Line Interface \(AWS CLI\):

    ```
    aws ssm get-parameter --name MyParameter
    ```

    For a new parameter, a `ParameterNotFound` message is returned until the parameter is validated\. For an existing parameter that you're updating, information about the new version isn't included until the parameter is validated\.

  If you attempt to create or update the parameter again before the validation process is complete, the system reports that validation is still in process\. If the parameter isn't created or updated, you can try again after 5 minutes have passed from the original attempt\. 