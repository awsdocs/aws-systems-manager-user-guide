# AWS Systems Manager Application Manager<a name="application-manager"></a>

Application Manager, a capability of AWS Systems Manager, helps DevOps engineers investigate and remediate issues with their AWS resources in the context of their applications and clusters\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single AWS Management Console\.

In Application Manager, an *application* is a logical group of AWS resources that you want to operate as a unit\. This logical group can represent different versions of an application, ownership boundaries for operators, or developer environments, to name a few\. Application Manager support for container clusters includes both Amazon Elastic Kubernetes Service \(Amazon EKS\) and Amazon Elastic Container Service \(Amazon ECS\) clusters\.

When you choose **Get started** on the Application Manager home page, Application Manager automatically imports metadata about your resources that were created in other AWS services or Systems Manager capabilities\. For applications, Application Manager imports metadata about all of your AWS resources organized into resource groups\. Each resource group is listed in the **Custom applications** category as a unique application\. Application Manager also automatically imports metadata about resources that were created by AWS CloudFormation, AWS Launch Wizard, Amazon ECS, and Amazon EKS\. Application Manager then displays those resources in predefined categories\.

For **Applications**, the list includes the following:
+ Custom applications
+ Launch Wizard
+ CloudFormation stacks
+ AppRegistry applications

For **Container clusters**, the list includes the following:
+ Amazon ECS clusters
+ Amazon EKS clusters

After import is complete, you can view operations information about your resources in these predefined categories\. Or, if you want to provide more context about a collection of resources, you can manually create an application in Application Manager and move resources or groups of resources into that application\. This allows you to view operations information in the context of an application\. 

After you [set up](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager-getting-started-related-services.html) and configure AWS services and Systems Manager capabilities, Application Manager displays the following types of information about your resources:
+ Information about the current state, status, and Amazon EC2 Auto Scaling health of the Amazon Elastic Compute Cloud \(Amazon EC2\) instances in your application
+ Alarms provided by Amazon CloudWatch
+ Compliance information provided by AWS Config and State Manager \(a component of Systems Manager\)
+ Kubernetes cluster information provided by Amazon EKS
+ Log data provided by AWS CloudTrail and Amazon CloudWatch Logs
+ OpsItems provided by Systems Manager OpsCenter
+ Resource details provided by the AWS services that host them\.
+ Container cluster information provided by Amazon ECS\.

To help you remediate issues with components or resources, Application Manager also provides runbooks that you can associate with your applications\. To get started with Application Manager, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/appmanager)\. In the navigation pane, choose **Application Manager**\.

## What are the benefits of using Application Manager?<a name="application-manager-learn-more-benefits"></a>

Application Manager reduces the time it takes for DevOps engineers to detect and investigate issues with AWS resources\. To do this, Application Manager displays many types of operations information in the context of an application in one console\. Application Manager also reduces the time it takes to remediate issues by providing runbooks that perform common remediation tasks on AWS resources\.

## What are the features of Application Manager?<a name="application-manager-learn-more-features"></a>

Application Manager includes the following features:
+ **Import your AWS resources automatically**

  During initial setup, you can choose to have Application Manager automatically import and display resources in your AWS account that are based on CloudFormation stacks, AWS Resource Groups, Launch Wizard deployments, AppRegistry applications, and Amazon ECS and Amazon EKS clusters\. The system displays these resources in predefined application or cluster categories\. Thereafter, whenever new resources of these types are added to your AWS account, Application Manager automatically displays the new resources in the predefined application and cluster categories\. 
