# Organize Instances into Patch Groups<a name="sysman-patch-patchgroups"></a>

A *patch group* is an optional means of organizing instances for patching\. For example, you can create patch groups for different operating systems \(Linux or Windows\), different environments \(Development, Test, and Production\), or different server functions \(web servers, file servers, databases\)\. Patch groups can help you avoid deploying patches to the wrong set of instances\. They can also help you avoid deploying patches before they have been adequately tested\.

You create a patch group by using Amazon EC2 tags\. Unlike other tagging scenarios across Systems Manager, a patch group *must* be defined with the tag key: **Patch Group**\. Note that the key is case sensitive\. You can specify any value, for example "web servers," but the key must be **Patch Group**\.

**Note**  
An instance can only be in one patch group\.

After you create a patch group and tag instances, you can register the patch group with a patch baseline\. By registering the patch group with a patch baseline, you ensure that the correct patches are installed during the patching execution\.

When the system runs the task to apply a patch baseline to an instance, the service checks to see if a patch group is defined for the instance\. If the instance is assigned to a patch group, the system then checks to see which patch baseline is registered to that group\. If a patch baseline is found for that group, the system applies the patch baseline\. If an instance isn't configured for a patch group, the system automatically uses the currently configured default patch baseline\.

For example, let's say an instance is tagged with `key=Patch Group` and `value=Front-End Servers`\. When Patch Manager runs the **AWS\-RunPatchBaseline** task on that instance, the service checks to see which patch baseline is registered with Front\-End Servers\. If a patch baseline is found, the system uses that baseline\. If no patch baseline is registered for Front\-End Servers, the system uses the default patch baseline\. 

To view an example of creating a patch baseline and patch groups by using the AWS CLI, see [Walkthrough: Patch a Server Environment \(AWS CLI\)](sysman-patch-cliwalk.md)\. For more information about Amazon EC2 tags, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide*\.