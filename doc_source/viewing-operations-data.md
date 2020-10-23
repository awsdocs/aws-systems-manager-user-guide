# Viewing operations data for AWS Resource Groups<a name="viewing-operations-data"></a>

In the AWS Systems Manager console, the AWS Resource Groups page displays operations data for a selected group on five tabs: **Details**, **Config**, **CloudTrail**, **Monitoring**, and **OpsItems**\. 

![\[The five tabs that display operations data for a selected group in AWS Resource Groups.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/rg-operations-data-tabs.png)

**Note**  
These tabs are not available when viewing a group in the Resource Groups console\.

You can use the information on these tabs to help you understand which resources in a group are compliant and working correctly and which resources require action\. If you need to take action on a resource, you can use Systems Manager Automation runbooks to perform common operations maintenance and troubleshooting tasks\.


****  

| Tab | Details | 
| --- | --- | 
|  **Details**  |  For tag\-based resource groups, this tab includes an overview of the group, the tags on which the group is based, and a list of the resources discovered by the resource group query\. For AWS CloudFormation stack\-based groups, this tab includes detailed information about the AWS CloudFormation stack, including stack events\. For tag\-based and AWS CloudFormation stack\-based resource groups, if a resource in the **Group resources** section includes a link, you can choose this link to view additional details about the selected resource\. Not all AWS resources are supported for the additional details view\. For more information, see [Resources supported in the additional details view](#viewing-operations-data-supported) in this topic\.  | 
|  **Config**  |  This tab includes AWS Config compliance and history information for resources in the selected group\.   | 
|  **CloudTrail**  |  This tab lists AWS CloudTrail events for resources in the selected group\.  | 
|  **Monitoring**  |  This tab lists Amazon CloudWatch alarms for resources in the selected group\. This tab also shows CloudWatch dashboards associated with your resource group\. The tab is empty if no alarms have been configured and no dashboards created for the selected resource group\.  | 
|  **OpsItems**  |  This tab lists all operational work items \(OpsItems\) for AWS CloudFormation stack\-based resources in the selected group\. OpsItems for tag\-based resources are not supported\. For tag\-based resources, the OpsItems table is empty\. The table is also empty if no OpsItems were created for the AWS CloudFormation resources in this group\.  | 

**Important**  
Before you can view OpsItems on the **OpsItems** tab, you must set up and configure AWS Systems Manager Explorer\. For more information, see [Getting Started with Systems Manager Explorer and OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/Explorer-setup) in the *AWS Systems Manager User Guide*\. After you set up and configure Explorer, Systems Manager automatically populates the **OpsItems** tab\.

## Resources supported in the additional details view<a name="viewing-operations-data-supported"></a>

The **Details** tab for tag\-based and AWS CloudFormation stack\-based groups includes a **Group resources** section\. This section lists each resource in the selected group\. 

![\[The Group resources section for a selected group in AWS Resource Groups.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/rg-operations-data-group-resources.png)

For tag\-based and AWS CloudFormation stack\-based resource groups, if a resource in this section includes a link, you can choose this link to view additional details about the selected resource\. The additional details page includes operational data from multiple AWS services to help you quickly assess the status of a resource and recent actions applied to it\. 

![\[Additional details view for a selected resource in AWS Resource Groups.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/rg-operations-data-enhanced-details.png)

Not all AWS resources are supported for the additional details view\. The following list shows the AWS services that are supported\. When listed in the **Group resources** section, these resources include a link to the additional details page\.

```
  ACM::Certificate
  AutoScaling::AutoScalingGroup
  CloudFront::Distribution
  CloudFormation::Stack
  CloudWatch::Alarm
  CloudTrail::Trail
  CodeBuild::Project
  CodePipeline::Pipeline
  DynamoDB::Table
  EC2::CustomerGateway
  EC2::EIP
  EC2::Host
  EC2::Instance
  EC2::InternetGateway
  EC2::NetworkAcl
  EC2::NetworkInterface
  EC2::RouteTable
  EC2::SecurityGroup
  EC2::Subnet
  EC2::Volume
  EC2::VPC
  EC2::VPNConnection
  EC2::VPNGateway
  ElasticBeanstalk::Application
  ElasticLoadBalancing::LoadBalancer
  ElasticLoadBalancingV2::NetworkLoadBalancer
  ElasticLoadBalancingV2::ApplicationLoadBalancer
  IAM::Group
  IAM::ManagedPolicy
  IAM::Role
  IAM::User
  Lambda::Function
  RDS::DBCluster
  RDS::DBInstance
  RDS::EventSubscription
  RDS::DBSecurityGroup
  RDS::DBSnapshot
  RDS::DBSubnetGroup
  Redshift::Cluster
  Redshift::ClusterParameterGroup
  Redshift::ClusterSecurityGroup
  Redshift::ClusterSnapshot
  Redshift::ClusterSubnetGroup
  S3::Bucket
  SSM::ManagedInstanceInventory
  SSM::Association
```