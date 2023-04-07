# How Patch Manager operations work<a name="patch-manager-patching-operations"></a>

This section provides technical details that explain how Patch Manager, a capability of AWS Systems Manager, determines which patches to install and how it installs them on each supported operating system\. For Linux operating systems, it also provides information about specifying a source repository, in a custom patch baseline, for patches other than the default configured on a managed node\. This section also provides details about how patch baseline rules work on different distributions of the Linux operating system\.

**Note**  
The information in the following topics applies no matter which method or type of configuration you are using for your patching operations:  
A patch policy configured in Quick Setup
A Host Management option configured in Quick Setup
A maintenance window to run a patch `Scan` or `Install` task
An on\-demand **Patch now** operation

**Topics**
+ [How package release dates and update dates are calculated](patch-manager-release-dates.md)
+ [How security patches are selected](patch-manager-selecting-patches.md)
+ [How to specify an alternative patch source repository \(Linux\)](patch-manager-alternative-source-repository.md)
+ [How patches are installed](patch-manager-installing-patches.md)
+ [How patch baseline rules work on Linux\-based systems](patch-manager-linux-rules.md)
+ [Key differences between Linux and Windows patching](patch-manager-windows-and-linux-differences.md)