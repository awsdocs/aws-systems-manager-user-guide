# Configuring Resource Data Sync for Inventory<a name="sysman-inventory-datasync"></a>

You can use Systems Manager Resource Data Sync to send Inventory data collected from all of your managed instances to a single Amazon S3 bucket\. Resource Data Sync then automatically updates the centralized data when new Inventory data is collected\. With all Inventory data stored in a target Amazon S3 bucket, you can use services like Amazon Athena and Amazon QuickSight to query and analyze the aggregated data\.

For example, say that you've configured Inventory to collect data about the operating system \(OS\) and applications running on a fleet of 150 managed instances\. Some of these instances are located in a hybrid data center, and others are running in Amazon EC2 across multiple AWS Regions\. If you have *not* configured Resource Data Sync for Inventory, you either need to manually gather the collected inventory data for each instance, or you have to create scripts to gather this information\. You would then need to port the data into an application so that you can run queries and analyze it\.

With Resource Data Sync, you perform a one\-time operation that synchronizes all Inventory data from all of your managed instances\. After the sync is successfully created, Systems Manager creates a baseline of all Inventory data and saves it in the target Amazon S3 bucket\. When new inventory data is collected, Systems Manager automatically updates the data in the Amazon S3 bucket\. You can then quickly and cost\-effectively port the data to Amazon Athena and Amazon QuickSight\.

Diagram 1 shows how Resource Data Sync aggregates inventory data from managed instances in Amazon EC2 and a hybrid environment to a target Amazon S3 bucket\. This diagram also shows how Resource Data Sync works with multiple AWS accounts and AWS Regions\.

**Diagram 1: Resource Data Sync with Multiple AWS Accounts and AWS Regions**

![\[Systems Manager Resource Data Sync architecture\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssm-inventory-datasync-vsd.png)

If you delete a managed instance, Resource Data Sync preserves the Inventory file for the deleted instance\. For running instances, however, Resource Data Sync automatically overwrites old inventory files when new files are created and written to the Amazon S3 bucket\. If you want to track inventory changes over time, you can use the AWS Config service to track the MangagedInstanceInventory resource type\. For more information, see [Getting Started with AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/getting-started.html)\.

## Create a Resource Data Sync for Inventory<a name="sysman-inventory-datasync-create"></a>

Use the following procedure to create a Resource Data Sync for Inventory by using the Amazon S3 and AWS Systems Manager consoles\. You can also use AWS CloudFormation to create or delete a Resource Data Sync\. To use AWS CloudFormation, add the [AWS::SSM::ResourceDataSync](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-resourcedatasync.html) resource to your AWS CloudFormation template\. For information, see one of the following documentation resources:
+ [AWS CloudFormation resource for Resource Data Sync in AWS Systems Manager](https://aws.amazon.com//blogs/mt/aws-cloudformation-resource-for-resource-data-sync-in-aws-systems-manager/) \(blog\)
+ [Working with AWS CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt Inventory data in the Amazon S3 bucket\. For an example of how to create an encrypted sync by using the AWS CLI and how to work with the centralized data in Amazon Athena and Amazon QuickSight, see [Walkthrough: Use Resource Data Sync to Aggregate Inventory Data](sysman-inventory-resource-data-sync.md)\. 

**To create and configure an Amazon S3 Bucket for Resource Data Sync**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *bucket\-name* and *account\-id* with the name of the Amazon S3 bucket you created and a valid AWS account ID\.

   To enable multiple AWS accounts to send inventory data to the central Amazon S3 bucket, specify each account in the policy as shown in the following `Resource` sample:

   ```
   "Resource": [
        "arn:aws:s3:::MyTestS3Bucket/*/accountid=123456789012/*", 
        "arn:aws:s3:::MyTestS3Bucket/*/accountid=a1b2c3d4e5f6/*",
        "arn:aws:s3:::MyTestS3Bucket/*/accountid=1234abcd56ef/*"
                   ],
   ```

   Optionally, replace *bucket\-prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *bucket\-prefix*/ from the ARN in the following policy\. 
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
            "Resource":"arn:aws:s3:::bucket-name"
         },
         {
            "Sid":" SSMBucketDelivery",
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"s3:PutObject",
            "Resource":[
               "arn:aws:s3:::bucket-name/bucket-prefix/*/accountid=account-id-1/*",
               "arn:aws:s3:::bucket-name/bucket-prefix/*/accountid=account-id-2/*",
               "arn:aws:s3:::bucket-name/bucket-prefix/*/accountid=account-id-3/*"
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

**To create a Resource Data Sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose **Resource Data Syncs**, and then choose **Create resource data sync**\.

1. In the **Sync name** field, type a name for the sync configuration\.

1. In the **Bucket name** field, type the name of the Amazon S3 bucket you created at the start of this procedure\.

1. \(Optional\) In the **Bucket prefix** field, type the name of an Amazon S3 bucket prefix \(subdirectory\)\.

1. In the **Bucket region** field, choose **This region** if the Amazon S3 bucket you created is located in the current AWS Region\. If the bucket is located in a different AWS Region, choose **Another region**, and type the name of the Region\.
**Note**  
If the sync and the target Amazon S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com//s3/pricing/)\.

1. In the **KMS Key ARN** field, type or paste a KMS Key ARN to encrypt inventory data in Amazon S3\.

1. Choose **Create**\.

To synchronize inventory data from multiple AWS Regions, you must create a Resource Data Sync in *each* Region\. Repeat this procedure in each AWS Region where you want to collect inventory data and send it to the central Amazon S3 bucket\. When you create the sync in each Region, specify the central Amazon S3 bucket in the **Bucket name** field\. Then use the **Bucket region** option to choose the Region where you created the central Amazon S3 bucket, as shown in the following screen shot\. The next time the association runs to collect inventory data, Systems Manager stores the data in the central Amazon S3 bucket\. 

![\[Systems Manager Resource Data Sync from multiple AWS Regions\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-rds-multiple-regions.png)