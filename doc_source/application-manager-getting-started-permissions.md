# Configuring permissions for Systems Manager Application Manager<a name="application-manager-getting-started-permissions"></a>

You can use all features of Application Manager, a capability of AWS Systems Manager, if your AWS Identity and Access Management \(IAM\) user, group, or role has access to the API actions listed in this topic\. The API actions are separated into two tables to help you understand the different functions they perform\.

The following table lists the API actions that Systems Manager calls if you choose a resource in Application Manager because you want to view the resource details\. For example, if Application Manager lists an Amazon EC2 Auto Scaling group, and if you choose that group to view its details, then Systems Manager calls the `autoscaling:DescribeAutoScalingGroups` API action\. If you don't have any Auto Scaling groups in your account, this API action isn't called from Application Manager\.


****  

| Resource details only | 
| --- | 
|  <pre>acm:DescribeCertificate <br />acm:ListTagsForCertificate<br />autoscaling:DescribeAutoScalingGroups <br />cloudfront:GetDistribution<br />cloudfront:ListTagsForResource <br />cloudtrail:DescribeTrails<br />cloudtrail:ListTags <br />cloudtrail:LookupEvents<br />codebuild:BatchGetProjects <br />codepipeline:GetPipeline<br />codepipeline:ListTagsForResource <br />dynamodb:DescribeTable<br />dynamodb:ListTagsOfResource <br />ec2:DescribeAddresses<br />ec2:DescribeCustomerGateways <br />ec2:DescribeHosts<br />ec2:DescribeInternetGateways <br />ec2:DescribeNetworkAcls<br />ec2:DescribeNetworkInterfaces <br />ec2:DescribeRouteTables<br />ec2:DescribeSecurityGroups <br />ec2:DescribeSubnets<br />ec2:DescribeVolumes <br />ec2:DescribeVpcs <br />ec2:DescribeVpnConnections<br />ec2:DescribeVpnGateways <br />elasticbeanstalk:DescribeApplications<br />elasticbeanstalk:ListTagsForResource<br />elasticloadbalancing:DescribeInstanceHealth<br />elasticloadbalancing:DescribeListeners<br />elasticloadbalancing:DescribeLoadBalancers<br />elasticloadbalancing:DescribeTags <br />iam:GetGroup <br />iam:GetPolicy<br />iam:GetRole <br />iam:GetUser <br />lambda:GetFunction<br />rds:DescribeDBClusters <br />rds:DescribeDBInstances<br />rds:DescribeDBSecurityGroups <br />rds:DescribeDBSnapshots<br />rds:DescribeDBSubnetGroups <br />rds:DescribeEventSubscriptions<br />rds:ListTagsForResource <br />redshift:DescribeClusterParameters<br />redshift:DescribeClusterSecurityGroups<br />redshift:DescribeClusterSnapshots<br />redshift:DescribeClusterSubnetGroups <br />redshift:DescribeClusters<br />s3:GetBucketTagging</pre>  | 

The following table lists the API actions that Systems Manager uses to make changes to applications and resources listed in Application Manager or to view operations information for a selected application or resource\.


****  

| Application actions and details | 
| --- | 
|  <pre>cloudformation:DescribeStacks <br />cloudwatch:DescribeAlarms<br />cloudwatch:DescribeInsightRules <br />cloudwatch:ListMetrics<br />cloudwatch:ListTagsForResource<br />config:DescribeComplianceByResource<br />config:DescribeRemediationConfigurations<br />config:GetComplianceDetailsByResource<br />config:GetResourceConfigHistory<br />config:StartConfigRulesEvaluation <br />ec2:DescribeInstances<br />eks:DescribeCluster <br />eks:ListClusters <br />eks:ListFargateProfiles<br />eks:ListNodegroups <br />eks:TagResource <br />resource-groups:CreateGroup<br />resource-groups:DeleteGroup <br />resource-groups:GetGroup<br />resource-groups:GetGroupQuery <br />resource-groups:GetTags<br />resource-groups:ListGroupResources <br />resource-groups:ListGroups<br />resource-groups:Tag <br />resource-groups:Untag <br />ssm:CreateOpsMetadata<br />ssm:DeleteOpsMetadata <br />ssm:GetOpsSummary<br />ssm:GetOpsMetadata<br />ssm:UpdateServiceSetting<br />ssm:GetServiceSetting<br />ssm:ListOpsMetadata<br />ssm:UpdateOpsItem <br />tag:GetTagKeys <br />tag:GetTagValues</pre>  | 

