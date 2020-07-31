# AWS Systems Manager Quick Setup<a name="systems-manager-quick-setup"></a>

Use AWS Systems Manager Quick Setup to quickly configure required security roles and commonly used Systems Manager capabilities on your Amazon EC2 instances\. You can use Quick Setup in an individual account or across multiple accounts and Regions by integrating with AWS Organizations\. These capabilities help you manage and monitor the health of your instances while providing the minimum required permissions to get started\. Specifically, Quick Setup helps you configure the following components on the instances you choose or target by using tags:
+ AWS Identity and Access Management \(IAM\) instance profile roles for Systems Manager\.
+ A scheduled, bi\-weekly update of SSM Agent\.
+ A scheduled collection of Inventory metadata every 30 minutes\.
+ A daily scan of your instances to identify missing patches\.
+ A one\-time installation and configuration of the Amazon CloudWatch agent\.
+ A scheduled, monthly update of the CloudWatch agent\.

**Note**  
If your **Organization** Quick Setup targets an account that has previously run a **Local** Quick Setup, the existing configurations are not changed\. A new set of State Manager associations are created when you run an **Organization** Quick Setup\. This means you could have associations whose configuration options and schedules overlap\.

**Organization** Quick Setup is available in the following AWS Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Canada \(Central\)
+ South America \(São Paulo\)

**Local** Quick Setup is available in all Regions where Systems Manager is supported\. For a list of supported Regions, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

To access Quick Setup, choose **Quick Setup** in the navigation pane of the Systems Manager console\. You can also access Quick Setup by choosing **AWS Systems Manager** at the top of the navigation pane, and then choosing **Get Started with Systems Manager**, as shown in the following image\. To access the **Organization** Quick Setup type, which enables you to target multiple accounts and Regions, you must be logged in to the master account for your organization\. For information on how to get started with AWS Organizations, see [Getting started with AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started.html) in the *AWS Organizations User Guide*\.

![\[Accessing AWS Quick Setup\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-access.png)

**Note**  
You can edit Quick Setup configurations for your account any time\. To edit an **Organization** Quick Setup configuration, the **Configuration status** must be **Success** or **Failed**\. Before editing a Quick Setup configuration, we recommend that you learn how to change configurations by using the Quick Setup Results page\. For more information, see [Working with Quick Setup Results](#quick-setup-results)\.

## Permissions roles<a name="quick-setup-instance-profile"></a>

By default, Systems Manager doesn't have permission to communicate with or perform actions on your instances\. You must grant access by using an AWS Identity and Access Management \(IAM\) instance profile and an IAM service role \(or *assume* role\)\. An instance profile is a container that passes IAM role information to an EC2 instance at launch\. A service role enables Systems Manager to run commands on your instances\. For more information about instance profiles, see [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the *IAM User Guide*\. For more information about service roles, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.

You can choose to have Quick Setup create and configure these roles for you by choosing **Use the default role**\. If you select an existing role, then that role must include IAM policies with, at minimum, the permissions described in this topic\. If you select existing roles and they don't have these permissions, then Quick Setup may fail to configure one or more selected components, or those components may fail to run correctly\.

**Note**  
Quick Setup doesn't override instance profiles that already exist on your instances\.

### Details about the default instance profile<a name="quick-setup-instance-profile-details"></a>

In the **Instance profile role** section, if you choose **Use the default role**, then Quick Setup creates a new IAM instance profile that uses the **AmazonSSMManagedInstanceCore** policy and one additional policy\. The **AmazonSSMManagedInstanceCore** policy enables Systems Manager to perform the following actions on your instances\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeAssociation",
                "ssm:GetDeployablePatchSnapshotForInstance",
                "ssm:GetDocument",
                "ssm:DescribeDocument",
                "ssm:GetManifest",
                "ssm:GetParameter",
                "ssm:GetParameters",
                "ssm:ListAssociations",
                "ssm:ListInstanceAssociations",
                "ssm:PutInventory",
                "ssm:PutComplianceItems",
                "ssm:PutConfigurePackageResult",
                "ssm:UpdateAssociationStatus",
                "ssm:UpdateInstanceAssociationStatus",
                "ssm:UpdateInstanceInformation"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssmmessages:CreateControlChannel",
                "ssmmessages:CreateDataChannel",
                "ssmmessages:OpenControlChannel",
                "ssmmessages:OpenDataChannel"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2messages:AcknowledgeMessage",
                "ec2messages:DeleteMessage",
                "ec2messages:FailMessage",
                "ec2messages:GetEndpoint",
                "ec2messages:GetMessages",
                "ec2messages:SendReply"
            ],
            "Resource": "*"
        }
    ]
}
```

**Note**  
For information about the `ssmmessages*` and `ec2messages*` actions, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

Quick Setup also adds the following policy to the instance profile\. This policy enables trusted communications between the Systems Manager service in the cloud and your Amazon EC2 instances\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Principal":{
            "Service":"ec2.amazonaws.com"
         },
         "Action":"sts:AssumeRole"
      }
   ]
}
```

