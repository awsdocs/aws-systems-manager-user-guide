# Using Quick Setup<a name="quick-setup-using"></a>

Quick Setup, a capability of AWS Systems Manager, displays the results of each configuration in the **Configurations** table on the Quick Setup home page\. From this page, you can **View details** of each configuration, delete configurations from the **Actions** drop down, or **Create** configurations\. The **Configurations** table contains the following information:
+ **Configuration type** – The configuration type chosen when creating the configuration\. 
+ **Deployment type** – The AWS account and AWS Regions that you deployed the configuration to\.
+ **Organizational units** – Displays the organizational units \(OUs\) that the configuration is deployed to if you chose a **Custom** set of targets\. Organizational units and custom targets are only available to the management account of your organization\.
+ **Regions** – The Regions that the configuration is deployed to if you chose a **Custom** set of targets or targets within your **Current account**\. 
+ **Deployment status** – The deployment status indicates if AWS CloudFormation successfully deployed the target or stack instance\. The target and stack instances contain the configuration options that you chose during configuration creation\.
+ **Association status** – The association status is the state of all associations created by the configuration that you created\. The associations for all targets must run successfully; otherwise, the status is **Failed**\.

  Quick Setup creates and runs a State Manager association for each configuration target\. State Manager is a capability of AWS Systems Manager\.

## Configuration details<a name="quick-setup-details"></a>

The **Configuration details** page displays information about the deployment of the configuration and its related associations\. From this page, you can edit configuration options, update targets, or delete the configuration\. You can also view the details of each configuration deployment to get more information about the associations\. 

Each configuration type has some of the following status graphs:

**Configuration deployment status**  
Displays the number of deployments that have succeeded, failed, or are running or pending\. Deployments occur in the specified target accounts and Regions that contain nodes affected by the configuration\. 

**Configuration association status**  
Displays the number of State Manager associations that have succeeded, failed, or are pending\. Quick Setup creates an association in each deployment for the configuration options selected\.

**Setup status**  
Displays the number of actions performed by the configuration type and their current statuses\. 

**Resource compliance**  
Displays the number of resources that are compliant to the configurations specified policy\.

The **Configuration details** table displays information about the deployment of your configuration\. You can view more details about each deployment by selecting the deployment and then choosing **View details**\. The details page of each deployment displays the associations deployed to the nodes in that deployment\.

## Editing and deleting your configuration<a name="quick-setup-edit-delete"></a>

You can edit configuration options of a configuration from the **Configuration details** page by choosing **Actions** and then **Edit configuration options**\. When you add new options to the configuration, Quick Setup runs your deployments and creates new associations\. When you remove options from a configuration, Quick Setup runs your deployments and removes any related associations\.

**Note**  
You can edit Quick Setup configurations for your account at anytime\. To edit an **Organization** configuration, the **Configuration status** must be **Success** or **Failed**\. 

You can also update the targets included in your configurations by choosing **Actions** and **Add OUs**, **Add Regions**, **Remove OUs**, or **Remove Regions**\. If you're account isn't configured as the management account or you created the configuration for only the current account, you can't update the target organizational units \(OUs\)\. Removing a Region or OU removes the associations from those Regions or OUs\. 

You can delete a configuration from Quick Setup by choosing the configuration, then **Actions**, and then **Delete configuration**\. Or, you can delete the configuration from the **Configuration details** page under the **Actions** dropdown and then **Delete configuration**\. Quick Setup then prompts you to **Remove all OUs and Regions** which might take some time to complete\. Deleting a configuration also deletes all related associations\. This two\-step deletion process removes all deployed resources from all accounts and Regions and then deletes the configuration\.

## Configuration compliance<a name="quick-setup-compliance"></a>

You can view whether your instances are compliant with the associations created by your configurations in either Explorer or Compliance, which are both capabilities of AWS Systems Manager\. To learn more about compliance, see [Working with Compliance](sysman-compliance-about.md)\. To learn more about viewing compliance in Explorer, see [AWS Systems Manager Explorer](Explorer.md)\.

## Troubleshooting Quick Setup results<a name="quick-setup-results-troubleshooting"></a>

**Failed deployment**  
A deployment fails if the CloudFormation stack set failed during creation\. Use the following steps to investigate a deployment failure\.

1. Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\. 

1. Choose the stack created by your Quick Setup configuration\. The **Stack name** includes `QuickSetup` followed by the type of configuration you chose, such as `SSMHostMgmt`\. 
**Note**  
CloudFormation sometimes deletes failed stack deployments\. If the stack isn't available in the **Stacks** table, choose **Deleted** from the filter list\.

1. View the **Status** and **Status reason**\. For more information about stack statuses, see [Stack status codes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html#cfn-console-view-stack-data-resources-status-codes) in the *AWS CloudFormation User Guide*\. 

1. To understand the exact step that failed, view the **Events** tab and review each event's **Status**\. 

1. Review [Troubleshooting](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) in the *AWS CloudFormation User Guide*\.

1. If you are unable to resolve the deployment failure using the CloudFormation troubleshooting steps, delete the configuration and reconfigure it\.

**Failed association**  
The **Configuration details** table on the **Configuration details** page of your configuration shows a **Configuration status** of **Failed** if any of the associations failed during set up\. Use the following steps to troubleshoot a failed association\.

1. In the **Configuration details** table, choose the failed configuration and then choose **View Details**\.

1. Copy the **Association name**\.

1. Navigate to **State Manager** and paste the association name into the search field\. 

1. Choose the association and choose the **Execution history** tab\.

1. Under **Execution ID**, choose the association execution that failed\.

1. The **Association execution targets** page lists all of the nodes where the association ran\. Choose the **Output** button for an execution that failed to run\.

1. In the **Output** page, choose **Step \- Output** to view the error message for that step in the command execution\. Each step can display a different error message\. Review the error messages for all steps to help troubleshoot the issue\.
If viewing the step output doesn't solve the problem, then you can try to recreate the association\. To recreate the association, first delete the failing association in State Manager\. After deleting the association, edit the configuration and choose the option you deleted and choose **Update**\.  
To investigate **Failed** associations for an **Organization** configuration, you must sign in to the account with the failed association and use the following failed association procedure, previously described\. The **Association ID** isn't a hyperlink to the target account when viewing results from the management account\.

**Drift status**  
When viewing a configuration's details page, you can view the drift status of each deployment\. Configuration drift occurs whenever a user makes any change to a service or feature that conflicts with the selections made through Quick Setup\. If an association has changed after the initial configuration, the table displays a warning icon that indicates the number of items that have drifted\. You can determine what caused the drift by hovering over the icon\. 
When an association is deleted in State Manager, the related deployments display a drift warning\. To fix this, edit the configuration and choose the option that was removed when the association was deleted\. Choose **Update** and wait for the deployment to complete\.