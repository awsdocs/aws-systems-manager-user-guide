# Troubleshooting Amazon EC2 managed instance availability using `ssm-cli`<a name="ssm-cli"></a>

For an Amazon EC2 instance to be managed by AWS Systems Manager and available in lists of managed instances, it must meet three primary requirements:
+ SSM Agent must be installed and running on an instance with a supported operating system\.

  Some AWS managed Amazon Machine Images \(AMIs\) are configured to launch instances with [SSM Agent](ssm-agent.md) preinstalled\. \(You can also configure a custom AMI to preinstall SSM Agent\.\) For more information, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.
+ An AWS Identity and Access Management \(IAM\) instance profile that supplies the required permissions for the instance to communicate with the Systems Manager service must be attached to the instance\.
+ SSM Agent must be able to connect to a Systems Manager endpoint to register itself with the service\. Thereafter, the instance must be available to the service, which is confirmed by the service sending a signal every five minutes to check the instance's health\.

Starting with SSM Agent version 3\.1\.501\.0, you can use `ssm-cli` to determine whether an instance meets these requirements\. The `ssm-cli` is a standalone command line tool included in the SSM Agent installation\. Preconfigured commands are included that gather the required information to help you diagnose why an Amazon EC2 instance that you have confirmed is running isn't included in your lists of managed instances in Systems Manager\. These commands are run when you specify the `get-diagnostics` option\.

Run the following command to use `ssm-cli` to help you troubleshoot Amazon EC2 managed instance availability\. On a Windows instance, you must navigate to the `C:\Program Files\Amazon\SSM` directory before running the command\.

------
#### [ Linux & macOS ]

```
ssm-cli get-diagnostics --output table
```

------
#### [ Windows ]

```
ssm-cli.exe get-diagnostics --output table
```

------
#### [ PowerShell ]

```
.\ssm-cli.exe get-diagnostics --output table
```

------

The command returns output similar to the following\. Connectivity checks to the `ssmmessages`, `s3`, `kms`, `logs`, and `monitoring` endpoints are for additional optional features such as Session Manager that can log to Amazon Simple Storage Service \(Amazon S3\) or Amazon CloudWatch Logs, and use AWS Key Management Service \(AWS KMS\) encryption\.

------
#### [ Linux & macOS ]

```
[root@instance]# ssm-cli get-diagnostics --output table
┌───────────────────────────────────────┬─────────┬───────────────────────────────────────────────────────────────────────┐
│ Check                                 │ Status  │ Note                                                                  │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ EC2 IMDS                              │ Success │ IMDS is accessible and has instance id i-0123456789abcdefa in Region  │
│                                       │         │ us-east-2                                                             │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Hybrid instance registration          │ Skipped │ Instance does not have hybrid registration                            │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to ssm endpoint          │ Success │ ssm.us-east-2.amazonaws.com is reachable                              │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to ec2messages endpoint  │ Success │ ec2messages.us-east-2.amazonaws.com is reachable                      │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to ssmmessages endpoint  │ Success │ ssmmessages.us-east-2.amazonaws.com is reachable                      │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to s3 endpoint           │ Success │ s3.us-east-2.amazonaws.com is reachable                               │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to kms endpoint          │ Success │ kms.us-east-2.amazonaws.com is reachable                              │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to logs endpoint         │ Success │ logs.us-east-2.amazonaws.com is reachable                             │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Connectivity to monitoring endpoint   │ Success │ monitoring.us-east-2.amazonaws.com is reachable                       │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ AWS Credentials                       │ Success │ Credentials are for                                                   │
│                                       │         │ arn:aws:sts::123456789012:assumed-role/Fullaccess/i-0123456789abcdefa │
│                                       │         │ and will expire at 2021-08-17 18:47:49 +0000 UTC                      │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Agent service                         │ Success │ Agent service is running and is running as expected user              │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ Proxy configuration                   │ Skipped │ No proxy configuration detected                                       │
├───────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│ SSM Agent version                     │ Success │ SSM Agent version is 3.0.1209.0, latest available agent version is    │
│                                       │         │ 3.1.192.0                                                             │
└───────────────────────────────────────┴─────────┴───────────────────────────────────────────────────────────────────────┘
```

------
#### [ Powershell & Windows ]

