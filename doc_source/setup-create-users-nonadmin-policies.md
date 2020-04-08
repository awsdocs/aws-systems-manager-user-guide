# Task 1: Create policies for Tag Editor and Resource Groups<a name="setup-create-users-nonadmin-policies"></a>

You can use *resource groups* to organize your AWS resources\. Resource groups make it easier to manage and automate tasks on large numbers of resources across AWS at one time\. 

Tags are properties of a resource, so they are shared across your entire account\. Users in a department or specialized group can draw from a common vocabulary \(tags\) to create resource groups that are meaningful to their roles and responsibilities\. Having a common pool of tags also means that when users share a resource group, they don't have to worry about missing or conflicting tag information\.

AWS resource groups are managed in the AWS Resource Groups service\. It is optional to provide the users and user groups in your account access to this service and its Tag Editor, but we recommend it for more effective management operations\.

For more information about the AWS Resource Groups service and Tag Editor, see the *[AWS Resource Groups User Guide](https://docs.aws.amazon.com/ARG/latest/userguide/)\.*

The following procedure shows how to create a custom policy that allows your account's users to access Resource Groups and Tag Editor\. You need to complete this procedure only one time\. In later topics, you grant these permissions by attaching the policy to users and user groups in your account\.

**To create a policy for Tag Editor and Resource Groups**

1. Use your AWS account ID or account alias, and the credentials for your admin IAM user, to sign in to the [IAM console](https://console.aws.amazon.com/iam)\.

1. In the navigation pane of the console, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab and paste the following policy:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "resource-groups:*",
           "cloudformation:DescribeStackResources",
           "cloudformation:ListStackResources",
           "tag:GetResources",
           "tag:TagResources",
           "tag:UntagResources",
           "tag:getTagKeys",
           "tag:getTagValues"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

   This policy allows all actions for Tag Editor and Resource Groups\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter **SSMTagEditorAndResourceGroupAccess** or another name that you prefer, and then choose **Create policy**\.

Continue to [Task 2: Create user groups](setup-create-users-nonadmin-groups.md)\.