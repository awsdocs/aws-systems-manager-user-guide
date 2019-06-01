# AWS Resource Groups<a name="systems-manager-resource-groups"></a>

A *resource group* is a collection of AWS resources that are all in the same AWS Region, and that match criteria provided in a query\. \(In AWS, a resource is an entity that you can work with\. Examples include an Amazon EC2 instance, an AWS CloudFormation stack, and an Amazon S3 bucket\.\) You build queries in the AWS Resource Groups \(Resource Groups\) console, or pass them as arguments to Resource Groups commands in the AWS CLI\.

With AWS Resource Groups, you can create a custom console that organizes and consolidates information based on criteria that you specify in tags\. After you add resources to a group you created in Resource Groups, use AWS Systems Manager tools such as Automation to simplify management tasks on your resource group\. You can also use the resource groups you create as the basis for viewing monitoring and configuration insights in Systems Manager\. 

For information about granting the IAM users in your account access to Resource Groups and its Tag Editor in the AWS Management Console, see [Create Policies for Tag Editor and Resource Groups](setup-create-users-nonadmin-policies.md)\.

For more information about the Resource Groups service, see the *[AWS Resource Groups User Guide](https://docs.aws.amazon.com/ARG/latest/userguide/)*\.