# Adding applications and clusters to Application Manager<a name="application-manager-getting-started-adding-applications"></a>

Application Manager is a component of AWS Systems Manager\. In Application Manager, an *application* is a logical group of AWS resources that you want to operate as a unit\. This logical group can represent different versions of an application, ownership boundaries for operators, or developer environments, to name a few\.

When you choose **Get started** on the Application Manager home page, Application Manager automatically imports metadata about your resources that were created in other AWS services or Systems Manager capabilities\. For applications, Application Manager imports metadata about all of your AWS resources organized into resource groups\. Each resource group is listed in the **Custom applications** category as a unique application\. Application Manager also automatically imports metadata about resources that were created by AWS CloudFormation, AWS Launch Wizard, Amazon Elastic Container Service \(Amazon ECS\), and Amazon Elastic Kubernetes Service \(Amazon EKS\)\. Application Manager then displays those resources in predefined categories\.

For **Applications**, the list includes the following:
+ Custom applications
+ Launch Wizard
+ CloudFormation stacks
+ AppRegistry applications

For **Container clusters**, the list includes the following:
+ Amazon ECS clusters
+ Amazon EKS clusters

After import completes, you can view operations information for an application or a specific resource in these predefined categories\. Or, if you want to provide more context about a collection of resources, you can manually create an application in Application Manager\. You can then add resources or groups of resources into that application\. After you create an application in Application Manager, you can view operations information about your resource in the context of an application\. 

## Creating an application in Application Manager<a name="application-manager-create-application"></a>

Use the following procedure to create an application in Application Manager and add resources to that application\. 

**To create an application in Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. Choose the **Applications** tab, and then choose **Create application**\.

1. For **Application name**, enter a name to help you understand the purpose of the resources that will be added to this application\.

1. For **Application description**, enter information about this application\.

1. In the **Choose application components** section, use the options provided to choose resources for this application\. You can add a combination of tagged resources, resource groups, and stacks to an application\. You must choose a minimum of two components and a maximum of 15\. If you choose resources by using tags, then all resources assigned those tags will be listed on the **Resources** tab after you add the new application\. This is also true for resources included in a resource group or in a stack\. 

   If you don't see the resources you want to add to the application, then verify the resources have been tagged properly, added to an AWS Resource Groups group, or added to an AWS CloudFormation stack\.

1. For **Application tags \- optional**, specify tags for this application\.

1. Choose **Create**\.

Application Manager creates the application and opens it\. The **Components** tree lists the new application as the top\-level component and the resources, groups, or stacks you selected as subcomponents\. The next time you open Application Manager, you can find the new application in the **Custom applications** category\.