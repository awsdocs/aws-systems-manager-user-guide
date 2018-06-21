# Create a Resource Data Sync for Inventory<a name="sysman-inventory-datasync-create"></a>

Use the following procedure to create a Resource Data Sync for Inventory by using the Amazon S3 and AWS Systems Manager consoles\. You can also use AWS CloudFormation to create or delete a Resource Data Sync\. To use AWS CloudFormation, add the [AWS::SSM::ResourceDataSync](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-resourcedatasync.html) resource to your AWS CloudFormation template\. For information, see [Working with AWS CloudFormation Templates](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*\.

**Note**  
You can use AWS Key Management Service \(AWS KMS\) to encrypt Inventory data in the Amazon S3 bucket\. For an example of how to create an encrypted sync by using the AWS CLI and how to work with the centralized data in Amazon Athena and Amazon QuickSight, see [Walkthrough: Use Resource Data Sync to Aggregate Inventory Data](sysman-inventory-resource-data-sync.md)\. 

**To create and configure an Amazon S3 Bucket for Resource Data Sync**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *bucket\-name* and *account\-id* with the name of the Amazon S3 bucket you created and a valid AWS account ID\. Optionally, replace *bucket\-prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *bucket\-prefix*/ from the ARN in the following policy\. 
**Note**  
For information about viewing your AWS account ID, see [Your AWS Account ID and Its Alias](http://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

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
               "Resource": "arn:aws:s3:::bucket-name"
           },
           {
               "Sid": " SSMBucketDelivery",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObject",
               "Resource": ["arn:aws:s3:::bucket-name/bucket-prefix/*/accountid=account-id/*"],
               "Condition": {
                   "StringEquals": {
                       "s3:x-amz-acl": "bucket-owner-full-control"
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