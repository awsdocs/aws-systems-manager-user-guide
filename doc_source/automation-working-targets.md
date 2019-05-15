# About Targets<a name="automation-working-targets"></a>

The `Targets` parameter enables you to quickly define which resources in your fleet can run an Automation workflow\. For example, if you want to run an Automation that restarts your managed instances, then instead of manually selecting dozens of instance IDs in the console or typing them in a command, you can target instances by specifying Amazon EC2 tags with the `Targets` parameter\.

When you run an Automation that uses a target, Systems Manager creates a child Automation for each target\. For example, if you target 200 Amazon Elastic Block Store \(Amazon EBS\) volumes with an Automation, Systems Manager creates 200 child Automation workflows\. The parent Automation is complete when all child Automations reach a final state\.

**Note**  
Any `input parameters` that you specify at runtime \(either in the **Input parameters** section of the console or by using the `parameters` option from the command line\) are automatically processed by all child Automations\.

You can target resources for an Automation execution by using tags, Resource Groups, and parameter values\. Additionally, you can use the `TargetMaps` option to target multiple parameter values from the command line or a file\. The following section describes each of these targeting options in more detail\.

## Targeting Tags<a name="automation-working-targets-tags"></a>

Many AWS resources support tags, including Amazon Elastic Compute Cloud \(Amazon EC2\) and Amazon Relational Database Service \(Amazon RDS\) instances, Amazon Elastic Block Store \(Amazon EBS\) volumes and snapshots, Resource Groups, and Amazon Simple Storage Service \(Amazon S3\) buckets, to name a few\. You can quickly run Automation workflows on your AWS resources by targeting tags\. A tag is a key\-value pair, such as Operating\_System\-Linux or Department\-Finance\. If you assign a specific name to a resource, then you can also use the word "Name" as a key, and the name of the resource as the value\.

When you specify a tag as the target for an Automation, you also specify a target parameter\. The target parameter uses the `TargetParameterName` option\. By choosing a target parameter, you define the type of resource on which the Automation runs\. The target parameter you specify with the tag must be a valid parameter defined in the Automation document\. For example, if you want to target dozens of Amazon EC2 instances by using tags, then choose the `InstanceId` target parameter\. By choosing this parameter, you define *instances* as the resource type for the Automation execution\. The following screenshot uses the AWS\-DetachEBSVolume document\. The logical target parameter is `VolumeId`\. 

