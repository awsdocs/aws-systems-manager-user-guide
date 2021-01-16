# Exporting OpsData from Systems Manager Explorer<a name="Explorer-exporting-OpsData"></a>

When you click a link in Systems Manager Explorer, some pages display OpsData in a list\. These pages include an **Export** button that enables you to export up to 5,000 OpsData items as a comma separated value \(\.csv\) file to an Amazon Simple Storage Service \(Amazon S3\) bucket\. Before you can export data from Explorer pages, you must configure data export as described in this topic\.

![\[Exporting OpsData from Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-data-export.png)

**Before You Begin**  
When you configure data export, you must specify an Amazon Simple Notification Service \(Amazon SNS\) topic that exists in the same AWS Region where you want to export the data\. Systems Manager sends a notification to the Amazon SNS topic when an export completes\. For information about creating an Amazon SNS topic, see [Tutorial: Creating an Amazon SNS Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html)\.

Also, be aware that when you export Explorer data, Systems Manager creates an AWS Identity and Access Management \(IAM\) role named `AmazonSSMExplorerExportRole`\. This role uses the following IAM policy\.

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

1. Choose **Settings**\.

1. In the **Configure data export** section, choose **Edit**\.

1. To upload the data export file to an existing S3 bucket, choose **Select an existing S3 bucket** and then choose the bucket from the list\. To upload the data export file to a new S3 bucket, choose **Create a new S3 bucket**, and then enter the name you want to use for the new bucket\.

1. Use the **Select an Amazon SNS topic ARN** list to choose the topic you want to notify when the export completes\.

1. Choose **Create**\.

You can now export OpsData from Explorer pages to the specified S3 bucket\.

If you can't export data by using this procedure, verify that your IAM user account, group, or role includes the `iam:CreatePolicyVersion` and `iam:DeletePolicyVersion` actions\. For information about adding these actions to your user account, group, or role, see [Editing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html) in the *IAM User Guide*\.