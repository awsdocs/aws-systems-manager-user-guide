# Step 1: Verify Session Manager Prerequisites<a name="session-manager-prerequisites"></a>

Before using Session Manager, make sure your environment meets the following requirements\.


**Session Manager Prerequisites**  

| Requirement | Description | 
| --- | --- | 
|  Supported EC2 Operating Systems  |  Amazon EC2 instances you want to use with Session Manager must run a supported version of Windows or Linux\. **Windows** Windows Server 2008 R2 through Windows Server 2016  Microsoft Windows Server 2016 Nano is not supported\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-prerequisites.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-prerequisites.html)  | 
|  SSM Agent  |  SSM Agent version 2\.3\.68\.0 or later must be installed on the instances you want to connect to through sessions\.  When a version of SSM Agent that supports Session Manager starts on an instance, it creates a user account with root or administrator privileges called `ssm-user`\. Sessions are launched using the credentials of this user account\. To install or update SSM Agent, see [Installing and Configuring SSM Agent](ssm-agent.md)\.  | 
|  AWS CLI  |  \(Optional\) If you use the AWS CLI to start your sessions \(instead of using the AWS Systems Manager console\), version 1\.16\.12 or later of the CLI must be installed on your local machine\. You can call aws \-\-version to check the version\. If you need to install or upgrade the CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the AWS Command Line Interface User Guide\. In addition, to use the CLI to manage your instances with Session Manager, you must first install the Session Manager plugin on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.  | 