### Details about the service role<a name="quick-setup-service-role-details"></a>

In the **Systems Manager service role** section, if you choose **Use the default role**, then Quick Setup creates a new IAM service role that includes the following policies\. The first policy enables Systems Manager to perform the following actions on your instances\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateInstanceProfile",
                "iam:ListInstanceProfilesForRole",
                "iam:PassRole",
                "ec2:DescribeIamInstanceProfileAssociations",
                "iam:GetInstanceProfile",
                "ec2:DisassociateIamInstanceProfile",
                "ec2:AssociateIamInstanceProfile",
                "iam:AddRoleToInstanceProfile"
            ],
            "Resource": "*"
        }
    ]
}
```

The following policy enables Systems Manager to perform the actions in the previous policy on your behalf\. Systems Manager assumes your role to perform the actions\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Principal":{
            "Service":"ssm.amazonaws.com"
         },
         "Action":"sts:AssumeRole"
      }
   ]
}
```

**Note**  
Configuring an instance with an instance profile for Systems Manager does not give a user access to run commands or use Systems Manager capabilities on that instance\. Your IAM user, group, or role must be configured with a separate permissions policy that enables you to perform actions on your instances by using Systems Manager\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

## Update Systems Manager \(SSM\) Agent<a name="quick-setup-ssm-agent"></a>

SSM Agent is Amazon software that processes requests from the Systems Manager service in the AWS Cloud, and then runs them on your instance as specified in the request\. SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04
+ Amazon ECS\-Optimized

If you enable this option, then Systems Manager automatically checks every two weeks for a new version of the agent\. If there is a new version, then Systems Manager automatically updates the agent on your instance to the latest released version\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of SSM Agent\. For more information about SSM Agent, including information about how to manually install the agent, see [Working with SSM Agent](ssm-agent.md)\.

## Collect inventory from your instances<a name="quick-setup-inventory"></a>

AWS Systems Manager Inventory provides visibility into your computing environment\. You can use Inventory to collect *metadata* from your managed instances\. You can store this metadata in a central Amazon Simple Storage Service \(Amazon S3\) bucket\. Then use built\-in tools to query the data and quickly determine which instances are running the software and configurations required by your software policy, and which instances need to be updated\. Quick Setup configures collection for the following types of metadata:
+ **AWS components**: EC2 driver, agents, versions, and more\.
+ **Applications**: Application names, publishers, versions, and more\.
+ **Instance details**: System name, operating system \(OS\) name, OS version, last boot, DNS, domain, work group, OS architecture, and more\.
+ **Network configuration**: IP address, MAC address, DNS, gateway, subnet mask, and more\. 
+ **Services**: Name, display name, status, dependent services, service type, start type, and more \(Windows Server instances only\)\.
+ **Windows roles**: Name, display name, path, feature type, installed state, and more \(Windows Server instances only\)\.
+ **Windows updates**: Hotfix ID, installed by, installed date, and more \(Windows Server instances only\)\.

You can configure Systems Manager Inventory to collect the following additional types of metadata from your instances\. For more information, see [AWS Systems Manager Inventory](systems-manager-inventory.md)\.
+ **Custom inventory**: Metadata that was assigned to a managed instance as described in [Working with custom inventory](sysman-inventory-custom.md)\.
+ **Files**: Name, size, version, installed date, modification time, last accessed time, and more\.
+ **Tags**: Tags assigned to your instances\.
+ **Windows Registry**: Registry key path, value name, value type, and value\.

