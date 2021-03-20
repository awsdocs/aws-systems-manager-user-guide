# AWS\-DeleteEKSCluster<a name="automation-aws-deleteekscluster"></a>

**Description**

This runbook deletes the resources associated with an Amazon EKS cluster, including node groups and Fargate profiles\. Optionally, you can choose to delete all self\-managed nodes, the AWS CloudFormation stacks used to create the nodes, and the VPC CloudFormation stack for your cluster\. For more information about deleting a cluster, see [Deleting a cluster](https://docs.aws.amazon.com/eks/latest/userguide/delete-cluster.html) in the *Amazon EKS User Guide*\. 

**Note**  
If you have active services in your cluster that are associated with a load balancer, you must delete those services before deleting the cluster\. If you don't, the system can't delete the load balancers\. Use the following procedure to find and delete services before you run the AWS\-DeleteEKSCluster runbook\.

**To locate and delete services in your cluster**

1. Install the Kubernetes command line utility, `kubectl`\. For more information, see [Installing kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) in the *Amazon EKS User Guide*\.

1. Run the following command to list all services running in your cluster\.

   ```
   kubectl get svc --all-namespaces
   ```

1. Run the following command to delete any services that have an associated EXTERNAL\-IP value\. These services are fronted by a load balancer, and you must delete them in Kubernetes to allow the load balancer and associated resources to be properly released\.

   ```
   kubectl delete svc service-name
   ```

You can now run the AWS\-DeleteEKSCluster runbook\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteEKSCluster)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ EKSClusterName

  Type: String

  Description: \(Required\) The name of the Amazon EKS Cluster to be deleted\.
+ VPCCloudFormationStack

  Type: String

  Description: \(Optional\) AWS Cloudformation stack name for VPC for the EKS cluster being deleted\. This deletes the AWS Cloudformation stack for VPC and any resources created by the stack\.
+ VPCCloudFormationStackRole

  Type: String

  Description: \(Optional\) The ARN of an IAM role that AWS CloudFormation assumes to delete the VPC CloudFormation stack\. AWS CloudFormation uses the role's credentials to make calls on your behalf\.
+ SelfManagedNodeStacks

  Type: String

  Description: \(Optional\) Comma\-separated list of AWS Cloudformation stack names for self\-managed nodes, This will delete the AWS Cloudformation stacks for self\-managed nodes\.
+ SelfManagedNodeStacksRole

  Type: String

  Description: \(Optional\) The ARN of an IAM role that AWS CloudFormation assumes to delete the Self\-managed Node Stacks\. AWS CloudFormation uses the role's credentials to make calls on your behalf\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `sts:AssumeRole`
+ `eks:ListNodegroups`
+ `eks:DeleteNodegroup`
+ `eks:ListFargateProfiles`
+ `eks:DeleteFargateProfile`
+ `eks:DeleteCluster`
+ `cfn:DescribeStacks`
+ `cfn:DeleteStack`

**Document Steps**
+ aws:executeScript \- DeleteNodeGroups: Find and delete all node groups in the EKS cluster\.
+ aws:executeScript \- DeleteFargateProfiles: Find and delete all Fargate profiles in the EKS cluster\.
+ aws:executeScript \- DeleteSelfManagedNodes: Delete all self\-managed nodes and the CloudFormation stacks used to create the nodes\.
+ aws:executeScript \- DeleteEKSCluster: Delete EKS cluster\.
+ aws:executeScript \- DeleteVPCCloudFormationStack: Delete the VPC CloudFormation stack\.