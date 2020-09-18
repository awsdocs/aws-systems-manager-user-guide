# Assigning parameter policies<a name="parameter-store-policies"></a>

Parameter policies help you manage a growing set of parameters by enabling you to assign specific criteria to a parameter such as an expiration date or *time to live*\. Parameter policies are especially helpful in forcing you to update or delete passwords and configuration data stored in Parameter Store\. Parameter Store offers the following types of policies: `Expiration`, `ExpirationNotification`, and `NoChangeNotification`\. The policies are described in more detail in this section\.

Parameter Store enforces parameter policies by using asynchronous, periodic scans\. After you create a policy, you don't need to perform additional actions to enforce the policy\. Parameter Store independently performs the action defined by the policy according to the criteria you specified\. 

**Note**  
Parameter policies are available for parameters that use the advanced parameters tier\. For more information, see [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

A parameter policy is a JSON array, as shown in the following table\. You can assign a policy when you create a new advanced parameter, or you can apply a policy by updating a parameter\. Parameter Store supports the following types of parameter policies\.


| Policy | Details | Examples | 
| --- | --- | --- | 
|  **Expiration**  |  This policy deletes the parameter\. You can specify a specific date and time by using either the `ISO_INSTANT` format or the `ISO_OFFSET_DATE_TIME` format\. To change when you want the parameter to be deleted, you must update the policy\. Updating a *parameter* does not affect the expiration date or time of the policy attached to it\. When the expiration date and time is reached, Parameter Store deletes the parameter\.  This example uses the `ISO_INSTANT` format\. You can also specify a date and time by using the `ISO_OFFSET_DATE_TIME` format\. Here is an example: `2019-11-01T22:13:48.87+10:30:00` \.   |  <pre>{<br />   "Type":"Expiration",<br />   "Version":"1.0",<br />   "Attributes":{<br />      "Timestamp":"2018-12-02T21:34:33.000Z"<br />   }<br />}</pre>  | 
|  **ExpirationNotification**  |  This policy triggers an event in Amazon EventBridge that notifies you about the expiration\. By using this policy, you can receive notifications before the expiration time is reached, in units of days or hours\.  |  <pre>{<br />   "Type":"ExpirationNotification",<br />   "Version":"1.0",<br />   "Attributes":{<br />      "Before":"15",<br />      "Unit":"Days"<br />   }<br />}</pre>  | 
|  **NoChangeNotification**  |  This policy triggers an event in EventBridge if a parameter has *not* been modified for a specified period of time\. This policy type is useful when, for example, a password needs to be changed within a period of time\. This policy determines when to send a notification by reading the `LastModifiedTime` attribute of the parameter\. If you change or edit a parameter, the system resets the notification time period based on the new value of `LastModifiedTime`\.  |  <pre>{<br />   "Type":"NoChangeNotification",<br />   "Version":"1.0",<br />   "Attributes":{<br />      "After":"20",<br />      "Unit":"Days"<br />   }<br />}</pre>  | 

You can assign multiple policies to a parameter\. For example, you can assign `Expiration` and `ExpirationNotification` policies so that the system triggers an EventBridge event to notify you about the impending deletion of a parameter\. You can assign a maximum of ten \(10\) policies to a parameter\.

The following example shows a [PutParameter](https://docs.aws.amazon.com/ssm/latest/APIReference/API_PutParameter.html) API request that assigns four policies to a new `SecureString` parameter named `ProdDB3`\.

```
PutParameterRequest
{
   "Name":"ProdDB3",
   "Description":"Parameter with policies",
   "Value":"P@ssW*rd21",
   "Type":"SecureString",
   "Overwrite":"True",
   "Policies":[
      {
         "Type":"Expiration",
         "Version":"1.0",
         "Attributes":{
            "Timestamp":"2018-12-02T21:34:33.000Z"
         }
      },
      {
         "Type":"ExpirationNotification",
         "Version":"1.0",
         "Attributes":{
            "Before":"30",
            "Unit":"Days"
         }
      },
      {
         "Type":"ExpirationNotification",
         "Version":"1.0",
         "Attributes":{
            "Before":"15",
            "Unit":"Days"
         }
      },
      {
         "Type":"NoChangeNotification",
         "Version":"1.0",
         "Attributes":{
            "After":"20",
            "Unit":"Days"
         }
      }
   ]
}
```

## Adding policies to an existing parameter<a name="sysman-paramstore-su-policy-create"></a>

This section includes information about how to add policies to an existing parameter by using the AWS Systems Manager console, the AWS CLI, and AWS Tools for Windows PowerShell\. For information about how to create a new parameter that includes policies, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

**Topics**
+ [Add policies to an existing parameter \(console\)](#sysman-paramstore-policy-create-console)
+ [Add policies to an existing parameter \(AWS CLI\)](#sysman-paramstore-policy-create-cli)
+ [Add policies to an existing parameter by using the Tools for Windows PowerShell](#sysman-paramstore-policy-create-ps)

### Add policies to an existing parameter \(console\)<a name="sysman-paramstore-policy-create-console"></a>

Use the following procedure to add policies to an existing parameter by using the Systems Manager console\.

**To add policies to an existing parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the option next to the parameter that you want to update to include policies, and then choose **Edit**\.

1. Choose **Advanced**\.

1. \(Optional\) In the **Parameter policies** section, choose **Enabled**\. You can specify an expiration date and one or more notification policies for this parameter\.

1. Choose **Save changes**\.

**Important**  
Parameter Store preserves policies on a parameter until you either overwrite the policies with new policies or remove the policies\. 
To remove all policies from an existing parameter, edit the parameter and apply an empty policy by using brackets and curly braces, as follows: `[{}]`
If you add a new policy to a parameter that already has policies, then Systems Manager overwrites the policies attached to the parameter\. The existing policies are deleted\. If you want to add a new policy to a parameter that already has one or more policies, then you must copy and paste the original policies, type the new policy, and then save your changes\.

### Add policies to an existing parameter \(AWS CLI\)<a name="sysman-paramstore-policy-create-cli"></a>

Use the following procedure to add policies to an existing parameter by using the AWS CLI\.

**To add policies to an existing parameter**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to add policies to an existing parameter\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter   
       --name "parameter-name" \
       --value 'parameter-value' \
       --type parameter-type \
       --overwrite \
       --policies "[{policies-enclosed-in-brackets-and-curly-braces}]"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter   
       --name "parameter-name" ^
       --value 'parameter-value' ^
       --type parameter-type ^
       --overwrite ^
       --policies "[{policies-enclosed-in-brackets-and-curly-braces}]"
   ```

------

   Here is an example that includes an expiration policy that deletes the parameter after 15 days\. The example also includes a notification policy that generates an EventBridge event five \(5\) days before the parameter is deleted\. Last, it includes a `NoChangeNotification` policy if no changes are made to this parameter after 60 days\. The example uses an obfuscated name \(`3l3vat3131`\) for a password and a AWS Key Management Service \(AWS KMS\) customer master key \(CMK\)\. For more information about CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-managed-cmk) in the *AWS Key Management Service Developer Guide*\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "/Finance/Payroll/3l3vat3131" \
       --value "P@sSwW)rd" \
       --type "SecureString" \
       --overwrite \
       --policies "[{\"Type\":\"Expiration\",\"Version\":\"1.0\",\"Attributes\":{\"Timestamp\":\"2020-05-13T00:00:00.000Z\"}},{\"Type\":\"ExpirationNotification\",\"Version\":\"1.0\",\"Attributes\":{\"Before\":\"5\",\"Unit\":\"Days\"}},{\"Type\":\"NoChangeNotification\",\"Version\":\"1.0\",\"Attributes\":{\"After\":\"60\",\"Unit\":\"Days\"}}]"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "/Finance/Payroll/3l3vat3131" ^
       --value "P@sSwW)rd" ^
       --type "SecureString" ^
       --overwrite ^
       --policies "[{\"Type\":\"Expiration\",\"Version\":\"1.0\",\"Attributes\":{\"Timestamp\":\"2020-05-13T00:00:00.000Z\"}},{\"Type\":\"ExpirationNotification\",\"Version\":\"1.0\",\"Attributes\":{\"Before\":\"5\",\"Unit\":\"Days\"}},{\"Type\":\"NoChangeNotification\",\"Version\":\"1.0\",\"Attributes\":{\"After\":\"60\",\"Unit\":\"Days\"}}]"
   ```

------

1. Run the following command to verify the details of the parameter\.

------
#### [ Linux ]

   ```
   aws ssm describe-parameters  \
       --parameter-filters "Key=Name,Values=parameter-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-parameters  ^
       --parameter-filters "Key=Name,Values=parameter-name"
   ```

------

**Important**  
Parameter Store retains policies for a parameter until you either overwrite the policies with new policies or remove the policies\. 
To remove all policies from an existing parameter, edit the parameter and apply an empty policy of brackets and curly braces\. For example:  

  ```
  aws ssm put-parameter \
      --name parameter-name \
      --type parameter-type  \
      --value 'parameter-value' \
      --policies "[{}]"
  ```

  ```
  aws ssm put-parameter ^
      --name parameter-name ^
      --type parameter-type  ^
      --value 'parameter-value' ^
      --policies "[{}]"
  ```
If you add a new policy to a parameter that already has policies, then Systems Manager overwrites the policies attached to the parameter\. The existing policies are deleted\. If you want to add a new policy to a parameter that already has one or more policies, then you must copy and paste the original policies, type the new policy, and then save your changes\.

### Add policies to an existing parameter by using the Tools for Windows PowerShell<a name="sysman-paramstore-policy-create-ps"></a>

Use the following procedure to add policies to an existing parameter by using Tools for Windows PowerShell\.

**To add policies to an existing parameter**

1. Open AWS Tools for Windows PowerShell and run the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey access-key-name –SecretKey secret-key-name
   ```

1. Run the following command to set the Region for your PowerShell session\. The example uses the US East \(Ohio\) Region \(us\-east\-2\)\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Run the following command to add policies to an existing parameter\.

   ```
   Write-SSMParameter -Name "parameter-name" -Value "parameter-value" -Type "parameter-type" -Policies "[{policies-enclosed-in-brackets-and-curly-braces}]" -Overwrite
   ```

   Here is an example that includes an expiration policy that deletes the parameter at midnight \(GMT\) on May 13, 2020\. The example also includes a notification policy that generates an EventBridge event five \(5\) days before the parameter is deleted\. Last, it includes a `NoChangeNotification` policy if no changes are made to this parameter after 60 days\. The example uses an obfuscated name \(`3l3vat3131`\) for a password and an AWS\-managed customer master key \(CMK\)\.

   ```
   Write-SSMParameter -Name "/Finance/Payroll/3l3vat3131" -Value "P@sSwW)rd" -Type "SecureString" -Policies "[{\"Type\":\"Expiration\",\"Version\":\"1.0\",\"Attributes\":{\"Timestamp\":\"2018-05-13T00:00:00.000Z\"}},{\"Type\":\"ExpirationNotification\",\"Version\":\"1.0\",\"Attributes\":{\"Before\":\"5\",\"Unit\":\"Days\"}},{\"Type\":\"NoChangeNotification\",\"Version\":\"1.0\",\"Attributes\":{\"After\":\"60\",\"Unit\":\"Days\"}}]" -Overwrite
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "parameter-name-you-specified").Parameters
   ```

**Important**  
Parameter Store preserves policies on a parameter until you either overwrite the policies with new policies or remove the policies\. 
To remove all policies from an existing parameter, edit the parameter and apply an empty policy of brackets and curly braces\. For example:  

  ```
  Write-SSMParameter -Name "parameter-name" -Value "parameter-value" -Type "parameter-type" -Policies "[{}]"
  ```
If you add a new policy to a parameter that already has policies, then Systems Manager overwrites the policies attached to the parameter\. The existing policies are deleted\. If you want to add a new policy to a parameter that already has one or more policies, then you must copy and paste the original policies, type the new policy, and then save your changes\.