+ **Create or edit CloudFormation stacks and templates**

  Application Manager helps you provision and manage resources for your applications by integrating with [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\. You can create, edit, and delete AWS CloudFormation templates and stacks in Application Manager\. Application Manager also includes a template library where you can clone, create, and store templates\. Application Manager and CloudFormation display the same information about the current status of a stack\. Templates and template updates are stored in Systems Manager until you provision the stack, at which time the changes are also displayed in CloudFormation\.
+ **View information about your instances in the context of an application**

  Application Manager integrates with Amazon Elastic Compute Cloud \(Amazon EC2\) to display information about your instances in the context of an application\. Application Manager displays instance state, status, and Amazon EC2 Auto Scaling health for a selected application in a graphical format\. The **Instances** tab also includes a table with the following information for each instance in your application\.
  + Instance state \(Pending, Stopping, Running, Stopped\)
  + Ping status for SSM Agent
  + Status and name of the last Systems Manager Automation runbook processed on the instance
  + A count of Amazon CloudWatch Logs alarms per state\.
    + `ALARM` – The metric or expression is outside of the defined threshold\.
    + `OK` – The metric or expression is within the defined threshold\.
    + `INSUFFICIENT_DATA` – The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\.
  + Auto Scaling group health for the parent and individual autoscaling groups
+ **View operational metrics and alarms for an application or cluster**

  Application Manager integrates with [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) to provide real\-time operational metrics and alarms for an application or cluster\. You can drill down into your application tree to view alarms at each component level, or view alarms for an individual cluster\.
+ **View log data for an application**

  Application Manager integrates with [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) to provide log data in the context of your application without having to leave Systems Manager\.
+ **View and manage OpsItems for an application or cluster** 

  Application Manager integrates with [AWS Systems Manager OpsCenter](OpsCenter.md) to provide a list of operational work items \(OpsItems\) for your applications and clusters\. The list reflects automatically generated and manually created OpsItems\. You can view details about the resource that created an OpsItem and the OpsItem status, source, and severity\. 
+ **View resource compliance data for an application or cluster** 

  Application Manager integrates with [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) to provide compliance and history details about your AWS resources according to rules you specify\. Application Manager also integrates with [AWS Systems Manager State Manager](systems-manager-state.md) to provide compliance information about the state you want to maintain for your Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. 
+ **View Amazon ECS and Amazon EKS cluster infrastructure information**

  Application Manager integrates with [Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/) and [Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) to provide information about the health of your cluster infrastructures and a component runtime view of the compute, networking, and storage resources in a cluster\.

  However, you can't manage or view operations information about your Amazon EKS pods or containers in Application Manager\. You can only manage and view operations information about the infrastructure that is hosting your Amazon EKS resources\.
+ **View resource cost details for an application**

  Application Manager is integrated with AWS Cost Explorer, a feature of AWS Billing and Cost Management, through the **Cost** widget\. After you enable Cost Explorer in the Billing and Cost Management console, the **Cost** widget in Application Manager shows cost data for a specific non\-container application or application component\. You can use filters in the widget to view cost data according to different time periods, granularities, and cost types in either a bar or line chart\. 
+ **View detailed resource information in one console**

  Choose a resource name listed in Application Manager and view contextual information and operations information about that resource without having to leave Systems Manager\.
+ **Receive automatic resource updates for applications** 

  If you make changes to a resource in a service console, and that resource is part of an application in Application Manager, then Systems Manager automatically displays those changes\. For example, if you update a stack in the AWS CloudFormation console, and if that stack is part of an Application Manager application, then the stack updates are automatically reflected in Application Manager\. 
+ **Discover Launch Wizard applications automatically**

  Application Manager is integrated with [AWS Launch Wizard](https://docs.aws.amazon.com/launchwizard/?id=docs_gateway)\. If you used Launch Wizard to deploy resources for an application, Application Manager can automatically import and display them in a Launch Wizard section\.
+ **Monitor application resources in Application Manager by using CloudWatch Application Insights**

  Application Manager integrates with Amazon CloudWatch Application Insights\. Application Insights identifies and sets up key metrics, logs, and alarms across your application resources and technology stack\. Application Insights continuously monitors metrics and logs to detect and correlate anomalies and errors\. When the system detects errors or anomalies, Application Insights generates CloudWatch Events that you can use to set up notifications or take actions\. You can enable and view Application Insights on the **Overview** and **Monitoring** tabs in Application Manager\. For more information about Application Insights, see [What is Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-what-is.html) in the *Amazon CloudWatch User Guide*\.
+ **Remediate issues with runbooks** 

  Application Manager includes predefined Systems Manager runbooks for remediating common issues with AWS resources\. You can execute a runbook against all of the applicable resources in an application without having to leave Application Manager\.

## Is there a charge to use Application Manager?<a name="application-manager-learn-more-cost"></a>

Application Manager is available at no additional charge\.

## What are the resource quotas for Application Manager?<a name="application-manager-learn-more-quotas"></a>

You can view quotas for all Systems Manager capabilities in the [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\. Unless otherwise noted, each quota is Region specific\.

**Topics**
+ [What are the benefits of using Application Manager?](#application-manager-learn-more-benefits)
+ [What are the features of Application Manager?](#application-manager-learn-more-features)
+ [Is there a charge to use Application Manager?](#application-manager-learn-more-cost)
+ [What are the resource quotas for Application Manager?](#application-manager-learn-more-quotas)
+ [Getting started with Systems Manager Application Manager](application-manager-getting-started.md)
+ [Working with Application Manager](application-manager-working.md)