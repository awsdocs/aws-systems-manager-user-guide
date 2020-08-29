# Step 4: Create a managed\-instance activation for a hybrid environment<a name="sysman-managed-instance-activation"></a>

To set up servers and virtual machines \(VMs\) in your hybrid environment as managed instances, you need to create a managed\-instance activation\. After you successfully complete the activation, you *immediately* receive an Activation Code and Activation ID\. You specify this Code/ID combination when you install SSM Agent on servers and VMs in your hybrid environment\. The Code/ID provides secure access to the Systems Manager service from your managed instances\.

**Important**  
Systems Manager immediately returns the Activation Code and ID to the console or the command window, depending on how you created the activation\. Copy this information and store it in a safe place\. If you navigate away from the console or close the command window, you might lose this information\. If you lose it, you must create a new activation\. 

**About activation expirations**  
An *activation expiration* is a window of time when you can register on\-premises machines with Systems Manager\. An expired activation has no impact on your servers or virtual machines \(VMs\) that you previously registered with Systems Manager\. If an activation expires then you can’t register more servers or VMs with Systems Manager by using that specific activation\. You simply need to create a new one\.

Every on\-premises server and VM you previously registered remains registered as a Systems Manager managed instance until you explicitly deregister it\. You can deregister a managed instance on the **Managed Instances** page of the Systems Manager console, by using the AWS CLI command [deregister\-managed\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/deregister-managed-instance.html), or by using the API action [DeregisterManagedInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterManagedInstance.html)\.

**About activation tags**  
If you create an activation by using either the AWS CLI or AWS Tools for Windows PowerShell, you can specify tags\. Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. Here is an AWS CLI sample command to run on a local Linux machine that includes tags\.

```
aws ssm create-activation \
  --default-instance-name MyWebServers \
  --description "Activation for Finance department webservers" \
  --iam-role service-role/AmazonEC2RunCommandRoleForManagedInstances \
  --registration-limit 10 \
  --region us-east-2 \
  --tags "Key=Department,Value=Finance"
```

If you specify tags when you create an activation, then those tags are automatically assigned to your on\-premises servers and VMs when you activate them\.

You can't add tags to or delete tags from an existing activation\. If you don't want to automatically assign tags to your on\-premises servers and VMs using an activation, then you can add tags to them later\. More specifically, you can tag your on\-premises servers and VMs after they connect to Systems Manager for the first time\. After they connect, they are assigned a managed instance ID and listed in the Systems Manager console with an ID that is prefixed with "mi\-"\. For information about how to add tags to your managed instances without using the activation process, see [ Tagging managed instances](tagging-managed-instances.md)\.

**Note**  
You can't assign tags to an activation if you create it by using the Systems Manager console\. You must create it by using either the AWS CLI or Tools for Windows PowerShell\.

If you no longer want to manage an on\-premises server or virtual machine \(VM\) by using Systems Manager, you can deregister it\. For information, see [Deregistering managed instances in a hybrid environment](systems-manager-managed-instances-advanced-deregister.md)\.

**Topics**
+ [Create an activation \(console\)](#create-managed-instance-activation-console)
+ [Create a managed instance activation \(command line\)](#create-managed-instance-activation-commandline)

## Create an activation \(console\)<a name="create-managed-instance-activation-console"></a>

**To create a managed\-instance activation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Hybrid Activations**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Hybrid Activations**\.

1. Choose **Create activation**\.

1. \(Optional\) In the **Activation description** field, enter a description for this activation\. The description is optional, be we recommend that you enter a description if you plan to activate large numbers of servers and VMs\.

1. In the **Instance limit** field, specify the total number of on\-premises servers or VMs that you want to register with AWS as part of this activation\. 

1. In the ** IAM role name** section, choose a service role option that enables your servers and VMs to communicate with AWS Systems Manager in the cloud:

   1. Choose **Use the system created default command execution role** to use a role and managed policy created by AWS\. 

   1. Choose **Select an existing custom IAM role that has the required permissions** to use the optional custom role you created earlier\.

1. In the **Activation expiry date** field, specify an expiration date for the activation\. 
**Note**  
If you want to register additional managed instances after the expiry date, you must create a new activation\. The expiry date has no impact on registered and running instances\.

1. \(Optional\) In the **Default instance name** field, specify a name\. 

1. Choose **Create activation**\. Systems Manager immediately returns the Activation Code and ID to the console\. 

## Create a managed instance activation \(command line\)<a name="create-managed-instance-activation-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a managed instance activation\.

**To create an activation**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create an activation\.
**Note**  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

------
#### [ Linux ]

   ```
   aws ssm create-activation \
     --default-instance-name name \
     --iam-role iam-service-role-name \
     --registration-limit number-of-managed-instances \
     --region region \
     --tags "Key=key-name-1,Value=key-value-1" "Key=key-name-2,Value=key-value-2"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-activation ^
     --default-instance-name name ^
     --iam-role iam-service-role-name ^
     --registration-limit number-of-managed-instances ^
     --region region ^
     --tags "Key=key-name-1,Value=key-value-1" "Key=key-name-2,Value=key-value-2"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMActivation -DefaultInstanceName name `
     -IamRole iam-service-role-name `
     -RegistrationLimit number-of-managed-instances `
     –Region region `
     -Tag @{"Key"="key-name-1";"Value"="key-value-1"},@{"Key"="key-name-2";"Value"="key-value-2"}
   ```

------

   Here is an example\.

------
#### [ Linux ]

   ```
   aws ssm create-activation \
     --default-instance-name MyWebServers \
     --iam-role service-role/AmazonEC2RunCommandRoleForManagedInstances \
     --registration-limit 10 \
     --region us-east-2 \
     --tags "Key=Environment,Value=Production" "Key=Department,Value=Finance"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-activation ^
         --default-instance-name MyWebServers ^
         --iam-role service-role/AmazonEC2RunCommandRoleForManagedInstances ^
         --registration-limit 10 ^
         --region us-east-2 ^
         --tags "Key=Environment,Value=Production" "Key=Department,Value=Finance"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMActivation -DefaultInstanceName MyWebServers `
     -IamRole service-role/AmazonEC2RunCommandRoleForManagedInstances `
     -RegistrationLimit 10 `
     –Region us-east-2 `
     -Tag @{"Key"="Environment";"Value"="Production"},@{"Key"="Department";"Value"="Finance"}
   ```

------

   If the activation is created successfully, the system immediately returns an Activation Code and ID\.

Continue to [Step 6: Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)\.