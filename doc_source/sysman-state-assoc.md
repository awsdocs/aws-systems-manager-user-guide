# Create an Association \(Console\)<a name="sysman-state-assoc"></a>

This section describes how to create a State Manager association by using the AWS Systems Manager console\. To view an example of how to create an association by using the AWS CLI, see [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.

**To create a State Manager association \(Console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\. A name can help you understand the purpose of the association when you created it\. For example, you could specify **Automatically\_update\_AWSPVDrivers\_on\_us\-west\-2\_instances** for an association with that purpose\. Spaces aren't allowed in the name\.

1. In the **Document** list, choose the option beside a document name\. You can use the numbers to the right of the Search bar to view more documents\. 
**Note**  
You can view information about a document by choosing the document name\.

1. In the **Parameters** section, specify the required input parameters\.

1. In the **Targets** section, choose either **Specifying tags** or **Manually Selecting Instance**\. If you choose to target resources by using tags, then enter a tag key and a tag value in the fields provided\. For more information about using targets, see [Using Targets and Rate Controls with State Manager Associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, then use the buttons provided to create a cron or rate schedule for the association\. 

1. In the **Advanced options** section:
   + In **Compliance severity**, choose a severity level for the association\. Compliance reporting will indicate whether the association state is compliant or non\-compliant, along with the severity level you indicate here\. For more information, see [About State Manager Association Compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, configure options for executing State Manager associations across of fleet of managed instances\. For more information about these options, see [Using Targets and Rate Controls with State Manager Associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can execute the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can execute the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before State Manager stops executing associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors allowed before State Manager stops executing associations on additional targets\.

1. In the **Output options** section, choose **Enable writing output to S3** if you want to write the output of the command to create the associations to an Amazon S3 bucket\.

1. Choose **Create Association**\. 

State Manager creates and immediately executes the association on the specified instances or targets\. After the initial execution, the association runs in intervals according to the schedule that you defined and according to the following rules:
+ Associations are only executed on instances that are online when the interval starts\. Offline instances are skipped\.
+ State Manager attempts to execute the association on all configured instances during an interval\.
+ If an association is not executed during an interval \(because, for example, a concurrency value limited the number of instances that could process the association at one time\), then State Manager attempts to execute the association during the next interval\.
+ State Manager records history for all skipped intervals\. You can view the history on the **Execution History** tab\.