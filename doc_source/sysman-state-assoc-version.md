# Edit and Create a New Version of an Association \(Console\)<a name="sysman-state-assoc-version"></a>

You can edit an association to specify a new name, schedule, or targets\. You can also choose to write the output of the command to an Amazon S3 bucket\. After you edit an association, Systems Manager creates a new version\. You can view different versions after editing, as described in the following procedure\.

**Note**  
This procedure requires that you have write access to an existing S3 bucket\. If you have not used S3 before, be aware that you will incur charges for using S3\. For information about how to create a bucket, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To edit a State Manager association \(AWS Systems Manager\)**

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose the association you created in the previous procedure, and then choose **Edit**\.

1. In the **Name** field, type a new name\. For example, type **TestHostnameAssociation2**\.

1. In the **Specify schedule** section, choose a new option\. For example, choose **CRON schedule builder**, and then choose **Every 1 hour**\.

1. \(Optional\) To write the command output to an Amazon S3 bucket, do the following in the **Output options** section: 
   + Choose **Enable writing output to S3**\.
   + In the **S3 bucket name** field, type the name of an S3 bucket you have write access to\.
   + \(Optional\) To write output to a folder in the bucket, type its name in the **S3 key prefix** field\. If no folder exists with the name you specify, State Manager creates it for you\.

1. Choose **Edit association**\.

1. In the **Associations** page, choose the name of the association you just edited, and then choose the **Versions** tab\. The system lists each version of the association you created and edited\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the S3 bucket you specified for storing command output, and then choose the folder named with the ID of the instance that ran the association\. \(If you chose to store output in a folder in the bucket, open it first\.\)

1. Drill down several levels, through the `awsrunPowerShell` folder, to the `stdout` file\.

1. Choose **Open** or **Download** to view the host name\.

**To edit a State Manager association \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **State Manager**\.

1. Choose the association you created in the previous procedure\.

1. From the **Actions** menu, choose **Edit Association**\.

1. In the **Name** field, type a new name\. For example, type **TestHostnameAssociation2**\.

1. In the **Schedule** section, choose a new option\. For example, choose **Cron schedule builder**, and then choose **Every 1 hour**\.

1. In the **Advanced** section, choose **Write to S3**\.

1. Disregard the **S3Region** field\. This field is deprecated\. Specify the name of your bucket in the **S3Bucket Name** field\. If want to write output to a sub\-folder, specify the sub folder name in the **S3Key Prefix** field\. 

1. Choose **Edit Association**, and then choose **Close**\.

1. In the State Manager page, choose the association you just edited, and then choose the **Versions** tab\. The system lists each version of the association you created and edited\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose your bucket name, and then choose an instance ID that ran the association\.

1. Drill down several levels, through the `awsrunPowerShell` folder, to the `stdout` file\.

1. Choose the file, and then choose the link in the **Link** section\. The host name is listed in the **HostId** tags\.