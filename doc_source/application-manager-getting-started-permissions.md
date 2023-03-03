# Configuring permissions for Systems Manager Application Manager<a name="application-manager-getting-started-permissions"></a>

You can use all features of Application Manager, a capability of AWS Systems Manager, if your AWS Identity and Access Management \(IAM\) entity \(such as a user, group, or role\) has access to the API operations listed in this topic\. The API operations are separated into two tables to help you understand the different functions they perform\.

The following table lists the API operations that Systems Manager calls if you choose a resource in Application Manager because you want to view the resource details\. For example, if Application Manager lists an Amazon EC2 Auto Scaling group, and if you choose that group to view its details, then Systems Manager calls the `autoscaling:DescribeAutoScalingGroups` API operations\. If you don't have any Auto Scaling groups in your account, this API operation isn't called from Application Manager\.


****  

| Resource details only | 
| --- | 
|  <pre>acm:DescribeCertificate <br />acm:ListTagsForCertificate<br />autoscaling:DescribeAutoScalingGroups <br />cloudfront:GetDistribution<br />cloudfront:ListTagsForResource <br />cloudtrail:DescribeTrails<br />cloudtrail:ListTags <br />cloudtrail:LookupEvents<br />codebuild:BatchGetProjects <br />codepipeline:GetPipeline<br />codepipeline:ListTagsForResource <br />dynamodb:DescribeTable<br />dynamodb:ListTagsOfResource <br />ec2:DescribeAddresses<br />ec2:DescribeCustomerGateways <br />ec2:DescribeHosts<br />ec2:DescribeInternetGateways <br />ec2:DescribeNetworkAcls<br />ec2:DescribeNetworkInterfaces <br />ec2:DescribeRouteTables<br />ec2:DescribeSecurityGroups <br />ec2:DescribeSubnets<br />ec2:DescribeVolumes <br />ec2:DescribeVpcs <br />ec2:DescribeVpnConnections<br />ec2:DescribeVpnGateways <br />elasticbeanstalk:DescribeApplications<br />elasticbeanstalk:ListTagsForResource<br />elasticloadbalancing:DescribeInstanceHealth<br />elasticloadbalancing:DescribeListeners<br />elasticloadbalancing:DescribeLoadBalancers<br />elasticloadbalancing:DescribeTags <br />iam:GetGroup <br />iam:GetPolicy<br />iam:GetRole <br />iam:GetUser <br />lambda:GetFunction<br />rds:DescribeDBClusters <br />rds:DescribeDBInstances<br />rds:DescribeDBSecurityGroups <br />rds:DescribeDBSnapshots<br />rds:DescribeDBSubnetGroups <br />rds:DescribeEventSubscriptions<br />rds:ListTagsForResource <br />redshift:DescribeClusterParameters<br />redshift:DescribeClusterSecurityGroups<br />redshift:DescribeClusterSnapshots<br />redshift:DescribeClusterSubnetGroups <br />redshift:DescribeClusters<br />s3:GetBucketTagging</pre>  | 

The following table lists the API operations that Systems Manager uses to make changes to applications and resources listed in Application Manager or to view operations information for a selected application or resource\.


****  

