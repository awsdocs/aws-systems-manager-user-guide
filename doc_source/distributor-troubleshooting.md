# Troubleshooting AWS Systems Manager Distributor<a name="distributor-troubleshooting"></a>

The following information can help you troubleshoot problems that might occur when you use Distributor\.

**Topics**
+ [Wrong package with the same name is installed](#distributor-tshoot-1)
+ [Error: Failed to retrieve manifest: Could not find latest version of package](#distributor-tshoot-2)
+ [Error: Failed to retrieve manifest: Validation exception](#distributor-tshoot-3)

## Wrong package with the same name is installed<a name="distributor-tshoot-1"></a>

**Problem:** You've installed a package, but AWS Systems Manager Distributor installed a different package instead\.

**Cause:** During installation, AWS Systems Manager finds AWS\-published packages as results before user\-defined external packages\. If your user\-defined package name is the same as an AWS\-published package name, the AWS package is installed instead of your package\.

**Solution:** To avoid this problem, name your package something different from the name for an AWS\-published package\.

## Error: Failed to retrieve manifest: Could not find latest version of package<a name="distributor-tshoot-2"></a>

**Problem:** You received an error like the following:

```
Failed to retrieve manifest: ResourceNotFoundException: Could not find the latest version of package 
arn:aws:ssm:::package/package-name status code: 400, request id: guid
```

**Cause:** You are using a version of SSM Agent with Systems Manager Distributor that is earlier than version 2\.3\.274\.0\.

**Solution:** Update the version of SSM Agent to version 2\.3\.274\.0 or later\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample) or [Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.

## Error: Failed to retrieve manifest: Validation exception<a name="distributor-tshoot-3"></a>

**Problem:** You received an error like the following:

```
Failed to retrieve manifest: ValidationException: 1 validation error detected: Value 'documentArn'
at 'packageName' failed to satisfy constraint: Member must satisfy regular expression pattern:
arn:aws:ssm:region-id:account-id:package/package-name
```

**Cause:** You are using a version of SSM Agent with Systems Manager Distributor that is earlier than version 2\.3\.274\.0\.

**Solution:** Update the version of SSM Agent to version 2\.3\.274\.0 or later\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample) or [Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.