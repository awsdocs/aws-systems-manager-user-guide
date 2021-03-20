# AWS\-UpdateEKSManagedNodegroupVersion<a name="automation-aws-updateeksmanagednodegroupversion"></a>

**Description**

This runbook updates managed node groups in your Amazon EKS cluster to the latest AMI version\. For more information about this update process, see [Updating a managed node group](https://docs.aws.amazon.com/eks/latest/userguide/update-managed-node-group.html) in the *Amazon EKS User Guide*\. We also recommend that you review the following topics before you use the AWS\-UpdateEKSManagedNodegroupVersion runbook\.
+ [Managed node groups](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-groups.html)
+ [Managed node update behavior](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-update-behavior.html)
+ [UpdateNodegroupVersion](https://docs.aws.amazon.com/eks/latest/APIReference/;API_UpdateNodegroupVersion.html)

If your cluster uses autoscaling, scale the deployment down to zero replicas to avoid conflicting scaling actions\. 

**To scale a deployment to zero replicas**

1. Install the Kubernetes command line utility, `kubectl`\. For more information, see [Installing kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) in the *Amazon EKS User Guide*\.

1. Run the following command\.

   ```
   kubectl scale deployments/cluster-autoscaler --replicas=0 -n kube-system
   ```

1. Run the AWS\-UpdateEKSManagedNodegroupVersion runbook\.

1. Scale the deployment back to the desired number of replicas by running the following command\.

   ```
   kubectl scale deployments/cluster-autoscaler --replicas=number -n kube-system
   ```

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-UpdateEKSManagedNodegroupVersion)

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
+ ClusterName

  Type: String

  Description: \(Required\) The name of the Amazon EKS cluster\.
+ NodeGroupName

  Type: String

  Description: \(Required\) The name of the managed node group\.
+ LaunchTemplateVersion

  Type: String

  Description: \(Optional\) The Amazon Elastic Compute Cloud \(Amazon EC2\) launch template version\. This parameter is only valid if a node group was created from a launch template\.
+ ForceUpgrade

  Type: Boolean

  Description: \(Optional\) If true, the update won't fail in response to a pod disruption budget violation\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `eks:DescribeNodegroup `
+ `eks:UpdateNogegroupVersion`

**Document Steps**

aws:executeScript \- UpdateEKSManagedNodegroupVersion: Updates the AMI version used by a managed node group in an Amazon EKS cluster\.