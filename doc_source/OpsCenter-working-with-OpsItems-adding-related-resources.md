# Adding related resources to an OpsItem<a name="OpsCenter-working-with-OpsItems-adding-related-resources"></a>

Each OpsItem includes a **Related resources** section that lists the Amazon Resource Name \(ARN\) of the related resource\. A *related resource* is the impacted AWS resource that needs to be investigated\. 

If Amazon EventBridge creates the OpsItem, the system automatically populates the OpsItem with the ARN of the resource\. You can manually specify ARNs of related resources\. For some ARN types, OpsCenter automatically creates a deep link that displays details about the resource\. The deep link makes it unnecessary for you to have to use other console pages to view that information\. For example, you can specify the ARN of an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. Then, in OpsCenter, you can view all of the details that Amazon EC2 provides about that instance\. 

**To view and add related resources to an OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the **OpsItems** tab\.

1. Choose an OpsItem ID\.  
![\[A new OpsItem on the OpsCenter Overview page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1.png)

1. To view information about the impacted resource, choose the **Related resources details** tab\.  
![\[Viewing the Related resource details tab for an OpsItem.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1_5.png)

   This tab displays information about the resource from several AWS services\. Expand the **Resource details** section to view information about this resource as provided by the AWS service that hosts it\. You can also toggle through other related resources associated with this OpsItem by using the **Related resources** list\.

1. To add additional related resources, choose the **Overview** tab\.

1. In the **Related resources** section, choose **Add**\.

1. For **Resource type**, choose a resource from the list\.

1. For **Resource ID**, enter either the ID or the Amazon Resource Name \(ARN\)\. The type of information you choose depends on the resource that you chose in the previous step\.

**Note**  
You can manually add the ARNs of additional related resources\. Each OpsItem can list a maximum of 100 related resource ARNs\.

The following table lists the resource types that automatically create deep links to related resources\.


**Supported resource types**  

| Resource name | ARN format | 
| --- | --- | 
|  AWS Certificate Manager certificate  |  <pre>arn:aws:acm:region:account-id:certificate/certificate-id</pre>  | 
|  Amazon EC2 Auto Scaling group  |  <pre>arn:aws:autoscaling:region:account-id:autoScalingGroup:groupid:autoScalingGroupName/groupfriendlyname</pre>  | 
|  Amazon CloudFront distribution  |  <pre>arn:aws:cloudfront::account-id:*</pre>  | 
|  AWS CloudFormation stack  |  <pre>arn:aws:cloudformation:region:account-id:stack/stackname/additionalidentifier</pre>  | 
|  Amazon CloudWatch alarm  |  <pre>arn:aws:cloudwatch:region:account-id:alarm:alarm-name</pre>  | 
|  AWS CloudTrail trail  |  <pre>arn:aws:cloudtrail:region:account-id:trail/trailname</pre>  | 
|  AWS CodeBuild project  |  <pre>arn:aws:codebuild:region:account-id:resourcetype/resource</pre>  | 
|  AWS CodePipeline  |  <pre>arn:aws:codepipeline:region:account-id:resource-specifier</pre>  | 
|  Amazon DevOps Guru insight  |  <pre>arn:aws:devops-guru:region:account-id:insight/proactive or reactive/resource-id</pre>  | 
|  Amazon DynamoDB table  |  <pre>arn:aws:dynamodb:region:account-id:table/tablename</pre>  | 
|  Amazon Elastic Compute Cloud \(Amazon EC2\) customer gateway  |  <pre>arn:aws:ec2:region:account-id:customer-gateway/cgw-id</pre>  | 
|  Amazon EC2 elastic IP  |  <pre>arn:aws:ec2:region:account-id:eip/eipalloc-id</pre>  | 
|  Amazon EC2 Dedicated Host  |  <pre>arn:aws:ec2:region:account-id:dedicated-host/host-id</pre>  | 
|  Amazon EC2 instance  |  <pre>arn:aws:ec2:region:account-id:instance/instance-id</pre>  | 
|  Amazon EC2 internet gateway  |  <pre>arn:aws:ec2:region:account-id:internet-gateway/igw-id</pre>  | 
|  Amazon EC2 network access control list \(network ACL\)  |  <pre>arn:aws:ec2:region:account-id:network-acl/nacl-id</pre>  | 
|  Amazon EC2 network interface  |  <pre>arn:aws:ec2:region:account-id:network-interface/eni-id</pre>  | 
|  Amazon EC2 route table  |  <pre>arn:aws:ec2:region:account-id:route-table/route-table-id</pre>  | 
|  Amazon EC2 security group  |  <pre>arn:aws:ec2:region:account-id:security-group/security-group-id</pre>  | 
|  Amazon EC2 subnet  |  <pre>arn:aws:ec2:region:account-id:subnet/subnet-id</pre>  | 
|  Amazon EC2 volume  |  <pre>arn:aws:ec2:region:account-id:volume/volume-id</pre>  | 
|  Amazon EC2 VPC  |  <pre>arn:aws:ec2:region:account-id:vpc/vpc-id</pre>  | 
|  Amazon EC2 VPN connection  |  <pre>arn:aws:ec2:region:account-id:vpn-connection/vpn-id</pre>  | 
|  Amazon EC2 VPN gateway  |  <pre>arn:aws:ec2:region:account-id:vpn-gateway/vgw-id</pre>  | 
|  AWS Elastic Beanstalk application  |  <pre>arn:aws:elasticbeanstalk:region:account-id:application/applicationname</pre>  | 
|  Elastic Load Balancing \(Classic Load Balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/name</pre>  | 
|  Elastic Load Balancing \(Application Load Balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/app/load-balancer-name/load-balancer-id</pre>  | 
|  Elastic Load Balancing \(Network Load Balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/net/load-balancer-name/load-balancer-id</pre>  | 
|  AWS Identity and Access Management \(IAM\) group  |  <pre>arn:aws:iam::account-id:group/group-name</pre>  | 
|  IAM policy  |  <pre>arn:aws:iam::account-id:policy/policy-name</pre>  | 
|  IAM role  |  <pre>arn:aws:iam::account-id:role/role-name</pre>  | 
|  IAM user  |  <pre>arn:aws:iam::account-id:user/user-name</pre>  | 
|  AWS Lambda function  |  <pre>arn:aws:lambda:region:account-id:function:function-name</pre>  | 
|  Amazon Relational Database Service \(Amazon RDS\) cluster  |  <pre>arn:aws:rds:region:account-id:cluster:db-cluster-name</pre>  | 
|  Amazon RDS database instance  |  <pre>arn:aws:rds:region:account-id:db:db-instance-name</pre>  | 
|  Amazon RDS subscription  |  <pre>arn:aws:rds:region:account-id:es:subscription-name</pre>  | 
|  Amazon RDS security group  |  <pre>arn:aws:rds:region:account-id:secgrp:security-group-name</pre>  | 
|  Amazon RDS cluster snapshot  |  <pre>arn:aws:rds:region:account-id:cluster-snapshot:cluster-snapshot-name</pre>  | 
|  Amazon RDS subnet group  |  <pre>arn:aws:rds:region:account-id:subgrp:subnet-group-name</pre>  | 
|  Amazon Redshift cluster  |  <pre>arn:aws:redshift:region:account-id:cluster:cluster-name</pre>  | 
|  Amazon Redshift parameter group  |  <pre>arn:aws:redshift:region:account-id:parametergroup:parameter-group-name</pre>  | 
|  Amazon Redshift security group  |  <pre>arn:aws:redshift:region:account-id:securitygroup:security-group-name</pre>  | 
|  Amazon Redshift cluster snapshot  |  <pre>arn:aws:redshift:region:account-id:snapshot:cluster-name/snapshot-name</pre>  | 
|  Amazon Redshift subnet group  |  <pre>arn:aws:redshift:region:account-id:subnetgroup:subnet-group-name</pre>  | 
|  Amazon Simple Storage Service \(Amazon S3\) bucket  |  <pre>arn:aws:s3:::bucket_name</pre>  | 
|  AWS Config recording of AWS Systems Manager managed node inventory  |  <pre>arn:aws:ssm:region:account-id:managed-instance-inventory/node_id</pre>  | 
|  Systems Manager State Manager association  |  <pre>arn:aws:ssm:region:account-id:association/association_ID</pre>  | 