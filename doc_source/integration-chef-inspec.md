# Using Chef InSpec Profiles with Systems Manager Compliance<a name="integration-chef-inspec"></a>

Systems Manager now integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that enables you to create human\-readable profiles on GitHub or Amazon S3\. Then you can use Systems Manager to run compliance scans and view compliant and noncompliant instances\. A *profile* is a security, compliance, or policy requirement for your computing environment\. For example, you can create profiles that perform the following checks when you scan your instances with Systems Manager Compliance:
+ Check if specific ports are open or closed\.
+ Check if specific applications are running\.
+ Check if certain packages are installed\.
+ Check Windows Registry keys for specific properties\.

You can create InSpec profiles for Amazon EC2 instances and on\-premises servers or virtual machines \(VMs\) that you manage with Systems Manager\. The following sample Chef InSpec profile checks to see if port 22 is open\.

```
control 'Scan Port' do
  impact 10.0
  title 'Server: Configure the service port'
  desc 'Always specify which port the SSH server should listen to.
   Prevent unexpected settings.'
  describe sshd_config do
   its('Port') { should eq('22') }
  end
end
```

InSpec includes a collection of resources that help you quickly write checks and auditing controls\. InSpec uses the [InSpec Domain\-specific Language \(DSL\)](https://www.inspec.io/docs/reference/dsl_inspec/) for writing these controls in Ruby\. You can also use profiles created by a large community of InSpec users\. For example, the [DevSec chef\-os\-hardening](https://github.com/dev-sec/chef-os-hardening) project on GitHub includes dozens of profiles to help you secure your instances and servers\. You can author and store profiles in GitHub or Amazon Simple Storage Service \(Amazon S3\)\. 

## How It Works<a name="integration-chef-inspec-how"></a>

Here is how the process of using InSpec profiles with Systems Manager Compliance works\.

1. Either identify predefined InSpec profiles that you want to use, or create your own\. You can use [predefined profiles](https://github.com/search?p=1&q=topic%3Ainspec+org%3Adev-sec&type=Repositories) on GitHub to get started\. For information about how to create your own InSpec profiles, see [Compliance Automation with InSpec](https://learn.chef.io/modules/learn-the-inspec-basics#/)\.

1. Store profiles in either a public or private GitHub repository, or in an Amazon S3 bucket\.

1. Run Compliance with your InSpec profiles by using the AWS\-RunInSpecChecks SSM document\. You can begin a Compliance scan by using Run Command \(for on\-demand scans\), or you can schedule regular Compliance scans by using State Manager\.

1. Identify noncompliant instances by using the Compliance API or the Systems Manager Compliance console\.

**Note**  
Chef uses a client on your instances to process the profile\. You don't need to install the client\. When Systems Manager executes the AWS\-RunInSpecChecks SSM document, the system checks if the client is installed\. If not, Systems Manager installs the Chef client during the scan, and then uninstalls the client after the scan is completed\.

## Running an InSpec Compliance Scan<a name="integration-chef-inspec-running"></a>

This section includes information about how to execute an InSpec Compliance scan by using the Systems Manager console and the AWS CLI\. The console procedure shows you how to configure State Manager to run the scan\. The AWS CLI procedure shows you how to configure Run Command to run the scan\.

### Running an InSpec Compliance Scan with State Manager by Using the Console<a name="integration-chef-inspec-running-console"></a>

**To run an InSpec Compliance scan with State Manager by using the AWS Systems Manager console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose **Create Association**\.

1. In the **Provide association details** section, enter a name\.

1. In the **Command document** list, choose **AWS\-RunInSpecChecks**\.

1. In the **Document version** list, choose **Latest at runtime**\.

1. In the **Parameters** section, in the **Source Type** list, choose either **GitHub** or **S3**\.

   If you choose **GitHub**, then enter the path to an InSpec profile in either a public or private GitHub repository in the **Source Info** field\. Here is an example path to a public profile provided by the Systems Manager team from the following location: [https://github\.com/awslabs/amazon\-ssm/tree/master/Compliance/InSpec/PortCheck](https://github.com/awslabs/amazon-ssm/tree/master/Compliance/InSpec/PortCheck)\.

   ```
   {"owner":"awslabs","repository":"amazon-ssm","path":"Compliance/InSpec/PortCheck","getOptions":"branch:master"}
   ```

   If you choose **S3**, then enter a valid URL to an InSpec profile in an Amazon S3 bucket in the **Source Info** field\. 

   For more information about how Systems Manager integrates with GitHub and Amazon S3, see [Running Scripts from GitHub and Amazon S3](integration-remote-scripts.md)\. 

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Specify schedule** section, use the schedule builder options to create a schedule for when you want the Compliance scan to run\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. Choose **Create Association**\. The system creates the association and automatically runs the Compliance scan\.

1. Wait several minutes for the scan to complete, and then choose **Compliance** in the navigation pane\.

1. In **Corresponding managed instances**, locate instances where the **Compliance Type** column is **Custom:Inspec**\.

1. Choose an instance ID to view the details of noncompliant statuses\.

### Running an InSpec Compliance Scan with Run Command by Using the AWS CLI<a name="integration-chef-inspec-running-cli"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute one of the following commands to run an InSpec profile from either GitHub or Amazon S3\.

   The command takes the following parameters:
   + **sourceType**: GitHub or Amazon S3
   + **sourceInfo**: URL to the InSpec profile folder either in GitHub or an Amazon S3 bucket\. The folder must contain the base InSpec file \(\*\.yml\) and all related controls \(\*\.rb\)\.

   **GitHub**

   ```
   aws ssm send-command --document-name "AWS-RunInSpecChecks" --targets '[{"Key":"tag:tag_name","Values":["tag_value"]}]' --parameters '{"sourceType":["GitHub"],"sourceInfo":["{\"owner\":\"owner_name\", \"repository\":\"repository_name\", \"path\": \"Inspec.yml_file"}"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunInSpecChecks" --targets '[{"Key":"tag:testEnvironment","Values":["webServers"]}]' --parameters '{"sourceType":["GitHub"],"getOptions":"branch:master","sourceInfo":["{\"owner\":\"awslabs\", \"repository\":\"amazon-ssm\", \"path\": \"Compliance/InSpec/PortCheck\"}"]}'
   ```

   **Amazon S3**

   ```
   aws ssm send-command --document-name "AWS-RunInSpecChecks" --targets '[{"Key":"tag:tag_name","Values":["tag_value"]}]' --parameters'{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/directory/Inspec.yml_file\"}"]}'
   ```

   Here is an example\.

   ```
   aws ssm send-command --document-name "AWS-RunInSpecChecks" --targets '[{"Key":"tag:testEnvironment","Values":["webServers"]}]' --parameters'{"sourceType":["S3"],"sourceInfo":["{\"path\":\"https://s3.amazonaws.com/Compliance/InSpec/PortCheck.yml\"}"]}' 
   ```

1. Execute the following command to view a summary of the Compliance scan\.

   ```
   aws ssm list-resource-compliance-summaries --filters Key=ComplianceType,Values=Custom:Inspec
   ```

1. Execute the following command to drill down into an instance that is not compliant\.

   ```
   aws ssm list-compliance-items --resource-ids instance_ID --resource-type ManagedInstance --filters Key=DocumentName,Values=AWS-RunInSpecChecks
   ```

**Related AWS Services**  
The following related services can help you assess Compliance and work with Chef\.
+ [Amazon Inspector](http://docs.aws.amazon.com/inspector/latest/APIReference/) lets you perform security assessments on your instances based on and common vulnerabilities described in Central Internet Security \(CIS\) standards\.
+ [AWS OpsWorks for Chef Automate](http://docs.aws.amazon.com/opsworks-cm/latest/APIReference/) lets you run a Chef Automate server in AWS\. 