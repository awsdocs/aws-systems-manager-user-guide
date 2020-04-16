# Step 3: Control user session access to instances<a name="session-manager-getting-started-restrict-access"></a>

Session Manager allows you to centrally grant and revoke user access to instances\. Using IAM policies, you control which instances specific users or groups can connect to, and you control what Session Manager API actions they can perform on the instances they are given access to\. 

**About Session ID ARN Formats**  
IAM policies for Session Manager access use variables for user names as part of session IDs\. Session IDs in turn are used in session Amazon Resource Names \(ARNs\) to control access\. Session ARNs have the following format:

```
arn:aws:ssm:region-id:account-id:session/session-id
```

For example:

```
arn:aws:ssm:us-east-2:123456789012:session/JohnDoe-1a2b3c4d5eEXAMPLE
```

You can use a pair of default IAM policies supplied by AWS, one for end users and one for administrators, to supply permissions for Session Manager activities\. Or you can create custom IAM policies for different permissions requirements you might have\.

For more information about using variables in IAM policies, see [IAM Policy Elements: Variables](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)\. 

For information about how to create policies and attach them to IAM users or groups, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) and [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

**Topics**
+ [Enforce a session document permission check for the AWS CLI](getting-started-sessiondocumentaccesscheck.md)
+ [Quickstart default IAM policies for Session Manager](getting-started-restrict-access-quickstart.md)
+ [Additional sample IAM policies for Session Manager](getting-started-restrict-access-examples.md)