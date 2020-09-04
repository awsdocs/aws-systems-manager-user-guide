# Creating associations that run Ansible playbooks<a name="systems-manager-state-manager-ansible"></a>

You can create State Manager associations that run Ansible playbooks by using the `AWS-ApplyAnsiblePlaybooks` document\. This document offers the following benefits for running playbooks:
+ Support for running complex playbooks
+ Support for downloading playbooks from GitHub and Amazon Simple Storage Service \(Amazon S3\)
+ Support for compressed playbook structure
+ Enhanced logging
+ Ability to specify which playbook to run when playbooks are bundled

**Note**  
Systems Manager includes two SSM documents that enable you to create State Manager associations that run Ansible playbooks: `AWS-RunAnsiblePlaybook` and `AWS-ApplyAnsiblePlaybooks`\. The `AWS-RunAnsiblePlaybook` document is deprecated\. It remains available in Systems Manager for legacy purposes\. We recommend that you use the `AWS-ApplyAnsiblePlaybooks` document because of the enhancements described here\.

**Support for running complex playbooks**

The `AWS-ApplyAnsiblePlaybooks` document supports bundled, complex playbooks because it copies the entire file structure to a local directory before executing the specified main playbook\. You can provide source playbooks in Zip files or in a directory structure\. The Zip file or directory can be stored in GitHub or Amazon S3\. 

**Support for downloading playbooks from GitHub**

The `AWS-ApplyAnsiblePlaybooks` document uses the `aws:downloadContent` plugin to download playbook files\. Files can be stored in GitHub in a single file or as a combined set of playbook files\. To download content from GitHub, you must specify information about your GitHub repository in JSON format\. Here is an example:

```
{
   "owner":"TestUser",
   "repository":"GitHubTest",
   "path":"scripts/python/test-script",
   "getOptions":"branch:master",
   "tokenInfo":"{{ssm-secure:secure-string-token}}"
}
```

**Support for downloading playbooks from Amazon S3**

You can also store and download Ansible playbooks in Amazon S3 as either a single \.zip file or a directory structure\. To download content from Amazon S3, you must specify the path to the file\. Here are two examples:

**Example 1: Download a specific playbook file**

```
{
   "path":"https://s3.amazonaws.com/aws-execute-ansible-test/ansible/playbook.yml"
}
```

**Example 2: Download the contents of a directory**

```
{
   "path":"https://s3.amazonaws.com/aws-execute-ansible-test/ansible/webservers/"
}
```

**Important**  
If you specify Amazon S3, then the AWS Identity and Access Management \(IAM\) instance profile on your managed instances must be configured with the `AmazonS3ReadOnlyAccess` policy\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 

**Support for compressed playbook structure**

The `AWS-ApplyAnsiblePlaybooks` document enables you to run compressed \.zip files in the downloaded bundle\. The document checks if the downloaded files contain a compressed file in \.zip format\. If a \.zip is found, the document automatically decompresses the file and then runs the specified Ansible automation\.

**Enhanced logging**

The `AWS-ApplyAnsiblePlaybooks` document includes an optional parameter for specifying different levels of logging\. Specify \-v for low verbosity, \-vv or –vvv for medium verbosity, and \-vvvv for debug level logging\. These options directly map to Ansible verbosity options\.

**Ability to specify which playbook to run when playbooks are bundled**

The `AWS-ApplyAnsiblePlaybooks` document includes a required parameter for specifying which playbook to run when multiple playbooks are bundled\. This option provides flexibility for running playbooks to support different use cases\.

## Installed dependencies<a name="systems-manager-state-manager-ansible-depedencies"></a>

If you specify **True** for the **InstallDependencies** parameter, then Systems Manager verifies that the following dependencies are installed on your instances\. If one or more of these dependencies are not found, then Systems Manager automatically installs them\.
+ **Ubuntu/Debian**: Apt\-get \(Package Management\), Python 3, Ansible, Unzip
+ **Amazon Linux**: Ansible
+ **RHEL**: Python 3, Ansible, Unzip

## Create an association that runs Ansible playbooks \(console\)<a name="systems-manager-state-manager-ansible-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association that runs Ansible playbooks by using the `AWS-ApplyAnsiblePlaybooks` document\.

**To create an association that runs Ansible playbooks \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. For **Name**, specify a name that helps you remember the purpose of the association\.

1. In the **Document** list, choose **AWS\-ApplyAnsiblePlaybooks**\.

1. In the **Parameters** section, for **Source Type**, choose either **GitHub** or **S3**\.

   **GitHub**

   If you choose **GitHub**, enter repository information in the following format:

   ```
   {
      "owner":"user_name",
      "repository":"name",
      "path":"path_to_directory_or_playbook_to_download",
      "getOptions":"branch:branch_name",
      "tokenInfo":"{{(Optional)_token_information}}"
   }
   ```

   **S3**

   If you choose **S3**, enter path information in the following format:

   ```
   {
      "path":"https://s3.amazonaws.com/path_to_directory_or_playbook_to_download"
   }
   ```