| Application actions and details | 
| --- | 
|  <pre><br />applicationinsights:CreateApplication<br />applicationinsights:DescribeApplication<br />applicationinsights:ListProblems<br />ce:GetCostAndUsage<br />ce:GetTags<br />ce:ListCostAllocationTags<br />ce:UpdateCostAllocationTagsStatus<br />cloudformation:CreateStack<br />cloudformation:DeleteStack<br />cloudformation:DescribeStackDriftDetectionStatus<br />cloudformation:DescribeStackEvents<br />cloudformation:DescribeStacks<br />cloudformation:DetectStackDrift<br />cloudformation:GetTemplate<br />cloudformation:GetTemplateSummary<br />cloudformation:ListStacks<br />cloudformation:UpdateStack<br />cloudwatch:DescribeAlarms<br />cloudwatch:DescribeInsightRules<br />cloudwatch:DisableAlarmActions<br />cloudwatch:EnableAlarmActions<br />cloudwatch:GetMetricData<br />cloudwatch:ListTagsForResource<br />cloudwatch:PutMetricAlarm<br />config:DescribeComplianceByConfigRule<br />config:DescribeComplianceByResource<br />config:DescribeConfigRules<br />config:DescribeRemediationConfigurations<br />config:GetComplianceDetailsByConfigRule<br />config:GetComplianceDetailsByResource<br />config:GetResourceConfigHistory<br />config:ListDiscoveredResources<br />config:PutRemediationConfigurations<br />config:SelectResourceConfig<br />config:StartConfigRulesEvaluation<br />config:StartRemediationExecution<br />ec2:DescribeInstances<br />ecs:DescribeCapacityProviders<br />ecs:DescribeClusters<br />ecs:DescribeContainerInstances<br />ecs:ListClusters<br />ecs:ListContainerInstances<br />ecs:TagResource<br />eks:DescribeCluster<br />eks:DescribeFargateProfile<br />eks:DescribeNodegroup<br />eks:ListClusters<br />eks:ListFargateProfiles<br />eks:ListNodegroups<br />eks:TagResource<br />iam:CreateServiceLinkedRole<br />iam:ListRoles<br />logs:DescribeLogGroups<br />resource-groups:CreateGroup<br />resource-groups:DeleteGroup<br />resource-groups:GetGroup<br />resource-groups:GetGroupQuery<br />resource-groups:GetTags<br />resource-groups:ListGroupResources<br />resource-groups:ListGroups<br />resource-groups:Tag<br />resource-groups:Untag<br />resource-groups:UpdateGroup<br />s3:ListAllMyBuckets<br />s3:ListBucket<br />s3:ListBucketVersions<br />servicecatalog:GetApplication<br />servicecatalog:ListApplications<br />sns:CreateTopic<br />sns:ListSubscriptionsByTopic<br />sns:ListTopics<br />sns:Subscribe<br />ssm:AddTagsToResource<br />ssm:CreateDocument<br />ssm:CreateOpsMetadata<br />ssm:DeleteDocument<br />ssm:DeleteOpsMetadata<br />ssm:DescribeAssociation<br />ssm:DescribeAutomationExecutions<br />ssm:DescribeDocument<br />ssm:DescribeDocumentPermission<br />ssm:GetDocument<br />ssm:GetInventory<br />ssm:GetOpsMetadata<br />ssm:GetOpsSummary<br />ssm:GetServiceSetting<br />ssm:ListAssociations<br />ssm:ListComplianceItems<br />ssm:ListDocuments<br />ssm:ListDocumentVersions<br />ssm:ListOpsMetadata<br />ssm:ListResourceComplianceSummaries<br />ssm:ListTagsForResource<br />ssm:ModifyDocumentPermission<br />ssm:RemoveTagsFromResource<br />ssm:StartAssociationsOnce<br />ssm:StartAutomationExecution<br />ssm:UpdateDocument<br />ssm:UpdateDocumentDefaultVersion<br />ssm:UpdateOpsItem<br />ssm:UpdateOpsMetadata<br />ssm:UpdateServiceSetting<br />tag:GetTagKeys<br />tag:GetTagValues<br />tag:TagResources<br />tag:UntagResources</pre>  | 

## Configuring permissions<a name="application-manager-getting-started-user-permissions"></a>

To configure Application Manager permissions for an IAM entity \(such as a user, group, or role\), create an IAM policy using the following example\. This policy example includes all API operations used by Application Manager\. 