```
PS C:\Program Files\Amazon\SSM> ssm-cli.exe get-diagnostics --output table      
┌───────────────────────────────────────┬─────────┬─────────────────────────────────────────────────────────────────────┐
│ Check                                 │ Status  │ Note                                                                │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ EC2 IMDS                              │ Success │ IMDS is accessible and has instance id i-0123456789EXAMPLE in       │
│                                       │         │ Region us-east-2                                                    │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Hybrid instance registration          │ Skipped │ Instance does not have hybrid registration                          │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to ssm endpoint          │ Success │ ssm.us-east-2.amazonaws.com is reachable                            │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to ec2messages endpoint  │ Success │ ec2messages.us-east-2.amazonaws.com is reachable                    │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to ssmmessages endpoint  │ Success │ ssmmessages.us-east-2.amazonaws.com is reachable                    │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to s3 endpoint           │ Success │ s3.us-east-2.amazonaws.com is reachable                             │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to kms endpoint          │ Success │ kms.us-east-2.amazonaws.com is reachable                            │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to logs endpoint         │ Success │ logs.us-east-2.amazonaws.com is reachable                           │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Connectivity to monitoring endpoint   │ Success │ monitoring.us-east-2.amazonaws.com is reachable                     │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ AWS Credentials                       │ Success │ Credentials are for                                                 │
│                                       │         │  arn:aws:sts::123456789012:assumed-role/SSM-Role/i-123abc45EXAMPLE  │
│                                       │         │  and will expire at 2021-09-02 13:24:42 +0000 UTC                   │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Agent service                         │ Success │ Agent service is running and is running as expected user            │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Proxy configuration                   │ Skipped │ No proxy configuration detected                                     │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ Windows sysprep image state           │ Success │ Windows image state value is at desired value IMAGE_STATE_COMPLETE  │
├───────────────────────────────────────┼─────────┼─────────────────────────────────────────────────────────────────────┤
│ SSM Agent version                     │ Success │ SSM Agent version is 3.0.1209.0, latest agent version in us-east-2  │
│                                       │         │ is 3.1.192.0                                                        │
└───────────────────────────────────────┴─────────┴─────────────────────────────────────────────────────────────────────┘
```

------

The following table provides additional details for each of the checks performed by `ssm-cli`\.


**`ssm-cli` diagnostic checks**  

| Check | Details | 
| --- | --- | 
| Amazon EC2 instance metadata service | The instance is able to reach the instance metadata service\. A failed test indicates a connectivity issue to http://169\.254\.169\.254 which can be caused by local route, proxy, or operating system \(OS\) firewall and proxy configurations\. | 
| Hybrid instance registration | Whether the SSM Agent is registered using a hybrid activation\. | 
| Connectivity to ssm endpoint | The instance is able to reach the service endpoints for Systems Manager on TCP port 443\. A failed test indicates connectivity issues to https://ssm\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity issues can be caused by the VPC configuration including security groups, network access control lists, route tables, or OS firewalls and proxies\. | 
| Connectivity to ec2messages endpoint | The instance is able to reach the service endpoints for Systems Manager on TCP port 443\. A failed test indicates connectivity issues to https://ec2messages\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity issues can be caused by the VPC configuration including security groups, network access control lists, route tables, or OS firewalls and proxies\. | 
| Connectivity to ssmmessages endpoint | The instance is able to reach the service endpoints for Systems Manager on TCP port 443\. A failed test indicates connectivity issues to https://ssmmessages\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity issues can be caused by the VPC configuration including security groups, network access control lists, route tables, or OS firewalls and proxies\. | 
| Connectivity to s3 endpoint | The instance is able to reach the service endpoint for Amazon Simple Storage Service on TCP port 443\. A failed test indicates connectivity issues to https://s3\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity to this endpoint is not required for an instance to appear in your managed instances list\. | 
| Connectivity to kms endpoint |  The instance is able to reach the service endpoint for AWS Key Management Service on TCP port 443\. A failed test indicates connectivity issues to https://kms\.*region*\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity to this endpoint is not required for an instance to appear in your managed instances list\.  | 
| Connectivity to logs endpoint | The instance is able to reach the service endpoint for Amazon CloudWatch Logs on TCP port 443\. A failed test indicates connectivity issues to https://logs\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity to this endpoint is not required for an instance to appear in your managed instances list\. | 
| Connectivity to monitoring endpoint | The instance is able to reach the service endpoint for CloudWatch on TCP port 443\. A failed test indicates connectivity issues to https://monitoring\.region\.amazonaws\.com depending on the AWS Region where the instance is located\. Connectivity to this endpoint is not required for an instance to appear in your managed instances list\. | 
| AWS Credentials | Whether the SSM Agent has the required credentials based on the IAM instance profile attached to the instance\. A failed test indicates that no instance profile is attached to the instance, or it does not contain the required permissions for Systems Manager\. | 
| Agent service | Whether the SSM Agent service is running, and whether the service is running as root for Linux, or SYSTEM for Windows\. A failed test indicates the SSM Agent service is not running or is not running as root or SYSTEM\. | 
| Proxy configuration | Whether the SSM Agent is configured to use a proxy\. | 
| Sysprep image state \(Windows only\) | The state of Sysprep on the instance\. The SSM Agent will not start on the instance if the Sysprep state is a value other than IMAGE\_STATE\_COMPLETE\. | 
| SSM Agent version | Whether the latest available version of the SSM Agent is installed\. | 