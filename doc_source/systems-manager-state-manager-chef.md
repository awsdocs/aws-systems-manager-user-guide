# Creating associations that run Chef recipes<a name="systems-manager-state-manager-chef"></a>

You can create State Manager associations that run Chef recipes by using the `AWS-ApplyChefRecipes` document\. You can target Linux\-based Systems Manager managed nodes with the `AWS-ApplyChefRecipes` document\. This document offers the following benefits for running Chef recipes:
+ Supports multiple releases of Chef \(Chef 11 through Chef 14\)\.
+ Automatically installs the Chef client software on target instances\.
+ Optionally runs [Systems Manager compliance checks](systems-manager-compliance.md) on target instances, and stores the results of compliance checks in an S3 bucket\.
+ Runs multiple cookbooks and recipes in a single run of the document\.
+ Optionally runs recipes in `why-run` mode, to show which recipes will change on target instances without making changes\.
+ Optionally applies custom JSON attributes to `chef-client` runs\.

You can use GitHub or S3 buckets as sources for Chef cookbooks and recipes that you specify in an `AWS-ApplyChefRecipes` document\.

## Prerequisites: Set up your association, repository, and cookbooks<a name="state-manager-chef-prereqs"></a>

Before you create an `AWS-ApplyChefRecipes` document, prepare your Chef cookbooks and cookbook repository\. If you don't already have a Chef cookbook that you want to use, you can get started by using a test `HelloWorld` cookbook that AWS has prepared for you\. The `AWS-ApplyChefRecipes` document already points to this cookbook by default\. Your cookbooks should be set up similarly to the following directory structure\. In the following example, `jenkins` and `nginx` are examples of Chef cookbooks that are available in the [Chef Supermarket](https://supermarket.chef.io/) on the Chef website\.

