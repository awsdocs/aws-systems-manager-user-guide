# About the Configuration Profile IAM Role<a name="appconfig-creating-configuration-and-profile-iam-role"></a>

You can create the IAM role that provides access to the configuration data by using AppConfig, as described in the following procedure\. Or you can create the IAM role yourself and choose it from a list\. If you create the role by using AppConfig, the system creates the role and specifies one of the following permissions policies, depending on which type of configuration source you choose\.

**Configuration source is an SSM document**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetDocument"
            ],
            "Resource": [
                "arn:aws:ssm:AWS-Region:account-number:document/document-name"
            ]
        }
    ]
}
```

**Configuration source is a Parameter Store parameter**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter"
            ],
            "Resource": [
                "Arn:aws:ssm:AWS-Region:account-number:parameter/parameter-name"
            ]
        }
    ]
    }
```

If you create the role by using AppConfig, the system also creates the following trust relationship for the role\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "appconfig.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```