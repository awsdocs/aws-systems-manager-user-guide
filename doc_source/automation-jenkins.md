# Walkthrough: Using Automation with Jenkins<a name="automation-jenkins"></a>

If your organization uses Jenkins software in a CI/CD pipeline, you can add Automation as a post\-build step to pre\-install application releases into Amazon Machines Images \(AMIs\)\. You can also use the Jenkins scheduling feature to call Automation and create your own operating system \(OS\) patching cadence\.

The example below shows how to invoke Automation from a Jenkins server that is running either on\-premises or in Amazon EC2\. For authentication, the Jenkins server uses AWS credentials based on an AWS Identity and Access Management \(IAM\) user that you create in the example\. If your Jenkins server is running in Amazon EC2, you can also authenticate it using an IAM instance profile role\.

**Note**  
Be sure to follow Jenkins security best\-practices when configuring your instance\.

**Before You Begin**  
Complete the following tasks before you configure Automation with Jenkins\.
+ Complete the [Walkthrough: Simplify AMI patching using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md) example\. The following example uses the **UpdateMyLatestWindowsAmi** automation document created in that example\.
+ Configure IAM roles for Automation\. Systems Manager requires an instance profile role and a service role ARN to process Automation workflows\. For more information, see [Getting started with Automation](automation-setup.md)\.
+ After you configure IAM roles for Automation, use the following procedure to create an IAM user account for your Jenkins server\. The Automation workflow uses the IAM user account's Access key and Secret key to authenticate the Jenkins server during execution\.

**To create a user account for the Jenkins server**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Replace the default content with the following\. Be sure to replace *us\-west\-2* and *123456789012* with the Region and account you want to use\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "ssm:StartAutomationExecution",
               "Resource": [
                   "arn:aws:ssm:us-west-2:123456789012:document/UpdateMyLatestWindowsAmi",
                   "arn:aws:ssm:us-west-2:123456789012:automation-definition/UpdateMyLatestWindowsAmi:$DEFAULT"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **JenkinsPolicy**\.

1. Choose **Create policy**\.

1. In the navigation pane, choose **Users**\.

1. Choose **Add user**\.

1. In the **Set user details** section, specify a user name \(for example, *Jenkins*\)\.

1. In the **Select AWS access type** section, choose **Programmatic Access**\.

1. Choose **Next:Permissions**\.

1. In the **Set permissions for** section, choose **Attach existing policies directly**\.

1. In the filter field, type the name of the policy you created earlier\.

1. Select the check box next to the policy, and then choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key\-value pairs to organize, track, or control access for this user, and then choose **Next: Review**\.

1. Verify the details, and then choose **Create**\.

1. Copy the access and secret keys to a text file\. You will specify these credentials in the next procedure\.

Use the following procedure to configure the AWS CLI on your Jenkins server\.

**To configure the Jenkins server for Automation**

1. Connect to your Jenkins server on port 8080 using your preferred browser to access the management interface\.

1. Enter the password found in `/var/lib/jenkins/secrets/initialAdminPassword`\. To display your password, run the following command:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

1. The Jenkins installation script directs you to the **Customize Jenkins** page\. Select **Install suggested plugins**\.

1. Once the installation is complete, choose **Administrator Credentials**, select **Save Credentials**, and then select **Start Using Jenkins**\.

1. In the left navigation pane, choose **Manage Jenkins**, and then choose **Manage Plugins**\.

1. Choose the **Available** tab, and then enter **Amazon EC2 plugin**\.

1. Select the check box for **Amazon EC2 plugin**, and then select **Install without restart**\.

1. When the installation completes, select **Go back to the top page**\.

1. Choose **Manage Jenkins**, and then choose **Configure System**\.

1. In the **Cloud** section, select **Add a new cloud**, and then choose **Amazon EC2**\.

1. Enter your information in the remaining fields\. Note that you must enter your AWS credentials in the **Add Credentials** field\.

Use the following procedure to configure your Jenkins project to invoke Automation\.

**To configure your Jenkins server to invoke Automation**

1. Open the Jenkins console in a web browser\.

1. Choose the project that you want to configure with Automation, and then choose **Configure**\.

1. On the **Build** tab, choose **Add Build Step**\.

1. Choose **Execute shell** or **Execute Windows batch command** \(depending on your operating system\)\.

1. In the **Command** box, run an AWS CLI command like the following:

   ```
   aws --region the AWS Region of your source AMI ssm start-automation-execution --document-name your document name --parameters parameters for the document
   ```

   The following example command uses the **UpdateMyLatestWindowsAmi** document and the Systems Manager Parameter `latestAmi` created in [Walkthrough: Simplify AMI patching using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md):

   ```
   aws --region region-id ssm start-automation-execution \
       --document-name UpdateMyLatestWindowsAmi \
       --parameters \
           "sourceAMIid='{{ssm:latestAmi}}'"
   ```

   In Jenkins, the command looks like the example in the following screenshot\.  
![\[Jenkins information\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-ami-jenkins2.png)

1. In the Jenkins project, choose **Build Now**\. Jenkins returns output similar to the following example\.  
![\[Jenkins information\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-ami-jenkins.png)