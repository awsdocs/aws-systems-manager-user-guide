# Manually installing SSM Agent on CentOS instances<a name="agent-install-centos"></a>

The Amazon Machine Images \(AMIs\) for CentOS that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For a list of AWS managed AMIs on which the agent might be preinstalled, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

Use the information in this section to help you manually install or reinstall SSM Agent on a CentOS instance\.

**Before you begin**  
Before you install SSM Agent on a CentOS instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.
+ If you use a `yum` command to update SSM Agent on a managed node after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

**Topics**
+ [Install SSM Agent on CentOS 8\.x](agent-install-centos-8.md)
+ [Install SSM Agent on CentOS 7\.x](agent-install-centos-7.md)
+ [Install SSM Agent on CentOS 6\.x](agent-install-centos-6.md)