1. For **Install Dependencies**, choose an option\.

1. \(Optional\) For **Playbook File**, enter a file name\. If the playbook is contained in a Zip file, then you must specify a relative path to the Zip file\.

1. \(Optional\) For **Extra Variables**, enter variables that you want State Manager to send to Ansible at runtime\.

1. \(Optional\) For **Check**, choose an option\.

1. \(Optional\) For **Verbose**, choose an option\.

1. For **Targets**, choose an option\. For information about using targets, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. In the **Specify schedule** section, choose either **On schedule** or **No schedule**\. If you choose **On schedule**, then use the buttons provided to create a cron or rate schedule for the association\. 

1. In the **Advanced options** section, for **Compliance severity**, choose a severity level for the association\. Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you indicate here\. For more information, see [About State Manager association compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, configure options to run State Manager associations across a fleet of managed instances\. For information about using rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. Choose **Create Association**\.

**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 

## Create an association that runs Ansible playbooks \(CLI\)<a name="systems-manager-state-manager-ansible-cli"></a>

The following procedure describes how to use the AWS CLI to create a State Manager association that runs Ansible playbooks by using the `AWS-ApplyAnsiblePlaybooks` document\. 

**To create an association that runs Ansible playbooks \(CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run one of the following commands to create an association that runs Ansible playbooks by targeting instances using Amazon EC2 tags\. Command \(A\) specifies GitHub as the source type\. Command \(B\) specifies Amazon S3 as the source type\.

   **\(A\) GitHub source**

------
#### [ Linux ]

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" \
   --targets Key=tag:TagKey,Values=TagValue \
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"name\", \"getOptions\": \"branch:master\"}"],"InstallDependencies":["True_or_False"],"PlaybookFile":["file_name.yml"],"ExtraVariables":["key/value_pairs_separated_by_a_space"],"Check":["True_or_False"],"Verbose":["-v,-vv,-vvv, or -vvvv"]}' \
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" ^
   --targets Key=tag:TagKey,Values=TagValue ^
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"owner_name\", \"repository\": \"name\", \"getOptions\": \"branch:master\"}"],"InstallDependencies":["True_or_False"],"PlaybookFile":["file_name.yml"],"ExtraVariables":["key/value_pairs_separated_by_a_space"],"Check":["True_or_False"],"Verbose":["-v,-vv,-vvv, or -vvvv"]}' ^
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------

   Here is an example:

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" \
   --targets "Key=tag:OS,Values=Linux" \
   --parameters '{"SourceType":["GitHub"],"SourceInfo":["{\"owner\":\"ansibleDocumentTest\", \"repository\": \"Ansible\", \"getOptions\": \"branch:master\"}"],"InstallDependencies":["True"],"PlaybookFile":["hello-world-playbook.yml"],"ExtraVariables":["SSM=True"],"Check":["False"],"Verbose":["-v"]}' \
   --association-name "AnsibleAssociation" --schedule-expression "cron(0 2 ? * SUN *)"
   ```

   **\(B\) S3 source**

------
#### [ Linux ]

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" \
   --targets Key=tag:TagKey,Values=TagValue \
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_Zip_file,_directory,_or_playbook_to_download\"}"],"InstallDependencies":["True_or_False"],"PlaybookFile":["file_name.yml"],"ExtraVariables":["key/value_pairs_separated_by_a_space"],"Check":["True_or_False"],"Verbose":["-v,-vv,-vvv, or -vvvv"]}' \
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" ^
   --targets Key=tag:TagKey,Values=TagValue ^
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/path_to_Zip_file,_directory,_or_playbook_to_download\"}"],"InstallDependencies":["True_or_False"],"PlaybookFile":["file_name.yml"],"ExtraVariables":["key/value_pairs_separated_by_a_space"],"Check":["True_or_False"],"Verbose":["-v,-vv,-vvv, or -vvvv"]}' ^
   --association-name "name" --schedule-expression "cron_or_rate_expression"
   ```

------

   Here is an example:

   ```
   aws ssm create-association --name "AWS-ApplyAnsiblePlaybooks" \
   --targets "Key=tag:OS,Values= Windows" \
   --parameters '{"SourceType":["S3"],"SourceInfo":["{\"path\":\"https://s3.amazonaws.com/DOC-EXAMPLE-BUCKET/playbook.yml\"}"],"InstallDependencies":["True"],"PlaybookFile":["playbook.yml"],"ExtraVariables":["SSM=True"],"Check":["False"],"Verbose":["-v"]}' \
   --association-name "AnsibleAssociation" --schedule-expression "cron(0 2 ? * SUN *)"
   ```
**Note**  
State Manager associations do not support all cron and rate expressions\. For more information about creating cron and rate expressions for associations, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   The system attempts to create the association on the instances and immediately apply the state\. 

1. Run the following command to view an updated status of the association you just created\. 

   ```
   aws ssm describe-association --association-id "ID"
   ```