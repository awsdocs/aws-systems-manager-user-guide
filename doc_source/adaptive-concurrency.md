# Allowing Automation to adapt to your concurrency needs<a name="adaptive-concurrency"></a>

By default, Automation allows you to run up to 100 concurrent automations at a time\. Automation also provides an optional setting that you can use to adjust your concurrency automation quota automatically\. With this setting, your concurrency automation quota can accommodate up to 500 concurrent automations, depending on available resources\. 

**Note**  
If your automation calls API operations, adaptively scaling to your targets can result in throttling exceptions\. If recurring throttling exceptions occur when running automations with adaptive concurrency turned on, you might have to request quota increases for the API operation if available\.

**To turn on adaptive concurrency \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable adaptive concurrency**\.

1. Choose **Save**\.