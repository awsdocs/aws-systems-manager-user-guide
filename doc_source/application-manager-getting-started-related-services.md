# Setting up related services<a name="application-manager-getting-started-related-services"></a>

Application Manager, a capability of AWS Systems Manager, displays resources and information from other AWS services and Systems Manager capabilities\. To maximize the amount of operations information displayed in Application Manager, we recommend that you set up and configure these other services or capabilities *before* you use Application Manager\.

**Topics**
+ [Set up tasks for importing resources](#application-manager-getting-started-related-services-resources)
+ [Set up tasks for viewing operations information about resources](#application-manager-getting-started-related-services-information)

## Set up tasks for importing resources<a name="application-manager-getting-started-related-services-resources"></a>

The following setup tasks help you view AWS resources in Application Manager\. After each of these tasks is completed, Systems Manager can automatically import resources into Application Manager\. After your resources are imported, you can create applications in Application Manager and move your imported resources into them\. This helps you view operations information in the context of an application\.

**\(Optional\) Organize your AWS resources by using [tags](https://docs.aws.amazon.com/systems-manager/latest/userguide/tagging-resources.html)**  
You can assign metadata to your AWS resources in the form of tags\. Each tag is a label consisting of a user\-defined key and value\. Tags can help you manage, identify, organize, search for, and filter resources\. You can create tags to categorize resources by purpose, owner, environment, or other criteria\.

**\(Optional\) Organizes your AWS resources by using [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)**  
You can use resource groups to organize your AWS resources\. Resource groups make it easier to manage, monitor, and automate tasks on many resources at one time\.  
Application Manager automatically imports all of your resource groups and lists them in the **Custom applications** category\.

**\(Optional\) Set up and deploy your AWS resources by using [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)**  
AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. It helps you use AWS services such as Amazon EC2, Amazon Elastic Block Store \(Amazon EBS\), Amazon Simple Notification Service \(Amazon SNS\), Elastic Load Balancing, and AWS Auto Scaling\. With CloudFormation, you can build reliable, scalable, cost\-effective applications in the cloud without worrying about creating and configuring the underlying AWS infrastructure\.   
Application Manager automatically imports all of your AWS CloudFormation resources and lists them in the **AWS CloudFormation stacks** category\. You can create applications in Application Manager and move stacks into them\. This helps you view operations information for resources in your stacks in the context of an application\. For pricing information, see [AWS CloudFormation Pricing](https://aws.amazon.com/cloudformation/pricing/)\.

**\(Optional\) Set up and deploy your applications by using AWS Launch Wizard**  
Launch Wizard guides you through the process of sizing, configuring, and deploying AWS resources for third\-party applications without the need to manually identify and provision individual AWS resources\.  
Application Manager automatically imports all of your Launch Wizard resources and lists them in the **Launch Wizard** category\. For more information about AWS Launch Wizard, see [Getting started with AWS Launch Wizard for SQL Server](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-getting-started.html)\. Launch Wizard is available at no additional charge\. You only pay for the AWS resources that you provision to run your solution\.

**\(Optional\) Set up and deploy your containerized applications by using [Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/) and [Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)**  
Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast container management service that makes it easy to run, stop, and manage containers on a cluster\. Your containers are defined in a task definition that you use to run individual tasks or tasks within a service\.   
Amazon EKS is a managed service that helps you to run Kubernetes on AWS without needing to install, operate, and maintain your own Kubernetes control plane or nodes\. Kubernetes is an open\-source system for automating the deployment, scaling, and management of containerized applications\.   
Application Manager automatically imports all of your Amazon ECS and Amazon EKS infrastructure resources and lists them on the **Container clusters** tab\. However, you can't manage or view operations information about your Amazon EKS pods or containers in Application Manager\. You can only manage and view operations information about the infrastructure that is hosting your Amazon EKS resources\. For pricing information, see [Amazon ECS Pricing](https://aws.amazon.com/ecs/pricing/) and [Amazon EKS Pricing](https://aws.amazon.com/eks/pricing/)\.

## Set up tasks for viewing operations information about resources<a name="application-manager-getting-started-related-services-information"></a>

The following setup tasks help you view operations information about your AWS resources in Application Manager\.

**\(Optional\) Set up and configure Amazon CloudWatch [logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_GettingStarted.html) and [alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html)**  
CloudWatch is a monitoring and management service that provides data and actionable insights for AWS, hybrid, and on\-premises applications and infrastructure resources\. With CloudWatch, you can collect and access all your performance and operational data in the form of logs and metrics from a single platform\. To view CloudWatch logs and alarms for your resources in Application Manager, you must set up and configure CloudWatch\. For pricing information, see [CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.  
CloudWatch Logs support applies to applications only, not to clusters\.

**\(Optional\) Set up and configure [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/getting-started.html)**  
AWS Config provides a detailed view of the resources associated with your AWS account, including how they are configured, how they are related to one another, and how the configurations and their relationships have changed over time\. You can use AWS Config to evaluate the configuration settings of your AWS resources\. You do this by creating AWS Config rules, which represent your ideal configuration settings\. While AWS Config continuously tracks the configuration changes that occur among your resources, it checks whether these changes violate any of the conditions in your rules\. If a resource violates a rule, AWS Config flags the resource and the rule as *noncompliant*\. Application Manager displays compliance information about AWS Config rules\. To view this data in Application Manager, you must set up and configure AWS Config\. For pricing information, see [AWS Config Pricing](https://aws.amazon.com/config/pricing/)\.

**\(Optional\) Create State Manager [associations](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html)**  
You can use Systems Manager State Manager to create a configuration that you assign to your managed instances\. The configuration, called an *association*, defines the state that you want to maintain on your instances\. To view association compliance data in Application Manager, you must configure one or more State Manager associations\. State Manager is offered at no additional charge\.

**\(Optional\) Set up and configure [OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)**  
You can view operational work items \(OpsItems\) about your resources in Application Manager by using OpsCenter\. You can configure Amazon CloudWatch and Amazon EventBridge to automatically send OpsItems to OpsCenter based on alarms and events\. You can also enter OpsItems manually\. For pricing information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**\(Recommended\) Verify [runbook permissions](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-setup.html)**  
You can remediate issues with AWS resources from Application Manager by using Systems Manager Automation runbooks\. To use this remediation capability, you must configure or verify permissions\. For pricing information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.