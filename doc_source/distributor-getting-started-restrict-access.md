# Step 3: Control user access to packages<a name="distributor-getting-started-restrict-access"></a>

Using IAM policies, you can control who can create, deploy, and manage packages\. You also control which Run Command and State Manager API actions they can perform on managed instances\.

**ARN Format**  
User\-defined packages are associated with document ARNs and have the following format:

```
arn:aws:ssm:region-id:account-id:document/document-name
```

The following is an example\.

```
arn:aws:ssm:us-west-1:123456789012:document/ExampleDocumentName
```

You can use a pair of AWS\-supplied default IAM policies, one for end users and one for administrators, to grant permissions for Distributor activities\. Or you can create custom IAM policies appropriate for your permissions requirements\.

For more information about using variables in IAM policies, see [IAM Policy Elements: Variables](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)\. 

For information about how to create policies and attach them to IAM users or groups, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) and [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.