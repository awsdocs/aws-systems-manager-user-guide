# Finding public parameters<a name="parameter-store-finding-public-parameters"></a>

You can search for public parameters using the Parameter Store console or the AWS Command Line Interface\. A public parameter name begins with `aws/service/list`\. The next part of the name corresponds to the service that owns that parameter\. 

Below is a list of some services which provide public parameters:
+ ami\-al\-latest 
+ ami\-amazon\-linux\-latest
+ ami\-windows\-latest
+ aws\-storage\-gateway\-latest
+ bottlerocket
+ canonical
+ datasync
+ debian
+ ec2windows
+ ecs
+ global\-infrastructure
+ redhat
+ storagegateway
+ suse

All public parameters aren't published to all AWS Regions\.

## Finding public parameters using the AWS CLI<a name="paramstore-discover-public-cli"></a>

Use `describe-parameters` for discovery of public parameters\. Use `get-parameters-by-path` to get the actual path for a service listed under `/aws/service/list`\. To get the service's path, remove `/list` from the path\. For example, `/aws/service/list/ecs` becomes `/aws/service/ecs`\.

To retrieve a list of public parameters owned by different services in Parameter Store, run the following command\.

```
aws ssm get-parameters-by-path --path /aws/service/list
```

The following example output has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/list/ami-al-latest",
            "Type": "String",
            "Value": "/aws/service/ami-al-latest/",
            "Version": 1,
            "LastModifiedDate": "2021-01-29T10:25:10.902000-08:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/list/ami-al-latest",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/list/ami-windows-latest",
            "Type": "String",
            "Value": "/aws/service/ami-windows-latest/",
            "Version": 1,
            "LastModifiedDate": "2021-01-29T10:25:12.567000-08:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/list/ami-windows-latest",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/list/aws-storage-gateway-latest",
            "Type": "String",
            "Value": "/aws/service/aws-storage-gateway-latest/",
            "Version": 1,
            "LastModifiedDate": "2021-01-29T10:25:09.903000-08:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/list/aws-storage-gateway-latest",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/list/global-infrastructure",
            "Type": "String",
            "Value": "/aws/service/global-infrastructure/",
            "Version": 1,
            "LastModifiedDate": "2021-01-29T10:25:11.901000-08:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/list/global-infrastructure",
            "DataType": "text"
        },
        …
}
```

If you want to view parameters owned by a specific service, choose the service from the list that was produced after running the earlier command\. Then, make a `describe-parameters` call and filter the parameters using the name of your desired service\. For example, `/aws/service/global-infrastructure`\. The path filter might be one\-level \(only calls parameters that match the exact values given\) or recursive \(contains elements in the path beyond what you have given\)\. The path filter is recursive by default\. Specify `Option:OneLevel` if you don't want it to be recursive\.

```
aws ssm describe-parameters --parameter-filters "Key=Path, Values=/aws/service/global-infrastructure"
```

 This returns all parameters owned by `global-infrastructure`\. 

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/current-region",
            "Type": "String",
            "LastModifiedDate": "2019-06-21T05:15:34.252000-07:00",
            "Version": 1,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/version",
            "Type": "String",
            "LastModifiedDate": "2019-02-04T06:59:32.875000-08:00",
            "Version": 1,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        }
    ]
}
```

You can also view parameters owned by a specific service by using the `Option:BeginsWith` filter\.

```
aws ssm describe-parameters --parameter-filters "Key=Name, Option=BeginsWith, Values=/aws/service/ami-amazon-linux-latest"
```

The command returns information like the following example\. This example output has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
            "Type": "String",
            "LastModifiedDate": "2021-01-26T13:39:40.686000-08:00",
            "Version": 25,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
            "Type": "String",
            "LastModifiedDate": "2021-01-26T13:39:40.807000-08:00",
            "Version": 25,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
            "Type": "String",
            "LastModifiedDate": "2021-01-26T13:39:40.920000-08:00",
            "Version": 25,
            "Tier": "Standard",
            "Policies": [],
            "DataType": "text"
        },
        …
}
```

**Note**  
The returned parameters might be different when you use `Option=BeginsWith` because it uses a different search pattern\.

## Finding public parameters using the Parameter Store console<a name="paramstore-discover-public-console"></a>

You must have at least one parameter in your AWS account and AWS Region before you can search for public parameters using the console\.

**To find public parameters using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the **Public parameters** tab\.

1. Choose the **Select a service** dropdown\. Choose the service whose parameters you want to use\.

1. Filter the parameters owned by the service you selected by entering more information into the search bar\.

1. Choose the public parameter you want to use\. 