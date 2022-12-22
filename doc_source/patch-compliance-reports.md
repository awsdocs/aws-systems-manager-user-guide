# Working with patch compliance reports<a name="patch-compliance-reports"></a>

Use the information in the following topics to help you generate and work with patch compliance reports in Patch Manager, a capability of AWS Systems Manager\.

The information in the following topics apply no matter which method or type of configuration you're using for your patching operations: 
+ A patch policy configured in Quick Setup
+ A Host Management option configured in Quick Setup
+ A maintenance window to run a patch `Scan` or `Install` task
+ An on\-demand **Patch now** operation

**Important**  
If you have multiple types of operations in place to scan your instances for patch compliance, note that each scan overwrites the patch compliance data of previous scans\. As a result, you may end up with unexpected results in your patch compliance data\. For more information, see [Avoiding unintentional patch compliance data overwrites](avoid-patch-compliance-overwrites.md)\.  
To verify which patch baseline was used to generate the latest compliance information, browse to the **Compliance reporting** tab in Patch Manager, locate the row for the managed node you want information about, and then choose the baseline ID in the **Baseline ID used** column\.

**Topics**
+ [Viewing patch compliance results \(console\)](viewing-patch-compliance-results.md)
+ [Generating \.csv patch compliance reports \(console\)](patch-compliance-reports-to-s3.md)
+ [Remediating out\-of\-compliance managed nodes with Patch Manager](patch-compliance-remediation.md)
+ [Avoiding unintentional patch compliance data overwrites](avoid-patch-compliance-overwrites.md)