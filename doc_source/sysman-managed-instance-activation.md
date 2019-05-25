# Step 4: Create a Managed\-Instance Activation for a Hybrid Environment<a name="sysman-managed-instance-activation"></a>

To set up servers and virtual machines \(VMs\) in your hybrid environment as managed instances, you need to create a managed\-instance activation\. After you successfully complete the activation, you *immediately* receive an Activation Code and Activation ID\. You specify this Code/ID combination when you install SSM Agent on servers and VMs in your hybrid environment\. The Code/ID provides secure access to the Systems Manager service from your managed instances\.

**Important**  
Systems Manager immediately returns the Activation Code and ID to the console or the command window, depending on how you created the activation\. Copy this information and store it in a safe place\. If you navigate away from the console or close the command window, you might lose this information\. If you lose it, you must create a new activation\. 

**About activation expirations**  
An *activation expiration* is a window of time when you can register on\-premises machines with Systems Manager\. An expired activation has no impact on your servers or virtual machines \(VMs\) that you registered with Systems Manager\. This means that if an activation expires then you can’t register more servers or VMs with Systems Manager by using that specific activation\. You simply need to create a new one\. Every on\-premises server and VM you previously registered remains registered as a Systems Manager managed instance until you explicity deregister it\. You can deregister a managed instance in your hybrid environment in the **Managed Instances** area of the Systems Manager console, by using the AWS CLI command [deregister\-managed\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/deregister-managed-instance.html), or by using the API action [DeregisterManagedInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterManagedInstance.html)\.

**To create a managed\-instance activation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Activations**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Activations**\.

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

**To create a managed\-instance activation using the Tools for Windows PowerShell**

1. On a machine with where you have installed AWS Tools for Windows PowerShell, run the following command in Tools for Windows PowerShell\.

   ```
   New-SSMActivation -DefaultInstanceName name -IamRole iam-service-role-name -RegistrationLimit number-of-managed-instances –Region region
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.

   For example:

   ```
   New-SSMActivation -DefaultInstanceName MyWebServers -IamRole SSMServiceRole -RegistrationLimit 10 –Region us-east-2
   ```

1. Press Enter\. If the activation is created successfully, the system immediately returns an Activation Code and ID\.

**To create a managed\-instance activation \(AWS CLI\)**

1. On a machine where you have installed the AWS Command Line Interface \(AWS CLI\), run the following command in the CLI\.

   ```
   aws ssm create-activation --default-instance-name name --iam-role iam-service-role-name --registration-limit number of managed instances --region region
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.

   For example:

   ```
   aws ssm create-activation --default-instance-name MyWebServers --iam-role SSMServiceRole --registration-limit 10 --region us-east-2
   ```

1. Press Enter\. If the activation is created successfully, the system immediately returns an Activation Code and ID\.

Continue to [Step 5: Install SSM Agent for a Hybrid Environment \(Windows\)](sysman-install-managed-win.md)\.