Though AWS cannot officially support cookbooks on the [Chef Supermarket](https://supermarket.chef.io/) website, many of them work with the `AWS-ApplyChefRecipes` document\. The following are examples of criteria to check when you are testing a community cookbook:
+ The cookbook should support the Linux\-based operating systems of the Systems Manager managed nodes that you are targeting\.
+ The cookbook should be valid for the Chef client version \(Chef 11 through Chef 14\) that you use\.
+ The cookbook is compatible with Chef Infra Client, and, does not require a Chef server\.

Verify that you can reach the Chef\.io website, so that any cookbooks you specify in your run list can be installed when the Systems Manager document runs\. Using a nested `cookbooks` folder is supported, but not required; you can store cookbooks directly under the root level\.

```
<Top-level directory, or the top level of the archive file (ZIP or tgz or tar.gz)>
    └── cookbooks (optional level)
        ├── jenkins
        │   ├── metadata.rb
        │   └── recipes
        └── nginx
            ├── metadata.rb
            └── recipes
```

**Important**  
Before you create a State Manager association that runs Chef recipes, be aware that the document run installs the Chef client software on your Systems Manager managed nodes, unless you set the value of **Chef client version** to `None`\. This action uses an installation script from Chef to install Chef components on your behalf\. Before you run an `AWS-ApplyChefRecipes` document, be sure your enterprise can comply with any applicable legal requirements, including license terms applicable to the use of Chef software\. For more information, see the [Chef website](https://www.chef.io/)\.

Systems Manager can deliver compliance reports to an S3 bucket, the Systems Manager console, or make compliance results available in response to Systems Manager API commands\. To run Systems Manager compliance reports, the instance profile attached to Systems Manager managed instances must have permissions to write to the S3 bucket\. The instance profile must have permissions to use the Systems Manager `PutComplianceItem` API\. For more information about Systems Manager compliance, see [AWS Systems Manager Configuration Compliance](systems-manager-compliance.md)\.

### Logging the document run<a name="state-manager-chef-logging"></a>

When you run a Systems Manager document by using a State Manager association, you can configure the association to choose the output of the document run, and you can send the output to Amazon S3 or Amazon CloudWatch Logs\. To help ease troubleshooting when an association has finished running, verify that the association is configured to write command output to either an S3 bucket or CloudWatch Logs\. For more information, see [Create an association](sysman-state-assoc.md)\.

## Use GitHub as a cookbook source<a name="state-manager-chef-github"></a>

The `AWS-ApplyChefRecipes` document uses the [`aws:downloadContent`](ssm-plugins.md#aws-downloadContent) plugin to download cookbooks\. To download content from GitHub, specify information about your GitHub repository to the document in JSON format\. The following is an example:

```
{
   "owner":"TestUser",
   "repository":"GitHubCookbookRepository",
   "path":"cookbooks/HelloWorld",
   "getOptions":"branch:master",
   "tokenInfo":"{{ssm-secure:secure-string-token}}"
}
```

## Use Amazon S3 as a cookbook source<a name="state-manager-chef-s3"></a>

You can also store and download Chef cookbooks in Amazon S3 as either a single `.zip` or `tar.gz` file or a directory structure\. To download content from Amazon S3, you must specify the path to the file\. Here are two examples:

**Example 1: Download a specific cookbook**

```
{
   "path":"https://s3.amazonaws.com/chef-cookbooks/HelloWorld.zip"
}
```

**Example 2: Download the contents of a directory**

```
{
   "path":"https://s3.amazonaws.com/chef-cookbooks-test/HelloWorld"
}
```

**Important**  
If you specify Amazon S3, the AWS Identity and Access Management \(IAM\) instance profile on your managed instances must be configured with the `AmazonS3ReadOnlyAccess` policy\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

**Topics**
+ [Prerequisites: Set up your association, repository, and cookbooks](#state-manager-chef-prereqs)
+ [Use GitHub as a cookbook source](#state-manager-chef-github)
+ [Use Amazon S3 as a cookbook source](#state-manager-chef-s3)
+ [Create an association that runs Chef recipes \(console\)](#state-manager-chef-console)
+ [Create an association that runs Chef recipes \(CLI\)](#state-manager-chef-cli)
+ [Viewing Chef resource compliance details](#state-manager-chef-compliance)

## Create an association that runs Chef recipes \(console\)<a name="state-manager-chef-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association that runs Chef cookbooks by using the `AWS-ApplyChefRecipes` document\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. For **Name**, enter a name that helps you remember the purpose of the association\.

1. In the **Document** list, choose **AWS\-ApplyChefRecipes**\.

1. In **Parameters**, for **Source Type**, choose either **GitHub** or **S3**\.

1. In **Source info**, enter cookbook source information in one of the following formats\.

   1. If you chose **GitHub** in step 5, enter repository information in the following format:

      ```
      {
         "owner":"user_name",
         "repository":"name",
         "path":"path_to_directory_or_cookbook_to_download",
         "getOptions":"branch:branch_name",
         "tokenInfo":"{{(Optional)_token_information}}"
      }
      ```

   1. If you chose **S3** in step 5, enter path information in the following format:

      ```
      {
         "path":"https://s3.amazonaws.com/path_to_directory_or_cookbook_to_download"
      }
      ```

1. In **Run list**, list the recipes that you want to run in the following format, separating each recipe with a comma as shown\. Do not include a space after the comma\.

   ```
   recipe[cookbook_name1::recipe_name],recipe[cookbook_name2::recipe_name]
   ```

1. \(Optional\) In **JSON attributes content**, add any custom JSON that contains attributes you want the Chef client to pass to your target instances\.

   The **JSON attributes content** parameter is best used for the following purposes:
   + When you want to override only a small number of attributes, and you do not otherwise need to use custom cookbooks\.

     Custom JSON can help you avoid the extra work of setting up and maintaining a cookbook repository to override only a few attributes\.
   + Values that are expected to vary\.

     For example, if your Chef cookbooks configure a third\-party application that accepts payments, you can use custom JSON to specify the payment endpoint URL\. If the third\-party software manufacturer changes the payment endpoint URL, you can use custom JSON to update the payment endpoint to the new URL\.

1. For **Chef client version**, specify a Chef version\. Valid values are `11`, `12`, `13`, `14`, or `None`\. If you specify `11` through `14`, Systems Manager installs the correct Chef client version on your target instances\. If you specify `None`, Systems Manager does not install the Chef client on target instances before running the document's recipes\. The default value is `14`\.

1. \(Optional\) For **Chef client arguments**, specify additional arguments that are supported for the version of Chef you are using\. To learn more about supported arguments, run `chef-client -h` on an instance that is running the Chef client\.

1. \(Optional\) Enable **Why\-run** to show changes that will be made to target instances if the recipes are run, without actually changing target instances\.

1. For **Compliance severity**, choose the severity of Systems Manager Configuration Compliance results that you want reported\. Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you specify\. Configuration Compliance reports are stored in an S3 bucket that you specify as the value of the **Compliance report bucket** parameter \(step 15\)\. For more information about Configuration Compliance, see [Working with Configuration Compliance](sysman-compliance-about.md) in this guide\.

   Compliance scans measure drift between configuration that is specified in your Chef recipes and instance resources\. Valid values are `Critical`, `High`, `Medium`, `Low`, `Informational`, `Unspecified`, or `None`\. To skip compliance reporting, choose `None`\.

1. For **Compliance type**, specify the compliance type for which you want results reported\. Valid values are `Association` for State Manager associations, or `Custom:`*custom\_type*\. The default value is `Custom:Chef`\.

1. For **Compliance report bucket**, enter the name of an S3 bucket in which to store information about every Chef run performed by this document, including resource configuration and Configuration Compliance results\.

1. In **Rate control**, configure options to run State Manager associations across a fleet of managed instances\. For information about using rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In **Concurrency**, choose an option:
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In **Error threshold**, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. Choose **Create Association**\.

## Create an association that runs Chef recipes \(CLI\)<a name="state-manager-chef-cli"></a>

The following procedure describes how to use the AWS CLI to create a State Manager association that runs Chef cookbooks by using the `AWS-ApplyChefRecipes` document\.

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run one of the following commands to create an association that runs Chef cookbooks by targeting instances using Amazon EC2 tags\. Command \(A\) uses GitHub as the source type\. Command \(B\) uses Amazon S3 as the source type\.

   **\(A\) GitHub source**

------
#### [ Linux ]

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" \
   --targets Key=tag:TagKey,Values=TagValue \
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"name\", \"path\": \"path_to_directory_or_cookbook_to_download\", \"getOptions\": \"branch:branch_name\"}"], "RunList":["{\"recipe[cookbook_name1::recipe_name]\", \"recipe[cookbook_name2::recipe_name]\"}"], "JsonAttributesContent": ["{Custom_JSON}"], "ChefClientVersion": ["version_number"], "ChefClientArguments":["{chef_client_arguments}"], "WhyRun": true_or_false, "ComplianceSeverity": ["severity_value"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["DOC-EXAMPLE-BUCKET"]}' \
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" ^
   --targets Key=tag:TagKey,Values=TagValue ^
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"name\", \"path\": \"path_to_directory_or_cookbook_to_download\", \"getOptions\": \"branch:branch_name\"}"], "RunList":["{\"recipe[cookbook_name1::recipe_name]\", \"recipe[cookbook_name2::recipe_name]\"}"], "JsonAttributesContent": ["{Custom_JSON}"], "ChefClientVersion": ["version_number"], "ChefClientArguments":["{chef_client_arguments}"], "WhyRun": true_or_false, "ComplianceSeverity": ["severity_value"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["DOC-EXAMPLE-BUCKET"]}' ^
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------

   Here is an example:

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" \
   --targets Key=tag:OS,Values=Linux \
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"ChefRecipeTest\", \"repository\": \"ChefCookbooks\", \"path\": \"cookbooks/HelloWorld\", \"getOptions\": \"branch:master\"}"], "RunList":["{\"recipe[HelloWorld::HelloWorldRecipe]\", \"recipe[HelloWorld::InstallApp]\"}"], "JsonAttributesContent": ["{\"state\": \"visible\",\"colors\": {\"foreground\": \"light-blue\",\"background\": \"dark-gray\"}}"], "ChefClientVersion": ["14"], "ChefClientArguments":["{--fips}"], "WhyRun": false, "ComplianceSeverity": ["Medium"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["ChefComplianceResultsBucket"]}' \
   --association-name "MyChefAssociation" --schedule-expression "cron(0 2 ? * SUN *)"
   ```

   **\(B\) S3 source**

------
#### [ Linux ]

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" \
   --targets Key=tag:TagKey,Values=TagValue \
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_Zip_file,_directory,_or_cookbook_to_download\"}"], "RunList":["{\"recipe[cookbook_name1::recipe_name]\", \"recipe[cookbook_name2::recipe_name]\"}"], "JsonAttributesContent": ["{Custom_JSON}"], "ChefClientVersion": ["version_number"], "ChefClientArguments":["{chef_client_arguments}"], "WhyRun": true_or_false, "ComplianceSeverity": ["severity_value"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["DOC-EXAMPLE-BUCKET"]}' \
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" ^
   --targets Key=tag:TagKey,Values=TagValue ^
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_Zip_file,_directory,_or_cookbook_to_download\"}"], "RunList":["{\"recipe[cookbook_name1::recipe_name]\", \"recipe[cookbook_name2::recipe_name]\"}"], "JsonAttributesContent": ["{Custom_JSON}"], "ChefClientVersion": ["version_number"], "ChefClientArguments":["{chef_client_arguments}"], "WhyRun": true_or_false, "ComplianceSeverity": ["severity_value"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["DOC-EXAMPLE-BUCKET"]}' ^
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------

   Here is an example:

   ```
   aws ssm create-association --name "AWS-ApplyChefRecipes" ^
   --targets "Key=tag:OS,Values= Windows" ^
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/DOC-EXAMPLE-BUCKET/HelloWorld\"}"], "RunList":["{\"recipe[HelloWorld::HelloWorldRecipe]\", \"recipe[HelloWorld::InstallApp]\"}"], "JsonAttributesContent": ["{\"state\": \"visible\",\"colors\": {\"foreground\": \"light-blue\",\"background\": \"dark-gray\"}}"], "ChefClientVersion": ["14"], "ChefClientArguments":["{--fips}"], "WhyRun": false, "ComplianceSeverity": ["Medium"], "ComplianceType": ["Custom:Chef"], "ComplianceReportBucket": ["ChefComplianceResultsBucket"]}' ^
   --association-name "name" --schedule-expression "cron(0 2 ? * SUN *)"
   ```
**Note**  
State Manager associations do not support all cron and rate expressions\. For more information about creating cron and rate expressions for associations, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   The system attempts to create the association on the instances and immediately apply the state\.

1. Run the following command to view an updated status of the association you just created\. 

   ```
   aws ssm describe-association --association-id "ID"
   ```

## Viewing Chef resource compliance details<a name="state-manager-chef-compliance"></a>

Systems Manager captures compliance information about Chef\-managed resources in the Amazon Simple Storage Service \(Amazon S3\) **Compliance report bucket** value that you specified when you ran the `AWS-ApplyChefRecipes` document\. Searching for information about Chef resource failures in an S3 bucket can be time consuming\. Instead, you can view this information on the Systems Manager **Compliance** page\.

A Systems Manager Compliance scan collects information about resources on your managed nodes that were created or checked in the most recent Chef run\. The resources can include files, directories, `systemd` services, `yum` packages, templated files, `gem` packages, and dependent cookbooks, among others\.

The **Compliance resources summary** section displays a count of resources that failed\. In the following example, the **ComplianceType** is **Custom:Chef** and one resource is noncompliant\.

**Note**  
`Custom:Chef` is the default **ComplianceType** value in the `AWS-ApplyChefRecipes` document\. This value is customizable\.

![\[Viewing counts in the Compliance resources summary section of the Compliance page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-chef-compliance-summary.png)

The **Details overview for resources** section shows information about the AWS resource that is not in compliance\. This section also includes the Chef resource type against which compliance was run, severity of issue, compliance status, and links to more information when applicable\.

![\[Viewing compliance details for a Chef managed resource failure\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/state-manager-chef-compliance-details.png)

**View output** shows the last 4,000 characters of the detailed status\. Systems Manager starts with the exception as the first element, and then finds verbose messages and shows as many as it can until it reaches the 4000 character limit\. This process displays the log messages that were output before the exception was thrown, which are the most relevant messages for troubleshooting\.

For information about how to view compliance information, see [AWS Systems Manager Configuration Compliance](systems-manager-compliance.md)\.

### Association failures affect compliance reporting<a name="state-manager-chef-compliance-reporting"></a>

If the State Manager association fails, no compliance data is reported\. For example, if Systems Manager attempts to download a Chef cookbook from an S3 bucket that the instance doesn't have permission to access, the association fails, and Systems Manager reports no compliance data\.