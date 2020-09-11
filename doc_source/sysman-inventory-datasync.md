# Configuring Resource Data Sync for Inventory<a name="sysman-inventory-datasync"></a>

This topic describes how to set up and configure resource data sync for Systems Manager Inventory\. For information about resource data sync for Systems Manager Explorer, see [Setting up Systems Manager Explorer to display data from multiple accounts and Regions](Explorer-resource-data-sync.md)\.

## About Resource Data Sync<a name="systems-manager-inventory-datasync-about"></a>

You can use Systems Manager resource data sync to send inventory data collected from all of your managed instances to a single S3 bucket\. Resource data sync then automatically updates the centralized data when new inventory data is collected\. With all inventory data stored in a target S3 bucket, you can use services like Amazon Athena and Amazon QuickSight to query and analyze the aggregated data\.

For example, say that you've configured inventory to collect data about the operating system \(OS\) and applications running on a fleet of 150 managed instances\. Some of these instances are located in a hybrid data center, and others are running in Amazon EC2 across multiple AWS Regions\. If you have *not* configured resource data sync, you either need to manually gather the collected inventory data for each instance, or you have to create scripts to gather this information\. You would then need to port the data into an application so that you can run queries and analyze it\.

With resource data sync, you perform a one\-time operation that synchronizes all inventory data from all of your managed instances\. After the sync is successfully created, Systems Manager creates a baseline of all inventory data and saves it in the target S3 bucket\. When new inventory data is collected, Systems Manager automatically updates the data in the S3 bucket\. You can then quickly and cost\-effectively port the data to Amazon Athena and Amazon QuickSight\.

Diagram 1 shows how resource data sync aggregates inventory data from managed instances in Amazon EC2 and a hybrid environment to a target S3 bucket\. This diagram also shows how resource data sync works with multiple AWS accounts and AWS Regions\.

**Diagram 1: Resource Data Sync with Multiple AWS Accounts and AWS Regions**

