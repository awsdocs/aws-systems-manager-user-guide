# Step 5: Create a Configuration Profile<a name="appconfig-creating-configuration-profile"></a>

A configuration profile enables AWS AppConfig to access the source of your configuration data\. Valid configuration sources include Systems Manager \(SSM\) documents and Parameter Store parameters\. A configuration profile includes the following information\.
+ The URI location of the configuration data\. 
+ The AWS Identity and Access Management \(IAM\) role that provides access to the configuration data\.
+ \(Optional\) A validator for the configuration data\. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile\. A configuration profile can have a maximum of two validators\.

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

Use the following procedure to create an AppConfig configuration profile by using the AWS Systems Manager console\.

**To create a configuration profile**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane choose **AppConfig**\.

1. On the **Applications** tab, choose the application you created in [Step 3: Create an AppConfig Application](appconfig-creating-application.md) and then choose **View details**\.

1. Choose the **Configurations** tab, and then choose **Create configuration profile**\.

1. For **Name**, enter a name for the configuration profile\.

1. For **Description**, enter information about the configuration profile\.

1. In the **Service role** section, choose **New service role** to have AppConfig create the IAM role that provides access to the configuration data\. AppConfig automatically populates the **Role name** field based on the name you entered earlier\. Or, to choose a role that already exists in IAM, choose **Existing service role**\. Choose the role by using the **Role ARN** list\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. On the **Select configuration source** page, choose either **AWS Systems Manager document** or **AWS Systems Manager parameter**\. Choose the document or the parameter from the list, and then choose **Next**\.

1. On the **Add validators** page, choose either **JSON Schema** or **AWS Lambda**\. If you choose **JSON Schema**, enter the JSON Schema in the field\. If you choose **AWS Lambda**, choose the function and the version from the list\. 

1. Choose **Next**\.

AppConfig creates the configuration profile\. Proceed to [Step 6: Create a Deployment Strategy](appconfig-creating-deployment-strategy.md)\.