**Important**  
A *global* Inventory configuration is created from the one\-click configuration option on the **Inventory page** and targets all instances in your AWS account\. If you already have a global Inventory configuration enabled on your instances, then enabling Inventory collection in Quick Setup creates another schedule to collect Inventory data for the specified or targeted instances\. There is no conflict\. However, if you already have a non\-global Inventory configuration enabled on some of your instances, then the configuration you created earlier will fail\. Inventory collection does not support more than one specific configuration per instance\. To resolve this issue, either deploy a global inventory configuration or delete the conflict configuration\.

## Scan instances for missing patches daily<a name="quick-setup-patching"></a>

If you enable this option in Quick Setup, then Systems Manager uses Patch Manager to scan your instances each day and generate a simple report in the **Compliance** page\. The report shows how many instances are patch\-compliant according to the *default patch baseline*\. The report includes a list of each instance and its compliance status\. You can navigate this list to see details about noncompliant instances\. For more information about patching operations and patch baselines, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\. To view compliance information, see the Systems Manager [Compliance](https://console.aws.amazon.com/systems-manager/compliance) page\.

## Install and configure the CloudWatch agent<a name="quick-setup-cloudwatch"></a>

Amazon CloudWatch provides data and actionable insights to monitor your applications, understand and respond to system\-wide performance changes, optimize resource utilization, and get a unified view of operational health\. The CloudWatch agent collects metrics and log files from your instances and consolidates this information so that you can quickly determine the health of your instances\. For more information, see [Collecting metrics and logs from EC2 instances and on\-premises servers with the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)\. There may be added cost\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

## Update the CloudWatch agent once every four weeks<a name="quick-setup-cloudwatch-2"></a>

If you enable this option, then Systems Manager automatically checks every four weeks for a new version of the CloudWatch agent\. If there is a new version, then Systems Manager automatically updates the agent on your instance to the latest released version\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of the CloudWatch agent\.

## Choosing Targets for Quick Setup<a name="quick-setup-targets"></a>

After you choose Quick Setup options, choose which instances you want to configure with those options in the **Targets** section\. Quick Setup includes the following options for targeting instances\.

**Local**
+ **Choose all instances in the current AWS account and Region**: Quick Setup locates and applies the configuration options to all instances in the current AWS account and Region\.
+ **Specify instance tags**: Quick Setup uses the tag key and \(optional\) tag value that you specify to locate instances\.
+ **Choose instances manually**: Quick Setup enables you to choose one or more instances from a list\.

**Organization**
+ **Target organizational units \(OUs\)**: Quick Setup enables you to target multiple AWS Organizations organizational units \(OUs\)\. Quick Setup applies your configuration to all accounts within an OU\.
+ **Target Region**: Quick Setup enables you to choose which Regions to target within your **Target organizational units \(OUs\)**\.

**Note**  
You cannot target individual instances or tags when using **Organization** Quick Setup\. All instances in the accounts within the **Target organizational units \(OUs\)** and **Target Region** are configured by Quick Setup\. Quick Setup also applies your configuration to all new instances you launch in the accounts within your **Target organizational units \(OUs\)** and **Target Region**\. You can specify up to 2000 targets with **Organization** Quick Setup\. Targets in an **Organization** Quick Setup equate to AWS CloudFormation stack instances\. This means that a target is a reference to a stack in an account within a Region\. You can get the total number of targets by multiplying the number of accounts by the number of Regions specified\. For more information about AWS CloudFormation stack instances, see [Stack instances](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-concepts.html#stacksets-concepts-stackinstances) in the *AWS CloudFormation User Guide*\.

## Working with Quick Setup Results<a name="quick-setup-results"></a>

Systems Manager displays the results of Quick Setup in a separate card for each option you selected\. 

![\[The results of AWS Quick Setup.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-results-page.png)

For each option you selected, Systems Manager creates and immediately runs a State Manager association\. The **Configuration status** field shows **Success** when the association successfully runs on *all* selected or targeted instances\. If one association fails to run, **Configuration status** is **Failed**\. If an association is processing, **Configuration status** is **Pending**\. To update **Configuration status** for **Pending** items, refresh your browser\.

**Note**  
The **Inventory collection** option can take up to 10 minutes to complete, even if you only selected a few instances\.

A **Configuration status** of **Not configured** means that you didn't choose the option in Quick Setup\. If you see this status for **Managed instances**, it means that you didn't choose a role in the **Role selection** list\. You can run Quick Setup again, as described in this topic, and choose a role\.

To edit the association for a Quick Setup option, choose the **Edit** button in the option card\. If you edit the association, * don't* choose a different SSM document in the **Edit Association** page\. If you choose a different SSM document, the option becomes unavailable in Quick Setup\. Change only the parameters and targets of the association\. When you save your changes to the association, State Manager automatically runs the association\.

To view compliance data for your Quick Setup, choose the **View compliance summary in Explorer** button\. This opens the AWS Systems Manager Explorer dashboard\. For more information about Explorer, see [AWS Systems Manager Explorer](Explorer.md)\.

When viewing Quick Setup results while signed in to the master account for your AWS Organizations organization, you see a summary of the results for your **Organization** Quick Setup in the **Organization** tab\. This view shows the **Applied configuration** for your Quick Setup, allows you to filter results, shows the **Deployment** and **Association** statuses for your targets, and enables you to view more details for individual targets in the **Quick Setup deployments** card\.

**Deployment status** refers to the state of a target or stack instance\. In other words, this status indicates whether AWS CloudFormation successfully deployed your configuration options\. **Deployment status** does not evaluate the status of the associations created in your Quick Setup\.

**Association status** refers to the state of all associations created by your Quick Setup within a target or stack instance\. All associations must successfully run, otherwise the status for the target is **Failed**\.

## Troubleshooting Quick Setup Results<a name="quick-setup-results-troubleshooting"></a>

If a Quick Setup card shows **Not configured**, you might have missed an option on the Quick Setup page\. As a first step in troubleshooting this problem, choose the **Edit all** button at the top of the Quick Setup results page and review your selections\. If you missed one or more options, you can select the options and then choose **Reset** to configure those options\.

If you still see a problem with one or more Quick Setup results cards, then use the following procedure to troubleshoot the issue\.

**Note**  
To investigate **Failed** associations for an **Organization** Quick Setup, you must sign in to the account with the failed association and use the following procedure\. The **Association ID** is not a hyperlink to the target account when viewing results from the master account\.

**To troubleshoot a failed Quick Setup configuration**

1. In the Quick Setup results page, choose **View Details** in the card with a **Configuration status** of **Failed**\.  
![\[Choosing the View Details button in a Quick Setup card\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-1.png)

1. In the **Association ID** page, choose the **Execution history** tab\.

1. Under **Execution ID**, choose the association execution that failed\.  
![\[Choosing an association execution ID\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-2.png)

1. The **Association execution targets** page lists all of the instances where the association ran\. Choose the **Output** button for an execution that failed to run\.  
![\[Choosing the Output button for an association execution that failed to run\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-3.png)

1. In the **Output** page, choose **Step \- Output** to view the error message for that step in the command execution\. Each step can display a different error message\. Review the error messages for all steps to help troubleshoot the issue\.  
![\[Choosing Step Output for an association execution that failed to run\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-4.png)

If viewing the step output doesn't solve the problem, then you can recreate the association to see if the problem persists\. To recreate the association, you must first delete all associations that already exist for the Quick Setup option\. You can delete an association by using the **Delete** button in the Quick Setup results card\. You can also delete the association by choosing **State Manager** in the navigation pane\. After you delete the association, run Quick Setup again to see if the problem persists\.

## Running Quick Setup Again<a name="quick-setup-results-running-again"></a>

You can run Quick Setup again by choosing the **Edit all** button on the Quick Setup results page\. You can select or clear options\. If you clear an option, Systems Manager doesn't delete the association\. For example, if you previously selected the **Collect inventory from your instances every 30 minutes** option, and you clear the option when you run Quick Setup again, the original association still exists\. You must either delete the association from the Quick Setup results page or from the **State Manager** page\.

**Important**  
If you want to run Quick Setup on new instances, we recommend that you choose the new instances and also choose all of the instances on which you previously ran Quick Setup\. By choosing all of the instances on which you previously ran Quick Setup, you synchronize the association executions for all of the instances\. This synchronizes status and compliance reporting\. We also recommend that you target instances by using tags\. New instances with the specified tags are *automatically* added to Quick Setup associations\. This means that they automatically display status in the Quick Setup results and in the **Compliance** page\.