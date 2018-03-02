# Schedule Patch Updates Using a Maintenance Window<a name="sysman-patch-scheduletasks"></a>

After you configure a patch baseline \(and optionally a patch group\), you can apply patches to your instance by using a Maintenance Window\. A Maintenance Window can reduce the impact on server availability by letting you specify a time to perform the patching process that doesn't interrupt business operations\. A Maintenance Window works like this:

1. Create a Maintenance Window with a schedule for your patching operations\.

1. Choose the targets for the Maintenance Window by specifying the **Patch Group** tag for the tag name, and any value for which you have defined Amazon EC2 tags, for example, "production servers"\.

1. Create a new Maintenance Window task, and specify the **AWS\-RunPatchBaseline** document\. 

When you configure the task, you can choose to either scan instances or scan and install patches on the instances\. If you choose to scan instances, Patch Manager scans each instance and generates a list of missing patches for you to review\.

If you choose to scan and install patches, Patch Manager scans each instance and compares the list of installed patches against the list of approved patches in the baseline\. Patch Manager identifies missing patches, and then downloads and installs all missing and approved patches\.

If you want to perform a one\-time scan or install to fix an issue, you can use Run Command to call the **AWS\-RunPatchBaseline** document directly\.

**Important**  
After installing patches, Systems Manager reboots each instance\. The reboot is required to make sure that patches are installed correctly and to ensure that the system did not leave the instance in a potentially bad state\.