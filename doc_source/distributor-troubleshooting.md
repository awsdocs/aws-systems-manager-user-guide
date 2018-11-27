# Troubleshooting AWS Systems Manager Distributor<a name="distributor-troubleshooting"></a>

The following information can help you troubleshoot problems that might occur when you use Distributor\.

**Topics**
+ [Wrong Package with the Same Name Is Installed](#distributor-tshoot-1)

## Wrong Package with the Same Name Is Installed<a name="distributor-tshoot-1"></a>

**Problem:** You've installed a package, but AWS Systems Manager Distributor installed a different package instead\.

**Cause:** During installation, AWS Systems Manager finds AWS\-published packages as results before user\-defined external packages\. If your user\-defined package name is the same as an AWS\-published package name, the AWS package is installed instead of your package\.

**Solution:** To avoid this problem, name your package something different from the name for an AWS\-published package\.