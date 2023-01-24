# Configure DevOps Guru with Quick Setup<a name="quick-setup-devops"></a>

You can quickly configure DevOps Guru options by using Quick Setup\. Amazon DevOps Guru is a machine learning \(ML\) powered service that makes it easy to improve an application's operational performance and availability\. DevOps Guru detects behaviors that are different from normal operating patterns so you can identify operational issues long before they impact your customers\. DevOps Guru automatically ingests operational data from your AWS applications and provides a single dashboard to visualize issues in your operational data\. You can get started with DevOps Guru to improve application availability and reliability with no manual setup or machine learning expertise\.

Configuring DevOps Guru with Quick Setup is available in the following AWS Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(Oregon\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(Stockholm\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)

For pricing information, see [Amazon DevOps Guru pricing](http://aws.amazon.com/devops-guru/pricing/)\.

To set up DevOps Guru, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To set up DevOps Guru with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **DevOps Guru**, and then choose **Next**\.

1. In the **Configuration options** section, choose the AWS resource types you want to analyze and your notification preferences\.

   If you don't select the **Analyze all AWS resources in all the accounts in my organization** option, you can choose AWS resources to analyze later in the DevOps Guru console\. DevOps Guru analyzes different AWS resource types \(such as Amazon Simple Storage Service \(Amazon S3\) buckets and Amazon Elastic Compute Cloud \(Amazon EC2\) instances\), which are categorized into two pricing groups\. You pay for the AWS resource hours analyzed, for each active resource\. A resource is only active if it produces metrics, events, or log entries within an hour\. The rate you're charged for a specific AWS resource type depends on the price group\.

   If you select the **Enable SNS notifications** option, an Amazon Simple Notification Service \(Amazon SNS\) topic is created in each AWS account in the organizational units \(OUs\) you target with your configuration\. DevOps Guru uses the topic to notify you about important DevOps Guru events, such as the creation of a new insight\. If you don't enable this option, you can add a topic later in the DevOps Guru console\.

   If you select the **Enable AWS Systems Manager OpsItems** option, operational work items \(OpsItems\) will be created for related Amazon EventBridge events and Amazon CloudWatch alarms\.

1. In the **Schedule** section, choose how frequently you want Quick Setup to remediate changes made to resources that differ from your configuration\. The **Default** option runs once\. If you don't want Quick Setup to remediate changes made to resources that differ from your configuration, choose **Disabled** under **Custom**\.

1. In the **Targets** section, choose whether to allow DevOps Guru to analyze resources in some of your organizational units \(OUs\), or the account you're currently logged in to\.

   If you choose **Custom**, continue to step 8\.

   If you choose **Current account**, continue to step 9\.

1. In the **Target OUs** and **Target Regions** sections, select the check boxes of the OUs and Regions where you want to use DevOps Guru\.

1. Choose the Regions where you want to use DevOps Guru in the current account\.

1. Choose **Create**\.