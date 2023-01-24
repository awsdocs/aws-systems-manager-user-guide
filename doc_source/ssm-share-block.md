# Block public sharing for SSM documents<a name="ssm-share-block"></a>

Unless your use case requires public sharing to be turned on, we recommend turning on the block public sharing setting for your AWS Systems Manager \(SSM\) documents\. Turning on this setting prevents unwanted access to your SSM documents\. The block public sharing setting is an account level setting that can differ for each AWS Region\. Complete the following tasks to block public sharing for your SSM documents\.

## Block public sharing \(console\)<a name="share-block-console"></a>

**To block public sharing of your SSM documents**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Preferences**, and then choose **Edit** in the **Block public sharing** section\.

1. Select the **Block public sharing** check box, and then choose **Save**\. 

## Block public sharing \(command line\)<a name="share-block-cli"></a>

Open the AWS Command Line Interface \(AWS CLI\) or AWS Tools for Windows PowerShell on your local computer and run the following command to block public sharing of your SSM documents\.

------
#### [ Linux & macOS ]

```
aws ssm update-service-setting  \
    --setting-id /ssm/documents/console/public-sharing-permission \
    --setting-value Disable \
    --region 'The AWS Region you want to block public sharing in'
```

------
#### [ Windows ]

```
aws ssm update-service-setting ^
    --setting-id /ssm/documents/console/public-sharing-permission ^
    --setting-value Disable ^
    --region "The AWS Region you want to block public sharing in"
```

------
#### [ PowerShell ]

```
Update-SSMServiceSetting `
    -SettingId /ssm/documents/console/public-sharing-permission `
    -SettingValue Disable `
    –Region The AWS Region you want to block public sharing in
```

------

Confirm the setting value was updated using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-service-setting   \
    --setting-id /ssm/documents/console/public-sharing-permission \
    --region The AWS Region you blocked public sharing in
```

------
#### [ Windows ]

```
aws ssm get-service-setting  ^
    --setting-id /ssm/documents/console/public-sharing-permission ^
    --region "The AWS Region you blocked public sharing in"
```

------
#### [ PowerShell ]

```
Get-SSMServiceSetting `
    -SettingId /ssm/documents/console/public-sharing-permission `
    -Region The AWS Region you blocked public sharing in
```

------

## Restricting access to block public sharing with IAM<a name="share-block-iam"></a>

You can create AWS Identity and Access Management \(IAM\) policies that restrict users from modifying the block public sharing setting\. This prevents users from allowing unwanted access to your SSM documents\. 

The following is an example of an IAM policy that prevents users from updating the block public sharing setting\. To use this example, you must replace the example Amazon Web Services account ID with your own account ID\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "ssm:UpdateServiceSetting",
            "Resource": "arn:aws:ssm:*:987654321098:servicesetting/ssm/documents/console/public-sharing-permission"
        }
    ]
}
```