```
                    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "acm:DescribeCertificate",
                "acm:ListTagsForCertificate",
                "applicationinsights:CreateApplication",
                "applicationinsights:DescribeApplication",
                "applicationinsights:ListProblems",
                "autoscaling:DescribeAutoScalingGroups",
                "ce:GetCostAndUsage",
                "ce:GetTags",
                "ce:ListCostAllocationTags",
                "ce:UpdateCostAllocationTagsStatus",
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStackDriftDetectionStatus",
                "cloudformation:DescribeStackEvents",
                "cloudformation:DescribeStacks",
                "cloudformation:DetectStackDrift",
                "cloudformation:GetTemplate",
                "cloudformation:GetTemplateSummary",
                "cloudformation:ListStacks",
                "cloudformation:UpdateStack",
                "cloudfront:GetDistribution",
                "cloudfront:ListTagsForResource",
                "cloudtrail:DescribeTrails",
                "cloudtrail:ListTags",
                "cloudtrail:LookupEvents",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DescribeInsightRules",
                "cloudwatch:DisableAlarmActions",
                "cloudwatch:EnableAlarmActions",
                "cloudwatch:GetMetricData",
                "cloudwatch:ListTagsForResource",
                "cloudwatch:PutMetricAlarm",
                "codebuild:BatchGetProjects",
                "codepipeline:GetPipeline",
                "codepipeline:ListTagsForResource",
                "config:DescribeComplianceByConfigRule",
                "config:DescribeComplianceByResource",
                "config:DescribeConfigRules",
                "config:DescribeRemediationConfigurations",
                "config:GetComplianceDetailsByConfigRule",
                "config:GetComplianceDetailsByResource",
                "config:GetResourceConfigHistory",
                "config:ListDiscoveredResources",
                "config:PutRemediationConfigurations",
                "config:SelectResourceConfig",
                "config:StartConfigRulesEvaluation",
                "config:StartRemediationExecution",
                "dynamodb:DescribeTable",
                "dynamodb:ListTagsOfResource",
                "ec2:DescribeAddresses",
                "ec2:DescribeCustomerGateways",
                "ec2:DescribeHosts",
                "ec2:DescribeInstances",
                "ec2:DescribeInternetGateways",
                "ec2:DescribeNetworkAcls",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVolumes",
                "ec2:DescribeVpcs",
                "ec2:DescribeVpnConnections",
                "ec2:DescribeVpnGateways",
                "ecs:DescribeCapacityProviders",
                "ecs:DescribeClusters",
                "ecs:DescribeContainerInstances",
                "ecs:ListClusters",
                "ecs:ListContainerInstances",
                "ecs:TagResource",
                "eks:DescribeCluster",
                "eks:DescribeFargateProfile",
                "eks:DescribeNodegroup",
                "eks:ListClusters",
                "eks:ListFargateProfiles",
                "eks:ListNodegroups",
                "eks:TagResource",
                "elasticbeanstalk:DescribeApplications",
                "elasticbeanstalk:ListTagsForResource",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTags",
                "iam:CreateServiceLinkedRole",
                "iam:GetGroup",
                "iam:GetPolicy",
                "iam:GetRole",
                "iam:GetUser",
                "iam:ListRoles",
                "lambda:GetFunction",
                "logs:DescribeLogGroups",
                "rds:DescribeDBClusters",
                "rds:DescribeDBInstances",
                "rds:DescribeDBSecurityGroups",
                "rds:DescribeDBSnapshots",
                "rds:DescribeDBSubnetGroups",
                "rds:DescribeEventSubscriptions",
                "rds:ListTagsForResource",
                "redshift:DescribeClusterParameters",
                "redshift:DescribeClusters",
                "redshift:DescribeClusterSecurityGroups",
                "redshift:DescribeClusterSnapshots",
                "redshift:DescribeClusterSubnetGroups",
                "resource-groups:CreateGroup",
                "resource-groups:DeleteGroup",
                "resource-groups:GetGroup",
                "resource-groups:GetGroupQuery",
                "resource-groups:GetTags",
                "resource-groups:ListGroupResources",
                "resource-groups:ListGroups",
                "resource-groups:Tag",
                "resource-groups:Untag",
                "resource-groups:UpdateGroup",
                "s3:GetBucketTagging",
                "s3:ListAllMyBuckets",
                "s3:ListBucket",
                "s3:ListBucketVersions",
                "servicecatalog:GetApplication",
                "servicecatalog:ListApplications",
                "sns:CreateTopic",
                "sns:ListSubscriptionsByTopic",
                "sns:ListTopics",
                "sns:Subscribe",
                "ssm:AddTagsToResource",
                "ssm:CreateDocument",
                "ssm:CreateOpsMetadata",
                "ssm:DeleteDocument",
                "ssm:DeleteOpsMetadata",
                "ssm:DescribeAssociation",
                "ssm:DescribeAutomationExecutions",
                "ssm:DescribeDocument",
                "ssm:DescribeDocumentPermission",
                "ssm:GetDocument",
                "ssm:GetInventory",
                "ssm:GetOpsMetadata",
                "ssm:GetOpsSummary",
                "ssm:GetServiceSetting",
                "ssm:ListAssociations",
                "ssm:ListComplianceItems",
                "ssm:ListDocuments",
                "ssm:ListDocumentVersions",
                "ssm:ListOpsMetadata",
                "ssm:ListResourceComplianceSummaries",
                "ssm:ListTagsForResource",
                "ssm:ModifyDocumentPermission",
                "ssm:RemoveTagsFromResource",
                "ssm:StartAssociationsOnce",
                "ssm:StartAutomationExecution",
                "ssm:UpdateDocument",
                "ssm:UpdateDocumentDefaultVersion",
                "ssm:UpdateOpsMetadata",
                "ssm:UpdateOpsItem",
                "ssm:UpdateServiceSetting",
                "tag:GetTagKeys",
                "tag:GetTagValues",
                "tag:TagResources",
                "tag:UntagResources"
            ],
            "Resource": "*"
        }
    ]
}
```

**Note**  
You can restrict a user's ability to make changes to applications and resources in Application Manager by removing the following API operations from the IAM permissions policy attached to their user, group, or role\. Removing these actions creates a read\-only experience in Application Manager\. The following are all of the APIs that allow users to make changes to the application or any other related resources\.   

```
applicationinsights:CreateApplication
ce:UpdateCostAllocationTagsStatus
cloudformation:CreateStack
cloudformation:DeleteStack
cloudformation:UpdateStack
cloudwatch:DisableAlarmActions
cloudwatch:EnableAlarmActions
cloudwatch:PutMetricAlarm
config:PutRemediationConfigurations
config:StartConfigRulesEvaluation
config:StartRemediationExecution
ecs:TagResource
eks:TagResource
iam:CreateServiceLinkedRole
resource-groups:CreateGroup
resource-groups:DeleteGroup
resource-groups:Tag
resource-groups:Untag
resource-groups:UpdateGroup
sns:CreateTopic
sns:Subscribe
ssm:AddTagsToResource
ssm:CreateDocument
ssm:CreateOpsMetadata
ssm:DeleteDocument
ssm:DeleteOpsMetadata
ssm:ModifyDocumentPermission
ssm:RemoveTagsFromResource
ssm:StartAssociationsOnce
ssm:StartAutomationExecution
ssm:UpdateDocument
ssm:UpdateDocumentDefaultVersion
ssm:UpdateOpsMetadata
ssm:UpdateOpsItem
ssm:UpdateServiceSetting
tag:TagResources
tag:UntagResources
```

For information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about how to assign this policy to an IAM entity \(such as a user, group, or role\), see [ Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\. 