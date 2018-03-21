# Create a Resource Data Sync for Inventory<a name="sysman-inventory-datasync-create"></a>

Use the following procedure to create a Resource Data Sync for Inventory by using the Amazon EC2 console\. 

**Note**  
You can use AWS Key Management Service \(AWS Key Management Service\) to encrypt the data sent by Systems Manager to the centralized Amazon S3 bucket\. Currently, you can only configure encryption by using the AWS CLI\. For an example of how to create an encrypted sync by using the AWS CLI and how to work with the centralized data in Amazon Athena and Amazon QuickSight, see [Walkthrough: Use Resource Data Sync to Aggregate Inventory Data](sysman-inventory-resource-data-sync.md)\. 

**To create and configure an Amazon S3 Bucket for Resource Data Sync**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *Bucket\-Name* and *Account\-ID* with the name of the Amazon S3 bucket you created and a valid AWS account ID\. Optionally, replace *Bucket\-Prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *Bucket\-Prefix*/ from the ARN in the policy\. 

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
               "Resource": "arn:aws:s3:::Bucket-Name"
           },
           {
               "Sid": " SSMBucketDelivery",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObject",
               "Resource": ["arn:aws:s3:::Bucket-Name/Bucket-Prefix/*/accountid=Account_ID_number/*"],
               "Condition": {
                   "StringEquals": {
                       "s3:x-amz-acl": "bucket-owner-full-control"
                   }
               }
           }
       ]
   }
   ```

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a Resource Data Sync \(AWS Systems Manager\)**

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

1. Choose **Create**\.

**To create a Resource Data Sync \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and choose **Managed Instances**\.

1. Choose **Resource Data Syncs**, and then choose **Create a Resource Data Sync**\.

1. In the **Sync Name** field, type a name for the sync configuration\.

1. In the **Bucket Name** field, type the name of the Amazon S3 bucket you created at the start of this procedure\.

1. \(Optional\) In the **Bucket Prefix** field, type the name of an Amazon S3 bucket prefix \(subdirectory\)\.

1. In the **Bucket Region** field, choose **This region** if the Amazon S3 bucket you created is located in the current AWS Region\. If the bucket is located in a different AWS Region, choose **Another region**, and type the name of the Region\.
**Note**  
If the sync and the target Amazon S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com//s3/pricing/)\.

1. Choose **Create**\.