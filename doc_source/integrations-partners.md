# Integration with other products and services<a name="integrations-partners"></a>

AWS Systems Manager has built\-in integration for the products and services shown in the following table\.


|  |  | 
| --- |--- |
|  Ansible  |  [Ansible](https://www.ansible.com/) is an IT automation platform that makes your applications and systems easier to deploy\. Systems Manager provides the Systems Manager document \(SSM document\) `AWS-ApplyAnsiblePlaybooks` which allows you to create State Manager associations that run Ansible playbooks\.  Learn more [Walkthrough: Creating associations that run Ansible playbooks](systems-manager-state-manager-ansible.md)   | 
|  Chef  |  [Chef](https://www.chef.io/) is an IT automation tool that makes your applications and systems easier to deploy\. Systems Manager provides the `AWS-ApplyChefRecipes` SSM document, which allows you to create associations in State Manager, a capability of AWS Systems Manager, that run Chef recipes\.  Learn more [Walkthrough: Creating associations that run Chef recipes](systems-manager-state-manager-chef.md)  Systems Manager also integrates with [Chef InSpec](https://www.chef.io/products/chef-inspec/) profiles, allowing you to run compliance scans and view compliant and noncompliant instances\.  Learn more [Using Chef InSpec profiles with Systems Manager Compliance](integration-chef-inspec.md)   | 
|  GitHub  |  [GitHub](https://github.com/) provides hosting for software development version control and collaboration\. Systems Manager provides the SSM document `AWS-RunDocument`, which allows you to run other SSM documents stored in GitHub, and the SSM document `AWS-RunRemoteScript` , which allows you to run scripts stored in GitHub\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-partners.html)  | 
|  Jenkins  |  [Jenkins](https://www.jenkins.io/) is an open\-source automation server that allows developers to reliably build, test, and deploy their software\. Automation, a capability of Systems Manager, can be used as a post\-build step to pre\-install application releases into Amazon Machine Images \(AMIs\)\.  Learn more [Walkthrough: Using Automation with Jenkins](automation-jenkins.md)   | 

**Topics**
+ [Running scripts from GitHub](integration-remote-scripts.md)