![\[Systems Manager resource data sync architecture\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-resource-data-sync.png)

If you delete a managed instance, resource data sync preserves the inventory file for the deleted instance\. For running instances, however, resource data sync automatically overwrites old inventory files when new files are created and written to the S3 bucket\. If you want to track inventory changes over time, you can use the AWS Config service to track the `SSM:ManagedInstanceInventory` resource type\. For more information, see [Getting Started with AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/getting-started.html)\.

Use the procedures in this section to create a resource data sync for Inventory by using the Amazon S3 and AWS Systems Manager consoles\. You can also use AWS CloudFormation to create or delete a resource data sync\. To use AWS CloudFormation, add the [AWS::SSM::ResourceDataSync](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-resourcedatasync.html) resource to your AWS CloudFormation template\. For information, see one of the following documentation resources:
+ [AWS CloudFormation Resource for Resource Data Sync in AWS Systems Manager](https://aws.amazon.com/blogs/mt/aws-cloudformation-resource-for-resource-data-sync-in-aws-systems-manager/) \(blog\)
+ [Working with AWS CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt inventory data in the S3 bucket\. For an example of how to create an encrypted sync by using the AWS CLI and how to work with the centralized data in Amazon Athena and Amazon QuickSight, see [Walkthrough: Use Resource Data Sync to aggregate inventory data](sysman-inventory-resource-data-sync.md)\. 

## Before you begin<a name="sysman-inventory-datasync-before-you-begin"></a>

Before you create a resource data sync, use the following procedure to create a central S3 bucket to store aggregated inventory data\. The procedure describes how to assign a bucket policy that enables Systems Manager to write inventory data to the bucket from multiple accounts\. If you already have an S3 bucket that you want to use to aggregate inventory data for resource data sync, then you must configure the bucket to use the policy in the following procedure\.

**To create and configure an S3 bucket for resource data sync**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *DOC\-EXAMPLE\-BUCKET* and *account\-id* with the name of the S3 bucket you created and a valid AWS account ID\.

   To enable multiple AWS accounts to send inventory data to the central Amazon S3 bucket, specify each account in the policy as shown in the following `Resource` sample:

   ```
   "Resource": [
        "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*/accountid=123456789012/*", 
        "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*/accountid=444455556666/*",
        "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*/accountid=777788889999/*"
                   ],
   ```

   Optionally, replace *bucket\-prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *bucket\-prefix/* from the ARN in the following policy\. 
**Note**  
For information about viewing your AWS account ID, see [Your AWS Account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"SSMBucketPermissionsCheck",
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"s3:GetBucketAcl",
            "Resource":"arn:aws:s3:::DOC-EXAMPLE-BUCKET"
         },
         {
            "Sid":" SSMBucketDelivery",
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"s3:PutObject",
            "Resource":[
               "arn:aws:s3:::DOC-EXAMPLE-BUCKET/bucket-prefix/*/accountid=account-id-1/*",
               "arn:aws:s3:::DOC-EXAMPLE-BUCKET/bucket-prefix/*/accountid=account-id-2/*",
               "arn:aws:s3:::DOC-EXAMPLE-BUCKET/bucket-prefix/*/accountid=account-id-3/*"
            ],
            "Condition":{
               "StringEquals":{
                  "s3:x-amz-acl":"bucket-owner-full-control"
               }
            }
         }
      ]
   }
   ```
**Note**  
The Asia Pacific Region came online in April 25, 2019\. If you create a resource data sync for an AWS Region that came online since the Asia Pacific \(Hong Kong\) Region \(ap\-east\-1\) or later, then you must enter a region\-specific service principal entry in the `SSMBucketDelivery` section\. The following example includes a region\-specific service principal entry for `ssm.ap-east-1.amazonaws.com`\.   

   ```
   {
            "Sid":" SSMBucketDelivery",
            "Effect":"Allow",
            "Principal":{
               "Service":["ssm.amazonaws.com","ssm.ap-east-1.amazonaws.com"]
            },
   ```

## Create a Resource Data Sync for Inventory<a name="sysman-inventory-datasync-create"></a>

Use the following procedure to create a resource data sync for Systems Manager Inventory by using the Systems Manager console\. For information about how to create a resource data sync by using the AWS CLI, see [Walkthrough: Configure your managed instances for Inventory by using the CLI](sysman-inventory-cliwalk.md)\.

**To create a Resource Data Sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose **Configure Inventory**, **Resource Data Syncs**, and then choose **Create resource data sync**\.

1. In the **Sync name** field, type a name for the sync configuration\.

1. In the **Bucket name** field, type the name of the Amazon S3 bucket you created at the start of this procedure\.

1. \(Optional\) In the **Bucket prefix** field, type the name of an S3 bucket prefix \(subdirectory\)\.

1. In the **Bucket region** field, choose **This region** if the S3 bucket you created is located in the current AWS Region\. If the bucket is located in a different AWS Region, choose **Another region**, and type the name of the Region\.
**Note**  
If the sync and the target S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/)\.

1. \(Optional\) In the **KMS Key ARN** field, type or paste a KMS Key ARN to encrypt inventory data in Amazon S3\.

1. Choose **Create**\.

To synchronize inventory data from multiple AWS Regions, you must create a resource data sync in *each* Region\. Repeat this procedure in each AWS Region where you want to collect inventory data and send it to the central S3 bucket\. When you create the sync in each Region, specify the central Amazon S3 bucket in the **Bucket name** field\. Then use the **Bucket region** option to choose the Region where you created the central Amazon S3 bucket, as shown in the following screen shot\. The next time the association runs to collect inventory data, Systems Manager stores the data in the central S3 bucket\. 

![\[Systems Manager resource data sync from multiple AWS Regions\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-rds-multiple-regions.png)

## Creating an Inventory Resource Data Sync for accounts defined in AWS Organizations<a name="systems-manager-inventory-resource-data-sync-AWS-Organizations"></a>

You can synchronize inventory data from AWS accounts defined in AWS Organizations to a central Amazon S3 bucket\. After you complete the following procedures, inventory data is synchronized to *individual* Amazon S3 key prefixes in the central bucket\. Each key prefix represents a different AWS account ID\.

**Before you begin**  
Before you begin, verify that you set up and configured AWS accounts in AWS Organizations\. For more information, see [ in the *AWS Organizations User Guide*\.](https://docs.aws.amazon.com/organizations/latest/userguide/rgs_getting-started.html)

Also, be aware that you must create the organization\-based resource data sync for each AWS Region and account defined in AWS Organizations\. 

### Creating a central S3 bucket<a name="sysman-inventory-datasync-before-you-begin"></a>

Use the following procedure to create a central S3 bucket to store aggregated inventory data\. The procedure describes how to assign a bucket policy that enables Systems Manager to write inventory data to the bucket from your AWS Organizations account ID\. If you already have an S3 bucket that you want to use to aggregate inventory data for Resource Data Sync, then you must configure the bucket to use the policy in the following procedure\.

**To create and configure an S3 bucket for resource data sync for multiple accounts defined in AWS Organizations**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated inventory data\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *DOC\-EXAMPLE\-BUCKET* and *organization\-id* with the name of the Amazon S3 bucket you created and a valid AWS Organizations account ID\.

   Optionally, replace *bucket\-prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *bucket\-prefix/* from the ARN in the following policy\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "SSMBucketPermissionsCheck",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:GetBucketAcl",
               "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET"
           },
           {
               "Sid": " SSMBucketDelivery",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObject",
               "Resource": [
                   "arn:aws:s3:::DOC-EXAMPLE-BUCKET/bucket-prefix/*/accountid=*/*"
               ],
               "Condition": {
                   "StringEquals": {
                       "s3:x-amz-acl": "bucket-owner-full-control",
                       "s3:RequestObjectTag/OrgId": "organization-id"
                   }
               }
           },
           {
               "Sid": " SSMBucketDeliveryTagging",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObjectTagging",
               "Resource": [
                   "arn:aws:s3:::DOC-EXAMPLE-BUCKET/bucket-prefix/*/accountid=*/*"
               ]
           }
       ]
   }
   ```
**Note**  
The Asia Pacific Region came online in April 25, 2019\. If you create a resource data sync for an AWS Region that came online since the Asia Pacific \(Hong Kong\) Region \(ap\-east\-1\) or later, then you must enter a region\-specific service principal entry in the `SSMBucketDelivery` section\. The following example includes a region\-specific service principal entry for `ssm.ap-east-1.amazonaws.com`\.   

   ```
   {
            "Sid":" SSMBucketDelivery",
            "Effect":"Allow",
            "Principal":{
               "Service":["ssm.amazonaws.com","ssm.ap-east-1.amazonaws.com"]
            },
   ```

### Create an inventory Resource Data Sync for accounts defined in AWS Organizations<a name="systems-manager-inventory-resource-data-sync-AWS-Organizations-create"></a>

The following procedure describes how to use the AWS CLI to create a resource data sync for accounts that are defined in AWS Organizations\. You must use the AWS CLI to perform this task\. You must also perform this procedure for each AWS Region and account defined in AWS Organizations\.

**To create a resource data sync for accounts defined in AWS Organizations \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a resource data sync for multiple accounts defined in AWS Organizations\. For *DOC\-EXAMPLE\-BUCKET*, specify the name of the Amazon S3 bucket you created earlier in this topic\. If you created a prefix \(subdirectory\) for your bucket, then specify this information for *prefix\-name*\. 

   ```
   aws ssm create-resource-data-sync --sync-name name --s3-destination "BucketName=DOC-EXAMPLE-BUCKET,Prefix=prefix-name,SyncFormat=JsonSerDe,Region=AWS Region, for example us-east-2,DestinationDataSharing={DestinationDataSharingType=Organization}"
   ```