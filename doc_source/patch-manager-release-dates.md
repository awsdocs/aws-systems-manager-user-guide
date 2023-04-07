# How package release dates and update dates are calculated<a name="patch-manager-release-dates"></a>

**Important**  
The information on this page applies to the Amazon Linux, Amazon Linux 2, and operating systems \(OSs\) for Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. The packages for these OS types are created and maintained by Amazon Web Services\. How the manufacturers of other operating systems manage their packages and repositories affect how their release dates and update dates are calculated\. For OSs besides Amazon Linux, Amazon Linux 2, Amazon Linux 2022, and Amazon Linux 2023, such as Red Hat Enterprise Linux \(RHEL\) and SUSE Linux Enterprise Server \(SLES\), refer to the manufacturer's documentation for information about how their packages are updated and maintained\.

In the settings for [custom patch baselines](patch-manager-predefined-and-custom-patch-baselines.md#patch-manager-baselines-custom) you create, for most OS types, you can specify that patches are auto\-approved for installation after a certain number of days\. AWS provides several predefined patch baselines that include auto\-approval dates of 7 days\.

An *auto\-approval delay* is the number of days to wait after the patch was released, before the patch is automatically approved for patching\. For example, you create a rule using the `CriticalUpdates` classification and configure it for 7 days auto\-approval delay\. As a result, a new critical patch with a release date or last update date of July 7 is automatically approved on July 14\.

To avoid unexpected results with auto\-approval delays on Amazon Linux, Amazon Linux 2, Amazon Linux 2022, and Amazon Linux 2023, it's important to understand how their release dates and update dates are calculated\.

In most cases, the auto\-approval wait time before patches are installed is calculated from an `Updated Date` value in `updateinfo.xml`, not a `Release Date` value\. The following are important details about these date calculations: 
+ The `Release Date` is the date a *notice* is released\. It does not mean the package is necessarily available in the associated repositories yet\. 
+ The `Update Date` is the last date the notice was updated\. An update to a notice can represent something as small as a text or description update\. It does not mean the package was released from that date or is necessarily available in the associated repositories yet\. 

  This means that a package could have an `Update Date` value of July 7 but not be available for installation until \(for example\) July 13\. Suppose for this case that a patch baseline that specifies a 7\-day auto\-approval delay runs in an `Install` operation on July 14\. Because the `Update Date` value is 7 days prior to the run date, the patches and updates in the package are installed on July 14\. The installation happens even though only 1 day has passed since the package became available for actual installation\.
+ A package containing operating system or application patches can be updated more than once after initial release\.
+ A package can be released into the AWS managed repositories but then rolled back if issues are later discovered with it\.

In some patching operations, these factors might not be important\. For example, if a patch baseline is configured to install a patch with severity values of `Low` and `Medium`, and a classification of `Recommended`, any auto\-approval delay might have little impact on your operations\.

However, in cases where the timing of critical or high\-severity patches is more important, you might want to exercise more control over when patches are installed\. The recommended method for doing this is to use alternative patch source repositories instead of the default repositories for patching operations on a managed node\. 

You can specify alternative patch source repositories when you create a custom patch baseline\. In each custom patch baseline, you can specify patch source configurations for up to 20 versions of a supported Linux operating system\. For more information, see [How to specify an alternative patch source repository \(Linux\)](patch-manager-alternative-source-repository.md)\.