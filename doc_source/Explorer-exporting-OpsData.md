# Exporting OpsData from Systems Manager Explorer<a name="Explorer-exporting-OpsData"></a>

You can export 5,000 OpsData items as a comma separated value \(\.csv\) file to an Amazon Simple Storage Service \(Amazon S3\) bucket from AWS Systems Manager Explorer\. Explorer uses the **Export Ops Data to S3** automation runbook to export OpsData\. When you export OpsData, the system displays the automation runbook page where you can specify details, such as assumeRole, Amazon S3 bucket name, SNS topic Arn, and Column fields to be exported\. 

For information about the `AWS-ExportOpsDataToS3` automation runbook, see [https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-aws-exportopsdatatos3.html](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-aws-exportopsdatatos3.html)\. 

**Topics**
+ [Step 1: Specifying an SNS topic](#Explorer-specify-SNS-topic)
+ [Step 2: \(Optional\) Configuring data export](#Explorer-configure-data-export)
+ [Step 3: Exporting OpsData](#Explorer-export-OpsData)

## Step 1: Specifying an SNS topic<a name="Explorer-specify-SNS-topic"></a>

When you configure data export, you must specify an Amazon Simple Notification Service \(Amazon SNS\) topic that exists in the same AWS Region where you want to export the data\. Systems Manager sends a notification to the Amazon SNS topic when an export is complete\. For information about creating an Amazon SNS topic, see [Creating an Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html)\.

## Step 2: \(Optional\) Configuring data export<a name="Explorer-configure-data-export"></a>

You can configure data export settings from the **Settings** or **Export Ops Data to S3 Bucket** page\. 

**To configure data export from Explorer**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Configure data export** section, choose **Edit**\.

1. To upload the data export file to an existing Amazon S3 bucket, choose **Select an existing S3 bucket** and choose the bucket from the list\.

   To upload the data export file to a new Amazon S3 bucket, choose **Create a new S3 bucket** and enter the name that you want to use for the new bucket\.
**Note**  
You can only edit the Amazon S3 bucket name and Amazon SNS topic ARN from the page where you configured those settings for the first time in Explorer\. If you set up the Amazon S3 bucket and the Amazon SNS topic ARN from the **Settings** page, then you can only modify those settings from the **Settings** page\. 

1. For **Select an Amazon SNS topic ARN**, choose the topic that you want to notify when the export is complete\.

1. Choose **Create**\.

## Step 3: Exporting OpsData<a name="Explorer-export-OpsData"></a>

When you export Explorer data, Systems Manager creates an AWS Identity and Access Management \(IAM\) role named `AmazonSSMExplorerExportRole`\. This role uses the following IAM policy\.

```
{
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject"
              ],
              "Resource": [
                 "arn:aws:s3:::{{ExportDestinationS3BucketName}}/*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "s3:GetBucketAcl"
              ],
              "Resource": [
                 "arn:aws:s3:::{{ExportDestinationS3BucketName}}"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "sns:Publish"
              ],
              "Resource": [
                  "{{SnsTopicArn}}"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "logs:DescribeLogGroups",
                  "logs:DescribeLogStreams"
              ],
              "Resource": [
                  "*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "logs:CreateLogGroup",
                  "logs:PutLogEvents",
                  "logs:CreateLogStream"
              ],
              "Resource": [
                  "*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "ssm:GetOpsSummary"
              ],
              "Resource": [
                  "*"
              ]
          }
      ]
  }
```

The role includes the following trust entity\.

```
{
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "Service": "ssm.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
```

**To export OpsData from Explorer**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Export Table**\.
**Note**  
When you export OpsData for the first time, the system creates an assume role for the export\. You can't modify the default assume role\. 

1. For **Amazon S3 Bucket Name**, choose an existing bucket\. You can choose **Create** to create an Amazon S3 bucket if needed\. If you can't change the S3 bucket name, it means that you configured the bucket name from the **Settings** page\. You can only change the bucket name from the **Settings** page\.
**Note**  
You can only edit the Amazon S3 bucket name and Amazon SNS topic ARN from the page where you configured those settings for the first time in Explorer\. 

   

1. For **SNS Topic Arn**, choose an existing Amazon SNS topic ARN to notify when the download completes\. 

   If you can't change the Amazon SNS topic ARN, it means that you configured the Amazon SNS topic ARN from the **Settings** page\. You can only change the topic ARN from the **Settings** page\.

   

1. \(Optional\) For **SNS Success Message**, specify a success message that you want to display when the export is successfully completed\.

1. Choose **Submit**\. The system navigates to the previous page and displays the message **Click to view status of export process\. View details**\. 

   You can choose **View details** to view the status of the runbook and progress in Systems Manager Automation\.

You can now export OpsData from Explorer to the specified Amazon S3 bucket\.

If you can't export data by using this procedure, verify that your user, group, or role includes the `iam:CreatePolicyVersion` and `iam:DeletePolicyVersion` actions\. For information about adding these actions to your user, group, or role, see [Editing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html) in the *IAM User Guide*\.