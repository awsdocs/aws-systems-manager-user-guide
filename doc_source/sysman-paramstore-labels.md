# Working with parameter labels<a name="sysman-paramstore-labels"></a>

A parameter label is a user\-defined alias to help you manage different versions of a parameter\. When you modify a parameter, Systems Manager automatically saves a new version and increments the version number by one\. A label can help you remember the purpose of a parameter version when there are multiple versions\.

For example, let's say you have a parameter called /MyApp/DB/ConnectionString\. The value of the parameter is a connection string to a MySQL server in a local database in a test environment\. After you finish updating the application, you want the parameter to use a connection string for a production database\. You change the value of /MyApp/DB/ConnectionString\. Systems Manager automatically creates version two with the new connection string\. To help you remember the purpose of each version, you attach a label to each parameter\. For version one, you attach the label *Test* and for version two you attach the label *Production*\.

You can move labels from one version of a parameter to another version\. For example, if you create version three of the /MyApp/DB/ConnectionString parameter with a connection string for a new production database, then you can move the *Production* label from parameter two to parameter three\. 

Parameter labels are a lightweight alternative to parameter tags\. Your organization might have strict guidelines for tags that must be applied to different AWS resources\. In contrast, a label is simply a text association for a specific version of a parameter\. 

Similar to tags, you can query parameters by using labels\. You can view a list of specific parameter versions that all use the same label if you query your parameter set by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action, as described later in this section\.

**Label Requirements and Restrictions**

Parameter labels have the following requirements and restrictions:
+ A version of a parameter can have a maximum of 10 labels\.
+ You can't attach the same label to different versions of the same parameter\. For example, if version 1 has the label *Production*, then you can't attach *Production* to version 2\.
+ You can move a label from one version of a parameter to another\.
+ You can't create a label when you create a new parameter\. You must attach a label to a specific version of a parameter\.
+ You can't delete a parameter label\. If you no longer want to use a parameter label, then you must move it to a different version of a parameter\.
+ A label can have a maximum of 100 characters\.
+ Labels can contain letters \(case sensitive\), numbers, periods \(\.\), hyphens \(\-\), or underscores \(\_\)\. 
+ Labels can't begin with a number, "aws," or "ssm" \(not case sensitive\)\. If a label fails to meet these requirements, then the label is not attached to the parameter version and the system displays it in the list of `InvalidLabels`\.

**Topics**
+ [Working with parameter labels \(console\)](sysman-paramstore-labels-console.md)
+ [Working with parameter labels \(AWS CLI\)](sysman-paramstore-labels-cli.md)