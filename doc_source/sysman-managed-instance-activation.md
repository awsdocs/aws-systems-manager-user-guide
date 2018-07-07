# Create a Managed\-Instance Activation for a Hybrid Environment<a name="sysman-managed-instance-activation"></a>

To set up servers and VMs in your hybrid environment as managed instances, you need to create a managed\-instance activation\. After you complete the activation, you receive an Activation Code and Activation ID\. This Code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a managed\-instance activation \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Activations**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Activations**\.

1. Choose **Create activation**\.

1. \(Optional\) In the **Activation description** field, enter a description for this activation\. The description is optional, be we recommend that you enter a description if you plan to activate large numbers of servers and VMs\.

1. In the **Instance limit** field, specify the total number of servers or VMs that you want to register with AWS\. 

1. In the **IAM role name** section, choose a service role option that enables your servers and VMs to communicate with AWS Systems Manager in the cloud:

   1. Choose **Use the system created default command execution role** to use a role and managed policy created by AWS\. 

   1. Choose **Select an existing custom IAM role that has the required permissions** to use the optional custom role you created earlier\.

1. In the **Activation expiry date** field, specify an expiration date for the activation\. 
**Note**  
If you want to register additional managed instances after the expiry date, you must create a new activation\. The expiry date has no impact on registered and running instances\.

1. \(Optional\) In the **Default instance name** field, specify a name\. 

1. Choose **Create activation**\.
**Important**  
Store the managed\-instance Activation Code and Activation ID in a safe place\. You specify this Code and ID when you install SSM Agent on servers and VMs in your hybrid environment\. If you lose the Code and ID, you must create a new activation\.

**To create a managed\-instance activation using the console \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and choose **Activations**\.

1. Choose **Create an Activation**\.

1. Fill out the form and choose **Create Activation**\.

   Note that you can specify a date when the activation expires\. If you want to register additional managed instances after the expiry date, you must create a new activation\. The expiry date has no impact on registered and running instances\.

1. Store the managed\-instance Activation Code and Activation ID in a safe place\. You specify this Code and ID when you install SSM Agent on servers and VMs in your hybrid environment\. If you lose the code and ID, you must create a new activation\.

**To create a managed\-instance activation using the AWS Tools for Windows PowerShell**

1. On a machine with where you have installed AWS Tools for Windows PowerShell, run the following command in AWS Tools for Windows PowerShell\.

   ```
   New-SSMActivation -DefaultInstanceName name -IamRole iam-service-role-name -RegistrationLimit number-of-managed-instances –Region region
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   For example:

   ```
   New-SSMActivation -DefaultInstanceName MyWebServers -IamRole RunCommandServiceRole -RegistrationLimit 10 –Region us-east-2
   ```

1. Press Enter\. If the activation is successful, the system returns an Activation Code and an Activation ID\. Store the Activation Code and Activation ID in a safe place\.

**To create a managed\-instance activation using the AWS CLI**

1. On a machine where you have installed the AWS Command Line Interface \(AWS CLI\), run the following command in the CLI\.

   ```
   aws ssm create-activation --default-instance-name name --iam-role IAM service role --registration-limit number of managed instances --region region
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   For example:

   ```
   aws ssm create-activation --default-instance-name MyWebServers --iam-role RunCommandServiceRole --registration-limit 10 --region us-east-2
   ```

1. Press Enter\. If the activation is successful, the system returns an Activation Code and an Activation ID\. Store the Activation Code and Activation ID in a safe place\.