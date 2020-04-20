# About configurations stored in Amazon S3<a name="appconfig-creating-configuration-and-profile-S3-source"></a>

You can store configurations in an Amazon Simple Storage Service \(Amazon S3\) bucket\. When you create the configuration profile, you specify the URI to a single S3 object in a bucket\. You also specify the Amazon Resource Name \(ARN\) of an AWS Identity and Access Management \(IAM\) role that gives AppConfig permission to get the object\. Before you create a configuration profile for an Amazon S3 object, be aware of the following restrictions\.


****  

| Restriction | Details | 
| --- | --- | 
|  Size  |  Configurations stored as S3 objects can be a maximum of 1 MB in size\.  | 
|  Object encryption  |  A configuration profile can't target an encrypted S3 object\.  | 
|  Storage classes  |  AppConfig supports the following S3 storage classes: `STANDARD`, `INTELLIGENT_TIERING`, `REDUCED_REDUNDANCY`, `STANDARD_IA`, and `ONEZONE_IA`\. The following classes are not supported: All S3 Glacier classes \(`GLACIER` and `DEEP_ARCHIVE`\)\.  | 
|  Versioning  |  AppConfig requires that the S3 object use versioning\.  | 

## Configuring permissions for a configuration stored as an Amazon S3 object<a name="appconfig-creating-configuration-and-profile-S3-source-permissions"></a>

When you create a configuration profile for a configuration stored as an S3 object, you must specify an ARN for an IAM role that gives AppConfig permission to get the object\. The role must include the following permissions\.

Permissions to access the S3 object
+ s3:GetObject
+ s3:GetObjectVersion

Permissions to list S3 buckets

s3:ListAllMyBuckets

Permissions to access the S3 bucket where the object is stored
+ s3:GetBucketLocation
+ s3:GetBucketVersioning
+ s3:ListBucket
+ s3:ListBucketVersions

Complete the following procedure to create a role that enables AppConfig to get a configuration stored in an S3 object\.

**Creating the IAM Policy for Accessing an S3 Object**  
Use the following procedure to create an IAM policy that enables AppConfig to get a configuration stored in an S3 object\.

**To create an IAM policy for accessing an S3 object**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. Update the following sample policy with information about your S3 bucket and configuration object\. Then paste the policy into the text field on the **JSON** tab\. 

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetObject",
           "s3:GetObjectVersion"
         ],
         "Resource": "arn:aws:s3:::my-bucket/my-configurations/my-configuration.json"
       },
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketLocation",
           "s3:GetBucketVersioning",
           "s3:ListBucketVersions",
           "s3:ListBucket"
         ],
         "Resource": [
           "arn:aws:s3:::my-bucket"
         ]
       },
       {
         "Effect": "Allow",
         "Action": "s3:ListAllMyBuckets",
         "Resource": "*"
       } 
     ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, type a name in the **Name** box, and then type a description\.

1. Choose **Create policy**\. The system returns you to the **Roles** page\.

**Creating the IAM Role for Accessing an S3 Object**  
Use the following procedure to create an IAM role that enables AppConfig to get a configuration stored in an S3 object\.

**To create an IAM role for accessing an Amazon S3 object**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** section, choose **AWS service**\.  
![\[Choosing AWS Service on the AWS IAM Role page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-s3-role-1.png)

1. In the **Choose a use case** section, under **Common use cases**, choose **EC2**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, in the search box, enter the name of the policy you created in the previous procedure\. 

1. Choose the policy and then choose **Next: Tags**\.

1. On the **Add tags \(optional\)** page, enter a key and an optional value, and then choose **Next:Review**\.

1. On the **Review** page, type a name in the **Role name** field, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. On the **Roles** page, choose the role you just created to open the **Summary** page\. Note the **Role Name** and **Role ARN**\. You will specify the role ARN when you create the configuration profile later in this topic\.  
![\[Copying the ARN of IAM role\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-s3-role-2.png)

**Creating a Trust Relationship**  
Use the following procedure to configure the role you just created to trust AppConfig\.

**To add a trust relationship**

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. Delete `"ec2.amazonaws.com"` and add `"appconfig.amazonaws.com"`, as shown in the following example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "appconfig.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.