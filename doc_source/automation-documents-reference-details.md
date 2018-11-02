# Systems Manager Automation Document Details Reference<a name="automation-documents-reference-details"></a>

This section includes topics that describe each of the Systems Manager Automation documents that are owned by AWS and AWS Support\. Each page provides an explanation of the required and optional parameters you can specify when using the document\. Each page also lists the steps in the document and the output of the execution, if any\. 

You can view the JSON for these document in the Systems Manager console\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a document, and then choose **View details**\.

1. Choose the **Content** tab\.

**Note**  
This section does *not* include a separate page for documents that require approval such as the AWS\-CreateManagedLinuxInstanceWithApproval or AWS\-StopEC2InstanceWithApproval document\. Any document name that includes *WithApproval*, means the document includes the [aws:approve](automation-actions.md#automation-action-approve) action\. This action temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. 

**Topics**
+ [AWSSupport\-ActivateWindowsWithAmazonLicense](automation-awssupport-activatewindowswithamazonlicense.md)
+ [AWS\-ASGEnterStandby](automation-aws-asgenterstandby.md)
+ [AWS\-ASGExitStandby](automation-aws-asgexitstandby.md)
+ [AWS\-AttachEBSVolume](automation-aws-attachebsvolume.md)
+ [AWS\-AttachIAMToInstance](automation-aws-attachiamtoinstance.md)
+ [AWS\-ConfigureCloudWatchOnEC2Instance](automation-aws-configurecloudwatchonec2instance.md)
+ [AWS\-ConfigureS3BucketLogging](automation-aws-configures3bucketlogging.md)
+ [AWS\-Configures3BucketVersioning](automation-aws-configures3bucketversioning.md)
+ [AWS\-CopySnapshot](automation-aws-copysnapshot.md)
+ [AWS\-CreateDynamoDBBackup](automation-aws-createdynamodbbackup.md)
+ [AWS\-CreateImage](automation-aws-createimage.md)
+ [AWS\-CreateManagedLinuxInstance](automation-aws-createmanagedlinuxinstance.md)
+ [AWS\-CreateManagedWindowsInstance](automation-aws-createmanagedwindowsinstance.md)
+ [AWS\-CreateSnapshot](automation-aws-createsnapshot.md)
+ [AWS\-DeleteCloudFormationStack](automation-aws-deletecloudformationstack.md)
+ [AWS\-DeleteDynamoDBBackup](automation-aws-deletedynamodbbackup.md)
+ [AWS\-DeleteDynamoDBTableBackups](automation-aws-deletedynamodbtablebackups.md)
+ [AWS\-DeleteEBSVolumeSnapshots](automation-aws-deleteebsvolumesnapshots.md)
+ [AWS\-DeleteImage](automation-aws-deleteimage.md)
+ [AWS\-DeleteSnapshot](automation-aws-deletesnapshot.md)
+ [AWS\-DetachEBSVolume](automation-aws-detachebsvolume.md)
+ [AWS\-Disables3BucketPublicReadWrite](automation-aws-disables3bucketpublicreadwrite.md)
+ [AWS\-Enables3BucketEncryption](automation-aws-enables3bucketencryption.md)
+ [AWSSupport\-ExecuteEC2Rescue](automation-awssupport-executeec2rescue.md)
+ [AWSSupport\-GrantPermissionsToIAMUser](automation-awssupport-grantpermissionstoiamuser.md)
+ [AWSSupport\-ManageRDPSettings](automation-awssupport-managerdpsettings.md)
+ [AWSSupport\-ManageWindowsService](automation-awssupport-managewindowsservice.md)
+ [AWS\-PatchASGInstance](automation-aws-patchasginstance.md)
+ [AWS\-RebootRDSInstance](automation-aws-rebootrdsinstance.md)
+ [AWSSupport\-ResetAccess](automation-awssupport-resetaccess.md)
+ [AWS\-ResizeInstance](automation-aws-resizeinstance.md)
+ [AWS\-RestartEC2Instance](automation-aws-restartec2instance.md)
+ [AWSSupport\-SendLogBundleToS3Bucket](automation-awssupport-sendlogbundletos3bucket.md)
+ [AWS\-SetupInventory](automation-aws-setupinventory.md)
+ [AWS\-SetupManagedInstance](automation-aws-setupmanagedinstance.md)
+ [AWS\-SetupManagedRoleOnEC2Instance](automation-aws-setupmanagedroleonec2instance.md)
+ [AWS\-StartEC2Instance](automation-aws-startec2instance.md)
+ [AWSSupport\-StartEC2RescueWorkflow](automation-awssupport-startec2rescueworkflow.md)
+ [AWS\-StartRDSInstance](automation-aws-startrdsinstance.md)
+ [AWS\-StopEC2Instance](automation-aws-stopec2instance.md)
+ [AWS\-StopRDSInstance](automation-aws-stoprdsinstance.md)
+ [AWS\-TerminateEC2Instance](automation-aws-terminateec2instance.md)
+ [AWSSupport\-TroubleshootRDP](automation-awssupport-troubleshootrdp.md)
+ [AWSSupport\-TroubleshootSSH](automation-awssupport-troubleshootssh.md)
+ [AWS\-UpdateCloudFormationStack](automation-aws-updatecloudformationstack.md)
+ [AWS\-UpdateLinuxAmi](automation-aws-updatelinuxami.md)
+ [AWS\-UpdateWindowsAmi](automation-aws-updatewindowsami.md)
+ [AWSSupport\-UpgradeWindowsAWSDrivers](automation-awssupport-upgradewindowsawsdrivers.md)