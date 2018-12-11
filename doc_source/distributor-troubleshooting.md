# Troubleshooting AWS Systems Manager Distributor<a name="distributor-troubleshooting"></a>

The following information can help you troubleshoot problems that might occur when you use Distributor\.

**Topics**
+ [Wrong Package with the Same Name Is Installed](#distributor-tshoot-1)
+ [Error: Failed to Retrieve Manifest: Could not find latest version of package](#distributor-tshoot-2)

## Wrong Package with the Same Name Is Installed<a name="distributor-tshoot-1"></a>

**Problem:** You've installed a package, but AWS Systems Manager Distributor installed a different package instead\.

**Cause:** During installation, AWS Systems Manager finds AWS\-published packages as results before user\-defined external packages\. If your user\-defined package name is the same as an AWS\-published package name, the AWS package is installed instead of your package\.

**Solution:** To avoid this problem, name your package something different from the name for an AWS\-published package\.

## Error: Failed to Retrieve Manifest: Could not find latest version of package<a name="distributor-tshoot-2"></a>

**Problem:** You received an error like the following:

```
Failed to retrieve manifest: ResourceNotFoundException: Could not find the latest version of package arn:aws:ssm:::package/packagename
status code: 400, request id: guid
```

**Cause:** You are using a version of SSM Agent with Systems Manager Distributor that is earlier than version 2\.3\.274\.0\.

**Solution:** Update the version of SSM Agent to version 2\.3\.274\.0 or later\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample) or [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.