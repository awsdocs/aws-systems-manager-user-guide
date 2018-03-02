# AWS Systems Manager Automation<a name="systems-manager-automation"></a>

Systems Manager Automation is an AWS\-hosted service that simplifies common instance and system maintenance and deployment tasks\. For example, you can use Automation as part of your change management process to keep your Amazon Machine Images \(AMIs\) up\-to\-date with the latest application build\. Or, let’s say you want to create a backup of a database and upload it nightly to Amazon S3\. With Automation, you can avoid deploying scripts and scheduling logic directly to the instance\. Instead, you can run maintenance activities through Systems Manager Run Command and AWS Lambda steps orchestrated by the Automation service\.

Automation enables you to do the following\.

+ Pre\-install and configure applications and agents in your Amazon Machine Images \(AMIs\) using a streamlined and repeatable process that you can audit\.

+ Build workflows to configure and manage instances and AWS resources\.

+ Create your own custom workflows, or use pre\-defined workflows maintained by AWS\.

+ Receive notifications about Automation tasks and workflows by using Amazon CloudWatch Events

+ Monitor Automation progress and execution details by using the Amazon EC2 or the AWS Systems Manager console\. 


+ [AWS Systems Manager Automation Concepts](#automation-concepts)
+ [Automation Use Cases](#automation-use-cases)
+ [Automation QuickStart](automation-quickstart.md)
+ [Setting Up Automation](automation-setup.md)
+ [Systems Manager Automation Walkthroughs](automation-walk.md)
+ [Working with Automation Documents](automation-documents.md)
+ [Systems Manager Automation Examples](automation-examples.md)
+ [Automation System Variables](automation-variables.md)
+ [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)

## AWS Systems Manager Automation Concepts<a name="automation-concepts"></a>

AWS Systems Manager Automation uses the following concepts\.


****  

| Concept | Details | 
| --- | --- | 
|  Automation document  |  A Systems Manager Automation document defines the Automation workflow \(the actions that Systems Manager performs on your managed instances and AWS resources\)\. Automation includes several pre\-defined Automation documents that you can use to perform common tasks like restarting one or more Amazon EC2 instances or creating an Amazon Machine Image \(AMI\)\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. Steps execute in sequential order\. For more information, see [Working with Automation Documents](automation-documents.md)\.  | 
|  Automation action  |  The Automation workflow defined in an Automation document includes one or more steps\. Each step is associated with a particular action or plugin\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\. For more information, see the [Systems Manager Automation Document Reference](automation-actions.md)\.  | 
|  Automation queue  |  Each AWS account can execute 25 Automations simultaneously\. If you attempt to execute more than this, Systems Manager adds the additional executions to a queue and displays a status of Pending\. When an Automation completes \(or reaches a terminal state\), the first execution in the queue starts\. Each AWS account can queue 75 Automation executions\.  | 

## Automation Use Cases<a name="automation-use-cases"></a>

This section includes common uses cases for AWS Systems Manager Automation\.

**Perform common IT tasks**  
Automation can simplify common IT tasks such as changing the state of one or more instances \(using an approval workflow\) and managing instance states according to a schedule\. Here are some examples:

+ Use the AWS\-StopInstance document to request that one or more AWS Identity and Access Management \(IAM\) users approve the instance stop action\. After the approval is received, Automation stops the instance\.

+ Use the AWS\-StopInstance document to automatically stop instances on a schedule by using Amazon CloudWatch Events or by using a Maintenance Window task\. For example, you can configure an Automation workflow to stop instances every Friday evening, and then restart them every Monday morning\.

+ Use the AWS\-UpdateCloudFormationStackWithApproval document to update resources that were deployed by using CloudFormation template\. The update applies a new template\. You can configure the Automation to request approval by one or more IAM users before the update begins\.

**Safely perform disruptive tasks in bulk**  
Systems Manager includes features that help you target large groups of instances by using EC2 tags, and velocity controls that help you roll out changes according to the limits you define\.

Use the AWS\-RestartInstanceWithApproval document to target an AWS Resource Group that includes multiple instances\. You can configure the Automation workflow to use velocity controls\. For example, you can specify the number of instances that should be restarted concurrently\. You can also specify a maximum number of errors that are allowed before the Automation workflow is cancelled\.

**Simplify complex tasks**  
Automation offers one\-click automations for simplifying complex tasks such as creating golden Amazon Machine Images \(AMIs\), and recovering unreachable EC2 instances\. Here are some examples:

+ Use the AWS\-UpdateLinuxAMI and AWS\-UpdateWindowsAMI documents to create golden AMIs from a source AMI\. You can run custom scripts before and after updates are applied\. You can also include or exclude specific packages from being installed\.

+ Use the AWSSupport\-ExecuteEC2Rescue document to recover impaired instances\. An instance can become unreachable for a variety of reasons, including network misconfigurations, RDP issues, or firewall settings\. Troubleshooting and regaining access to the instance previously required dozens of manual steps before you could regain access\. The AWSSupport\-ExecuteEC2Rescue document lets you regain access by specifying an instance ID and clicking a button\.

**Enhance operations security**  
Using delegated administration, you can restrict or elevate user permissions for various types of tasks\. 

Delegated administration enables you to provide permissions for certain tasks on certain resource without having to give a user direct permission to access the resources\. This improves your overall security profile\. For example, assume that User1 doesn’t have permissions to restart EC2 instances, but you would like to authorize the user to do so\. Instead of allowing User1 direct permissions, you can: 

+ Create an IAM role with the permissions required to successfully stop and start EC2 instances\.

+ Create an Automation document and embed the role in the document\. \(The easiest way to do this is to customize the AWS\-RestartEC2Instance document and embed the role in the document instead of assigning an Automation service role \[or *assume* role\]\)\.

+ Modify IAM permissions for User1 and allow the user permission to execute the document\. Here is an example of how you would alter the document: 

  ```
  "Version": "2012-10-17",
      "Statement": [
          {
              "Action": "ssm:StartAutomationExecution",
              "Effect": "Allow",
              "Resource": [
                  "ARN of the document you created in Step 2”
              ]
          }
      ]
  }
  ```

  User1 can now execute this document and restart instances\.

**Share best practices**  
Automation lets you share best practices with rest of your organization\.

You can create best practices for resource management in Automation documents and easily share the documents across AWS Regions and groups\. You can also constrain the allowed values for the parameters the document accepts\. Here is an example of an Automation document that launches new EC2 instances and constrains users to specify only certain AMI IDs::

**Note**  
Replace the AMI value, and the value of `MySecurityKeyName` and `IamInstanceProfileName`, as needed\.

```
{
   "description":"Launch Allowed EC2 instances",
   "schemaVersion":"0.3",
   "assumeRole": "{{ AutomationAssumeRole }}",
   "parameters":{
      "AMIID":{
         "type":"String",
         "description":"Instance to value",
         "allowedValues" : [ "ami-e3bb7399","ami-55ef662f" ]
      },
    "AutomationAssumeRole": {
      "type": "String",
      "description": "(Optional) The ARN of the role that allows Automation to perform the actions on your behalf.",
      "default": ""
    }
   },
   "mainSteps":[
      {
         "name":"launchInstances",
         "action":"aws:runInstances",
         "timeoutSeconds":1200,
         "maxAttempts":1,
         "onFailure":"Abort",
         "inputs":{
            "ImageId":"{{ AMIID }}",
            "InstanceType":"t2.micro",
            "KeyName":"MySecurityKeyName",
            "MinInstanceCount":1,
            "MaxInstanceCount":1,
            "IamInstanceProfileName":"ManagedInstanceProfile"
         }
      }
   ]
}
```

This document ensures that users are only able to launch t2\.micro instances from the specified AMI IDs\.