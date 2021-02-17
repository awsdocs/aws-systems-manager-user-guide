# Auditing and logging Distributor activity<a name="distributor-logging-auditing"></a>

For more information about auditing and logging options for AWS Systems Manager, see [Monitoring AWS Systems Manager](monitoring.md)\.

## Audit Distributor activity using AWS CloudTrail<a name="distributor-logging-auditing-cloudtrail"></a>

AWS CloudTrail captures API calls made in the Systems Manager console, the AWS CLI, and the Systems Manager SDK\. The information can be viewed in the CloudTrail console or stored in an S3 bucket\. One bucket is used for all CloudTrail logs for your account\.

Logs of Run Command and State Manager actions show document creation, package installation, and package uninstallation activity\. For more information about viewing and using CloudTrail logs of Systems Manager activity, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.