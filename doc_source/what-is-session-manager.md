# Learn More About Session Manager<a name="what-is-session-manager"></a>

Refer to the following topics to learn more about AWS Systems Manager Session Manager\.

**Topics**
+ [How can Session Manager benefit my organization?](#session-manager-benefits)
+ [Who Should Use Session Manager?](#session-manager-who)
+ [What Are the Main Features of Session Manager?](#session-manager-features)
+ [What Is a Session?](#what-is-a-session)

## How can Session Manager benefit my organization?<a name="session-manager-benefits"></a>

Session Manager offers these benefits:
+ **Centralized access control to instances using IAM policies**

  Administrators have a single place to grant and revoke access to instances\. Using only AWS Identity and Access Management \(IAM\) policies, you can control which individual users or groups in your organization can use Session Manager and which instances they can access\. 
+ **No open inbound ports and no need to manage bastion hosts or SSH keys**

  Leaving inbound SSH ports and remote PowerShell ports open on your instances greatly increases the risk of entities running unauthorized or malicious commands on the instances\. Session Manager helps you improve your security posture by letting you close these inbound ports, freeing you from managing SSH keys and certificates, bastion hosts, and jump boxes\.
+ **One\-click access to instances from the console and CLI**

  Using the AWS Systems Manager console, you can start a session with a single click\. Because permissions to instances are provided through IAM policies instead of SSH keys or other mechanisms, the connection time is greatly reduced\.
+ **Cross\-platform support for both Windows and Linux**

  Session Manager provides both Windows and Linux support from a single tool\. For example, you don't need to use an SSH client for Linux instances and an RDP connection for Windows instances\.
+ **Logging and auditing session activity**

  To meet operational or security requirements in your organization, you might need to provide a record of the connections made to your instances and the commands that were run on them\. You can also receive notifications when a user in your organization starts or ends session activity\. 

  Logging and auditing capabilities are provided through integration with the following AWS services:
  + **AWS CloudTrail** – AWS CloudTrail captures information about Session Manager API calls made in your AWS account and writes it to log files that are stored in an Amazon S3 bucket you specify\. One bucket is used for all CloudTrail logs for your account\. For more information, see [Log AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\. 
  + **Amazon Simple Storage Service** – You can choose to store session log data in an Amazon S3 bucket of your choice for auditing purposes\. Log data can be sent to your S3 bucket with or without KMS encryption using your AWS Key Management Service \(AWS KMS\) key\. For more information, see [Logging Session Data Using Amazon S3 \(Console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-s3)\.
  + **Amazon CloudWatch Logs** – CloudWatch Logs lets you monitor, store, and access log files from various AWS services\. You can send session log data to a CloudWatch Logs log group for auditing purposes\. Log data can be sent to your log group with or without KMS encryption using your AWS KMS key\. For more information, see [Logging Session Data Using Amazon CloudWatch Logs \(Console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-logs)\.
  + **Amazon CloudWatch Events** and **Amazon Simple Notification Service** – CloudWatch Events lets you set up rules to detect when changes happen to AWS resources that you specify\. You can create a rule to detect when a user in your organization starts or stops a session, and then receive a notification through Amazon SNS \(for example, a text or email message\) about the event\. You can also configure a CloudWatch event to trigger other responses\. For more information, see [Monitoring Session Activity Using Amazon CloudWatch Events \(Console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-events)\.

## Who Should Use Session Manager?<a name="session-manager-who"></a>
+ Any AWS customer who wants to improve their security and audit posture, reduce operational overhead by centralizing access control on instances, and reduce inbound instance access\. 
+ Information Security experts who want to monitor and track instance access and activity, close down inbound ports on instances, or enable connections to instances that do not have a public IP address\. 
+ Administrators who want to grant and revoke access from a single location, and who want to provide one solution to users for both Windows and Linux instances\.
+ End users who want to connect to an instance with just one click from the browser or CLI without having to provide SSH keys\.

## What Are the Main Features of Session Manager?<a name="session-manager-features"></a>
+ **Support for both Windows and Linux instances**

  Session Manager lets you establish secure connections to your Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. On\-premises instances are not supported at this time\. For a list of supported Windows and Linux operating system types, see [Getting Started with Session Manager](session-manager-getting-started.md)\.
+ **Console, CLI, and SDK access to Session Manager capabilities**

  You can work with Session Manager in the following ways:

  The **AWS Systems Manager console** includes access to all the Session Manager capabilities for both administrators and end\-users\. You can perform any task that's related to your sessions by using the Systems Manager console\. 

  The **AWS CLI** includes access to Session Manager capabilities for end users\. You can start a session, view a list of sessions, and permanently terminate a session by using the AWS CLI\. 
**Note**  
To use the AWS CLI to run session commands, you must be using version 1\.16\.12 of the CLI \(or later\), and you must have installed the Session Manager plugin on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

  The **Session Manager SDK** consists of libraries and sample code that enables application developers to build frontend applications, such as custom shells or self\-service portals for internal users that natively use Session Manager to connect to instances\. Developers and partners can integrate Session Manager into their client\-side tooling or Automation workflows using the Session Manager APIs\. You can even build custom solutions\.
+ **IAM access control**

  Through the use of IAM policies, you can control which members of your organization can initiate sessions to instances and which instances they can access\. You can also provide temporary access to your instances\. For example, you might want to give an on\-call engineer \(or a group of on\-call engineers\) access to production servers only for the duration of their rotation\.
+ **Logging and auditing capability support**

  Session Manager provide you with options for auditing and logging session histories in your AWS account through integration with a number of other AWS services\. For more information, see [Auditing and Logging Session Activity](session-manager-logging-auditing.md)\.
+ **AWS PrivateLink support for instances without public IP addresses**

  You can also set up VPC Endpoints for Systems Manager using AWS PrivateLink to further secure your sessions\. PrivateLink limits all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. For more information, see [Setting Up VPC Endpoints for Systems Manager](sysman-setting-up-vpc.md)\.

## What Is a Session?<a name="what-is-a-session"></a>

A session is a connection made to an instance using Session Manager\. Sessions are based on a secure bi\-directional communication channel between the client \(you\) and the remote managed instance that streams inputs and outputs for commands\. Traffic between a client and a managed instance is encrypted using TLS 1\.2, and requests to create the connection are signed using Sigv4\. This two\-way communication enables interactive bash and PowerShell access to instances\.

For example, say that John is an on\-call engineer in your IT department\. He receives notification of an issue that requires him to remotely connect to an instance, such as a failure that requires troubleshooting or a directive to change a simple configuration option on an instance\. Using the AWS Systems Manager console or the AWS CLI, John starts a session connecting him to the instance, runs commands on the instance needed to complete the task, and then terminates the session\.

When John sends that first command to start the session, the Session Manager service authenticates his ID, verifies the permissions granted to him by an IAM policy, checks configuration settings \(such as verifying allowed limits for the sessions\), and sends a message to SSM Agent to open the two\-way connection\. After the connection is established and John types the next command, the command output from SSM Agent is uploaded to this communication channel and sent back to his local machine\.