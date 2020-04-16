# Supported resources reference<a name="OpsCenter-related-resources-reference"></a>

OpsCenter automatically creates a deep link to the primary resource page when you specify the Amazon Resource Name \(ARN\) for the following types of AWS resources\. For example, if you specify the ARN of an EC2 instance in the **Related Resources** field, then OpsCenter creates a deep link to the information about that instance in the Amazon EC2 console\. This enables you to view detailed information about your impacted AWS resources without having to leave OpsCenter\. For more information about adding related resources, see [Working with related resources](OpsCenter-working-with-OpsItems.md#OpsCenter-working-with-OpsItems-related-resources)\.


**Supported resources**  

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
|  Amazon DynamoDB table  |  <pre>arn:aws:dynamodb:region:account-id:table/tablename</pre>  | 
|  Amazon EC2 customer gateway  |  <pre>arn:aws:ec2:region:account-id:customer-gateway/cgw-id</pre>  | 
|  Amazon EC2 elastic IP  |  <pre>arn:aws:ec2:region:account-id:eip/eipalloc-id</pre>  | 
|  Amazon EC2 dedicated host  |  <pre>arn:aws:ec2:region:account-id:dedicated-host/host-id</pre>  | 
|  Amazon EC2 instance  |  <pre>arn:aws:ec2:region:account-id:instance/instance-id</pre>  | 
|  Amazon EC2 internet gateway  |  <pre>arn:aws:ec2:region:account-id:internet-gateway/igw-id</pre>  | 
|  Amazon EC2 network access control list \(ACL\)  |  <pre>arn:aws:ec2:region:account-id:network-acl/nacl-id</pre>  | 
|  Amazon EC2 network interface  |  <pre>arn:aws:ec2:region:account-id:network-interface/eni-id</pre>  | 
|  Amazon EC2 route table  |  <pre>arn:aws:ec2:region:account-id:route-table/route-table-id</pre>  | 
|  Amazon EC2 security group  |  <pre>arn:aws:ec2:region:account-id:security-group/security-group-id</pre>  | 
|  Amazon EC2 subnet  |  <pre>arn:aws:ec2:region:account-id:subnet/subnet-id</pre>  | 
|  Amazon EC2 volume  |  <pre>arn:aws:ec2:region:account-id:volume/volume-id</pre>  | 
|  Amazon EC2 VPC  |  <pre>arn:aws:ec2:region:account-id:vpc/vpc-id</pre>  | 
|  Amazon EC2 VPN connection  |  <pre>arn:aws:ec2:region:account-id:vpn-connection/vpn-id</pre>  | 
|  Amazon EC2 VPN gateway  |  <pre>arn:aws:ec2:region:account-id:vpn-gateway/vgw-id</pre>  | 
|  AWS Elastic Beanstalk application  |  <pre>arn:aws:elasticbeanstalk:region:account-id:application/applicationname</pre>  | 
|  Elastic Load Balancing \(classic load balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/name</pre>  | 
|  Elastic Load Balancing \(application load balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/app/load-balancer-name/load-balancer-id</pre>  | 
|  Elastic Load Balancing \(network load balancer\)  |  <pre>arn:aws:elasticloadbalancing:region:account-id:loadbalancer/net/load-balancer-name/load-balancer-id</pre>  | 
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
|  AWS Config recording of AWS Systems Manager managed instance inventory  |  <pre>arn:aws:ssm:region:account-id:managed-instance-inventory/instance_id</pre>  | 
|  Systems Manager State Manager association  |  <pre>arn:aws:ssm:region:account-id:association/association_ID</pre>  | 