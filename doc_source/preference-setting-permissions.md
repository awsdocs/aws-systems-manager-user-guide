# Grant or deny a user permissions to update Session Manager preferences<a name="preference-setting-permissions"></a>

Account preferences are stored as SSM documents for each AWS Region\. Before a user can update account preferences for sessions in your account, they must be granted the necessary permissions to access the type of SSM document where these preferences are stored\. These permissions are granted through an IAM policy\.

**Administrator policy to allow preferences to be created and updated**  
An administrator can have the following policy to create and update preferences at any time\. The following policy allows permission to access and update the `SSM-SessionManagerRunShell` document in the us\-east\-2 account 123456789012\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:CreateDocument",
                "ssm:UpdateDocument",
                "ssm:DeleteDocument"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:us-east-2:123456789012:document/SSM-SessionManagerRunShell"
            ]
        }
    ]
}
```

**User policy to prevent preferences from being updated**  
Use the following policy to prevent end users in your account from updating or overriding any Session Manager preferences\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:CreateDocument",
                "ssm:UpdateDocument",
                "ssm:DeleteDocument"
            ],
            "Effect": "Deny",
            "Resource": [
                "arn:aws:ssm:us-east-2:123456789012:document/SSM-SessionManagerRunShell"
            ]
        }
    ]
}
```