![\[Using tags to target a Systems Manager Automation execution\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-rate-control-tags-1.png)

The AWS\-DetachEBSVolume document also includes a special property called **Target type**, which is set to `/AWS::EC2::Volume`\. This means that if the tag\-key pair `Finance-TestEnv` returns different types of resources \(for example, Amazon EC2 instances, Amazon EBS volumes, Amazon EBS snapshots\) then only Amazon EBS volumes will be used\.

**Important**  
Target parameter names are case sensitive\. If you run Automations by using either the AWS CLI or AWS Tools for Windows PowerShell, then you must enter the target parameter name exactly as it is defined in the Automation document\. If you don't, the system returns an `InvalidAutomationExecutionParametersException` error\. You can use the [DescribeDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeDocument.html) API action to see information about the available target parameters in a specific document\. Here is an example AWS CLI command that provides information about the AWS\-DeleteSnapshot document:  

```
aws ssm describe-document --name AWS-DeleteSnapshot
```

You can specify a maximum of 5 tag keys and up to 5 values for *each* key\. If you specify multiple keys, the Automation runs only on those resources that match all of the criteria\. Specifying multiple keys and values for a target is only supported from the command line\. 

Here are some example AWS CLI commands that target resources by using tags\.

**Example 1: Targeting tags that use a single key\-value pair**

This example restarts all Amazon EC2 instances that are tagged with a key of *Department* and a value of *HumanResources*\. The target parameter uses the *InstanceId* parameter from the Automation document\. The example uses an additional parameter to run the automation by using an Automation service role \(also called an assume role\)\.

```
aws ssm start-automation-execution --document-name AWS-RestartEC2Instance --targets Key=tag:Department,Values=HumanResources --target-parameter-name InstanceId --parameters "AutomationAssumeRole=arn:aws:iam::111122223333:role/AutomationServiceRole"
```

The following example uses the AWS\-DeleteSnapshot Automation document to delete all snapshots with a key of *Name* and a value of *January2018Backups*\. The target parameter uses the *VolumeId* parameter\.

```
aws ssm start-automation-execution --document-name AWS-DeleteSnapshot --targets Key=tag:Name,Values=January2018Backups --target-parameter-name VolumeId
```

**Example 2: Targeting tags that use multiple key\-value pairs**

The following example uses the AWS\-ResizeInstance Automation document to change the instance size of all instances tagged with a key of *Environment* and a value of *Development*, *Test*, and *Pre\-production*\.

```
aws ssm start-automation-execution --document-name AWS-ResizeInstance --targets Key=tag:Environment,Values=Development,Test,Pre-production --target-parameter-name InstanceType --parameters "InstanceType=p3.8xlarge"
```

The following example uses the AWS\-StopEC2Instance Automation document to stop all Amazon EC2 instances that are tagged with both of the following keys and values:*Environment\-Pre\-production* and *ServerRole\-WebServer*\. The Automation runs only on instances tagged with both key\-value pairs\.

```
aws ssm start-automation-execution --document-name AWS-ResizeInstance --targets Key=tag:Environment,Values=Pre-production Key=tag:ServerRole, Values=WebServer --target-parameter-name InstanceId
```

## Targeting AWS Resource Groups<a name="automation-working-targets-resource-groups"></a>

You can specify a single AWS resource group as the target of an Automation\. Systems Manager creates a child Automation for every object in the target Resource Group\.

For example, say that one of your Resource Groups is named PatchedAMIs\. This Resource Group includes a list of 25 Windows Amazon Machine Images \(AMIs\) that are routinely patched\. If you run an Automation that uses the AWS\-CreateManagedWindowsInstance document and target this Resource Group, then Systems Manager creates a child Automation for each of the 25 AMIs\. This means, that by targeting the PatchedAMIs Resource Group, the Automation creates 25 instances from a list of patched AMIs\. The parent Automation is complete when all child Automations complete processing or reach a final state\.

The following AWS CLI command applies to the PatchAMIs Resource Group example\. The command takes the *AmiId* parameter for the \-\-target\-parameter\-name option\. The command doesn't include an additional parameter defining which type of instance to create from each AMI\. The AWS\-CreateManagedWindowsInstance document defaults to the t2\.medium instance type, so this command would create 25 t2\.medium Windows instances\.

```
aws ssm start-automation-execution --document-name AWS-CreateManagedWindowsInstance --targets Key=ResourceGroup,Values=PatchedAMIs  --target-parameter-name AmiId
```

The following console example uses a Resource Group called t2\-micro\-instances\.

![\[Using an AWS resource group to target a Systems Manager Automation execution\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-rate-control-resource-groups.png)

## Targeting Parameter Values<a name="automation-working-targets-parameter-values"></a>

You can also target a parameter value\. You enter `ParameterValues` as the key and then enter the specific resource value where you want the Automation workflow to run\. If you specify multiple values, Systems Manager runs a child Automation workflow on each value specified\.

For example, say that your Automation document includes an **InstanceID** parameter\. If you target the values of the **InstanceID** parameter when you run the Automation, then Systems Manager runs a child Automation for each instance ID value specified\. The parent Automation is complete when the Automation finishes running each specified instance, or if the execution fails\. You can target a maximum of 50 parameter values\.

The following example uses the AWS\-CreateImage Automation document\. The target parameter name specified is *InstanceId*\. The key uses *ParameterValues*\. The values are two Amazon EC2 instance IDs\. This command creates an Automation workflow for each instance, which produces an AMI from each instance\. 

```
aws ssm start-automation-execution --document-name AWS-CreateImage --target-parameter-name InstanceId --targets Key=ParameterValues,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE
```

### Targeting Parameter Value Maps<a name="automation-working-targets-maps"></a>

The `TargetMaps` option expands your ability to target `ParameterValues`\. You can enter an array of parameter values by using `TargetMaps` at the command line\. You can specify a maximum of 50 parameter values at the command line\. If you want to run commands that specify more than 50 parameter values, then you can enter the values in a JSON file\. You can then call the file from the command line\.

**Note**  
`TargetMaps` are not supported in the console\.

Use the following format to specify multiple parameter values by using the `TargetMaps` option in a command:

```
aws ssm start-automation-execution —document-name name_of_document --target-maps “parameterA=parameterA1, parameterB=parameterB1, parameterC=parameterC1” “parameterA=parameterA2, parameterB=parameterB2, parameterC=parameterC2” “parameterA=parameterA3, parameterB=parameterB3, parameterC=parameterC3”
```

If you want to enter more than 50 parameter values for the `TargetMaps` option, then specify the values in a file by using the following JSON format\. Using a JSON file also improves readability when providing multiple parameter values\.

```
[

      {“parameterA”:”parameterValueA1”, “parameterB”:”parameterValueB1”, “parameterC”:”parameterValueC1”},

      {“parameterA”:”parameterValueA2”, “parameterB”:”parameterValueB2”, “parameterC”:”parameterValueC2”},

      {“parameterA”:”parameterValueA3”, “parameterB”:”parameterValueB3”, “parameterC”:”parameterValueC3”}

   ]
```

Save the file with a \.json file extension\. You can call the file by using the following command:

```
aws ssm start-automation-execution --document-name name_of_document –-parameters one_or_more_input_parameters --target-maps full_path_to_file/file_name.json
```

You can also download the file from an Amazon S3 bucket, as long as you have permission to read data from the bucket\. Use the following command format:

```
aws ssm start-automation-execution —document-name name_of_document --target-maps http://bucket_name.s3.amazonaws.com/file_name.json
```

Here is an example scenario to help you understand the `TargetMaps` option\. In this scenario, a user wants to create Amazon EC2 instances of different types from different AMIs\. To perform this task, the user creates an Automation document named AMI\_Testing\. This document defines two input parameters: `instanceType` and `imageId`\. 

```
{
  "description": "AMI Testing",
  "schemaVersion": "0.3",
  "assumeRole": "{{assumeRole}}",
  "parameters": {
    "assumeRole": {
      "type": "String",
      "description": "Role under which to run the automation",
      "default": ""
    },
    "instanceType": {
      "type": "String",
      "description": "Type of EC2 Instance to launch for this test"
    },
    "imageId": {
      "type": "String",
      "description": "Source AMI id from which to run instance"
    }
  },
  "mainSteps": [
    {
      "name": "runInstances",
      "action": "aws:runInstances",
      "maxAttempts": 1,
      "onFailure": "Abort",
      "inputs": {
        "ImageId": "{{imageId}}",
        "InstanceType": "{{instanceType}}",
        "MinInstanceCount": 1,
        "MaxInstanceCount": 1
      }
    }
  ],
  "outputs": [
    "runInstances.InstanceIds"
  ]
}
```

The user then specifies the following target parameter values in a file named AMI\_instance\_types\.json\.

```
[
  {
    "instanceType" : ["t2.micro"],     
    "imageId" : ["ami-b70554c8"]     
  },
  {
    "instanceType" : ["t2.small"],     
    "imageId" : ["ami-b70554c8"]     
  },
  {
    "instanceType" : ["t2.medium"],     
    "imageId" : ["ami-cfe4b2b0"]     
  },
  {
    "instanceType" : ["t2.medium"],     
    "imageId" : ["ami-cfe4b2b0"]     
  },
  {
    "instanceType" : ["t2.medium"],     
    "imageId" : ["ami-cfe4b2b0"]     
  }
]
```

The user can run the Automation and create the five Amazon EC2 instances defined in AMI\_instance\_types\.json by running the following command:

```
aws ssm start-automation-execution --document-name AMI_Testing --target-parameter-name imageId --target-maps file:///home/TestUser/workspace/runinstances/AMI_instance_types.json
```