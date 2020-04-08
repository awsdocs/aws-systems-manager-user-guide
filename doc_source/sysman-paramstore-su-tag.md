# Tagging Systems Manager parameters<a name="sysman-paramstore-su-tag"></a>

You can use the Systems Manager console, the AWS CLI, the AWS Tools for PowerShell, or the [AddTagsToResource](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_AddTagsToResource.html) API to add tags to Systems Manager resources, including documents, managed instances, maintenance windows, Parameter Store parameters, and patch baselines\. 

Tags are used to organize parameters\. For example, you can tag parameters for specific environments, departments, or users and groups\. After you tag a parameter, you can restrict access to it by creating an IAM policy that specifies the tags that the user can access\. For more information about restricting access to parameters by using tags, see [Controlling access to parameters using tags](sysman-paramstore-access.md#sysman-paramstore-access-tag)\.

For information about the Regions where Systems Manager is available, see [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**Topics**
+ [Tag a parameter \(console\)](#sysman-paramstore-su-tag-sys)
+ [Tag a parameter \(AWS CLI\)](#sysman-paramstore-su-tag-cli)
+ [Tag a parameter \(AWS Tools for PowerShell\)](#sysman-paramstore-su-tag-tfw)

## Tag a parameter \(console\)<a name="sysman-paramstore-su-tag-sys"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the left navigation, choose **Parameter Store**\.

1. Choose the name of a parameter you have already created, and then choose the **Tags** tab\.

1. In the first box, enter a key for the tag, such as **Environment**\.

1. In the second box, enter a value for the tag, such as **Test**\.

1. Choose **Save**\.

## Tag a parameter \(AWS CLI\)<a name="sysman-paramstore-su-tag-cli"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to list parameters that you can tag\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter that you want to tag\.

1. Run the following command to tag a parameter\.

   ```
   aws ssm add-tags-to-resource --resource-type "Parameter" --resource-id "the_parameter_name" --tags "Key=a key, for example Environment,Value=a value, for example TEST"
   ```

   If successful, the command has no output\.

1. Run the following command to verify the parameter tags\.

   ```
   aws ssm list-tags-for-resource --resource-type "Parameter" --resource-id "the_parameter_name"
   ```

## Tag a parameter \(AWS Tools for PowerShell\)<a name="sysman-paramstore-su-tag-tfw"></a>

1. Open AWS Tools for Windows PowerShell and run the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Run the following command to set the Region for your PowerShell session\.

   ```
   Set-DefaultAWSRegion -Region region
   ```

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

1. Run the following command to list parameters that you can tag\.

   ```
   Get-SSMParameterList
   ```

1. Run the following commands to tag a parameter\.

   ```
   $tag1 = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   $tag1.Key = "Environment"
   $tag1.Value = "TEST"
   Add-SSMResourceTag -ResourceType "Parameter" -ResourceId "parameter_name" -Tag $tag1
   ```

   If successful, the command has no output\.

1. Run the following command to verify the parameter tags\.

   ```
   Get-SSMResourceTag -ResourceType "Parameter" -ResourceId "parameter_name"
   ```