## Configuring permissions<a name="application-manager-getting-started-user-permissions"></a>

To configure Application Manager permissions for an IAM user, group, or role, create an IAM policy using the following example\. This policy example includes all API actions used by Application Manager\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "acm:DescribeCertificate",
                "acm:ListTagsForCertificate",
                "autoscaling:DescribeAutoScalingGroups",
                "cloudfront:GetDistribution",
                "cloudfront:ListTagsForResource",
                "cloudtrail:DescribeTrails",
                "cloudtrail:ListTags",
                "cloudtrail:LookupEvents",
                "codebuild:BatchGetProjects",
                "codepipeline:GetPipeline",
                "codepipeline:ListTagsForResource",
                "dynamodb:DescribeTable",
                "dynamodb:ListTagsOfResource",
                "ec2:DescribeAddresses",
                "ec2:DescribeCustomerGateways",
                "ec2:DescribeHosts",
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
                "elasticbeanstalk:DescribeApplications",
                "elasticbeanstalk:ListTagsForResource",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTags",
                "iam:GetGroup",
                "iam:GetPolicy",
                "iam:GetRole",
                "iam:GetUser",
                "lambda:GetFunction",
                "rds:DescribeDBClusters",
                "rds:DescribeDBInstances",
                "rds:DescribeDBSecurityGroups",
                "rds:DescribeDBSnapshots",
                "rds:DescribeDBSubnetGroups",
                "rds:DescribeEventSubscriptions",
                "rds:ListTagsForResource",
                "redshift:DescribeClusterParameters",
                "redshift:DescribeClusterSecurityGroups",
                "redshift:DescribeClusterSnapshots",
                "redshift:DescribeClusterSubnetGroups",
                "redshift:DescribeClusters",
                "s3:GetBucketTagging",
                "cloudformation:DescribeStacks",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DescribeInsightRules",
                "cloudwatch:ListMetrics",
                "cloudwatch:ListTagsForResource",
                "config:DescribeComplianceByResource",
                "config:DescribeRemediationConfigurations",
                "config:GetComplianceDetailsByResource",
                "config:GetResourceConfigHistory",
                "config:StartConfigRulesEvaluation",
                "ec2:DescribeInstances",
                "eks:DescribeCluster",
                "eks:ListClusters",
                "eks:ListFargateProfiles",
                "eks:ListNodegroups",
                "eks:TagResource",
                "resource-groups:CreateGroup",
                "resource-groups:DeleteGroup",
                "resource-groups:GetGroup",
                "resource-groups:GetGroupQuery",
                "resource-groups:GetTags",
                "resource-groups:ListGroupResources",
                "resource-groups:ListGroups",
                "resource-groups:Tag",
                "resource-groups:Untag",
                "ssm:CreateOpsMetadata",
                "ssm:DeleteOpsMetadata",
                "ssm:GetOpsSummary",
                "ssm:GetOpsMetadata",
                "ssm:UpdateServiceSetting",
                "ssm:GetServiceSetting",
                "ssm:ListOpsMetadata",
                "ssm:UpdateOpsItem",
                "tag:GetTagKeys",
                "tag:GetTagValues"
            ],
            "Resource": "*"
        }
    ]
}
```

**Note**  
You can restrict a user's ability to make changes to applications and resources in Application Manager by removing the following API actions from the IAM permissions policy attached to their user, group, or role\. Removing these actions creates a read\-only experience in Application Manager\.  

```
eks:TagResource
resource-groups:CreateGroup
resource-groups:DeleteGroup 
resource-groups:Tag 
resource-groups:Untag 
ssm:CreateOpsMetadata
ssm:DeleteOpsMetadata 
ssm:UpdateOpsItem
```

For information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about how to assign this policy to an IAM user, group, or role, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\. 