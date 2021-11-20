# Calling public parameters for AWS services, Regions, endpoints, Availability Zones, local zones, and Wavelength Zones<a name="parameter-store-public-parameters-global-infrastructure"></a>

You can call the AWS Region, service, endpoint, Availability, and Wavelength Zones of public parameters by using the following path\.

`/aws/service/global-infrastructure`

**View active AWS Regions**  
You can view a list of all active AWS Regions by using the following command in the AWS Command Line Interface \(AWS CLI\)\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/regions \
    --query 'Parameters[].Name'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/regions ^
    --query Parameters[].Name
```

------

The command returns information like the following\.

```
[
    "/aws/service/global-infrastructure/regions/af-south-1",
    "/aws/service/global-infrastructure/regions/ap-east-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-3",
    "/aws/service/global-infrastructure/regions/ap-southeast-1",
    "/aws/service/global-infrastructure/regions/ca-central-1",
    "/aws/service/global-infrastructure/regions/cn-north-1",
    "/aws/service/global-infrastructure/regions/eu-west-2",
    "/aws/service/global-infrastructure/regions/eu-west-3",
    "/aws/service/global-infrastructure/regions/us-east-1",
    "/aws/service/global-infrastructure/regions/us-gov-west-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-2",
    "/aws/service/global-infrastructure/regions/ap-south-1",
    "/aws/service/global-infrastructure/regions/ap-southeast-2",
    "/aws/service/global-infrastructure/regions/cn-northwest-1",
    "/aws/service/global-infrastructure/regions/eu-south-1",
    "/aws/service/global-infrastructure/regions/me-south-1",
    "/aws/service/global-infrastructure/regions/sa-east-1",
    "/aws/service/global-infrastructure/regions/us-east-2",
    "/aws/service/global-infrastructure/regions/us-gov-east-1",
    "/aws/service/global-infrastructure/regions/us-west-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-1",
    "/aws/service/global-infrastructure/regions/eu-central-1",
    "/aws/service/global-infrastructure/regions/eu-north-1",
    "/aws/service/global-infrastructure/regions/eu-west-1",
    "/aws/service/global-infrastructure/regions/us-west-2"
]
```

**View available AWS services**  
You can view a complete list of all available AWS services and sort them into alphabetical order by using the following command\. This example output has been truncated for space\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/services ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\.

```
[
    "/aws/service/global-infrastructure/services/acm-pca",
    "/aws/service/global-infrastructure/services/acm",
    "/aws/service/global-infrastructure/services/alexaforbusiness",
    "/aws/service/global-infrastructure/services/apigateway",
    "/aws/service/global-infrastructure/services/application-autoscaling",
    "/aws/service/global-infrastructure/services/appmesh",
    "/aws/service/global-infrastructure/services/appstream",
    "/aws/service/global-infrastructure/services/appsync",
    "/aws/service/global-infrastructure/services/athena",
    "/aws/service/global-infrastructure/services/autoscaling-plans",
    "/aws/service/global-infrastructure/services/autoscaling",
    "/aws/service/global-infrastructure/services/backup",
    "/aws/service/global-infrastructure/services/batch",
    "/aws/service/global-infrastructure/services/budgets",
    "/aws/service/global-infrastructure/services/ce",
    "/aws/service/global-infrastructure/services/chime",
    "/aws/service/global-infrastructure/services/cloud9",
    "/aws/service/global-infrastructure/services/cloudformation",
    "/aws/service/global-infrastructure/services/cloudfront",
    "/aws/service/global-infrastructure/services/cloudhsm",
    "/aws/service/global-infrastructure/services/cloudhsmv2",
    "/aws/service/global-infrastructure/services/cloudsearch",
    "/aws/service/global-infrastructure/services/cloudtrail",
    "/aws/service/global-infrastructure/services/cloudwatch",
    "/aws/service/global-infrastructure/services/codebuild",
```

**View supported Regions for an AWS service**  
You can view a list of AWS Regions where a service is available\. This example uses AWS Systems Manager \(ssm\)\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services/ssm/regions \
    --query 'Parameters[].Value'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/services/ssm/regions ^
    --query Parameters[].Value
```

------

The command returns information like the following\.

```
[
    "ap-northeast-3",
    "ap-south-1",
    "cn-north-1",
    "eu-central-1",
    "eu-west-1",
    "eu-west-2",
    "eu-west-3",
    "me-south-1",
    "us-east-2",
    "us-gov-west-1",
    "af-south-1",
    "ap-east-1",
    "ap-northeast-1",
    "ap-northeast-2",
    "ap-southeast-1",
    "ca-central-1",
    "eu-north-1",
    "eu-south-1",
    "us-gov-east-1",
    "us-west-1",
    "ap-southeast-2",
    "cn-northwest-1",
    "sa-east-1",
    "us-east-1",
    "us-west-2"
]
```

**View the Regional endpoint for a service**  
You can view a Regional endpoint for a service by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameter \
    --name /aws/service/global-infrastructure/regions/us-east-2/services/ssm/endpoint \
    --query 'Parameter.Value'
```

------
#### [ Windows ]

```
aws ssm get-parameter ^
    --name /aws/service/global-infrastructure/regions/us-east-2/services/ssm/endpoint ^
    --query Parameter.Value
```

------

The command returns information like the following\.

```
"ssm.us-east-2.amazonaws.com"
```

**View complete Availability Zone details**  
You can view Availability Zones by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones/
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones/
```

------

The command returns information like the following\. This example has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/apne3-az3",
            "Type": "String",
            "Value": "apne3-az3",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:29:28.995000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/apne3-az3"
        },
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/aps1-az2",
            "Type": "String",
            "Value": "aps1-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:21:02.690000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/aps1-az2"
        },
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/cnn1-az2",
            "Type": "String",
            "Value": "cnn1-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:19:57.254000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/cnn1-az2"
        },
```

**View Availability Zone names only**  
You can view the names of Availability Zones only by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. This example has been truncated for space\.

```
[
    "/aws/service/global-infrastructure/availability-zones/afs1-az1",
    "/aws/service/global-infrastructure/availability-zones/afs1-az2",
    "/aws/service/global-infrastructure/availability-zones/afs1-az3",
    "/aws/service/global-infrastructure/availability-zones/ape1-az1",
    "/aws/service/global-infrastructure/availability-zones/ape1-az2",
    "/aws/service/global-infrastructure/availability-zones/ape1-az3",
    "/aws/service/global-infrastructure/availability-zones/apne1-az1",
    "/aws/service/global-infrastructure/availability-zones/apne1-az2",
    "/aws/service/global-infrastructure/availability-zones/apne1-az3",
    "/aws/service/global-infrastructure/availability-zones/apne1-az4",
```

**View names of Availability Zones in a single Region**  
You can view the names of the Availability Zones in one Region \(`us-east-1`, in this example\) using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/regions/us-east-1/availability-zones \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/regions/us-east-1/availability-zones ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. This example has been truncated for space\.

```
[
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az1",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az2",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az3",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az4",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az5",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az6"
```

**View Availability Zone ARNs only**  
You can view the Amazon Resource Names \(ARNs\) of Availability Zones only by using the following command\. 

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones \
    --query 'Parameters[].ARN | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones ^
    --query "Parameters[].ARN | sort(@)"
```

------

The command returns information like the following\. This example has been truncated for space\.

```
[
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az1",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az2",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az3",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az1",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az2",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az3",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/apne1-az1",
```

**View local zone details**  
You can view local zones by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/local-zones
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/local-zones
```

------

The command returns information like the following\. This example has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/local-zones/use1-bos1-az1",
            "Type": "String",
            "Value": "use1-bos1-az1",
            "Version": 3,
            "LastModifiedDate": "2020-12-15T14:16:17.502000-08:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/use1-bos1-az1",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/use1-chi1-az1",
            "Type": "String",
            "Value": "use1-chi1-az1",
            "Version": 1,
            "LastModifiedDate": "2021-09-08T10:05:38.634000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/use1-chi1-az1",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/use1-dfw1-az1",
            "Type": "String",
            "Value": "use1-dfw1-az1",
            "Version": 1,
            "LastModifiedDate": "2021-07-07T05:28:16.412000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/use1-dfw1-az1",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/use1-mci1-az1",
            "Type": "String",
            "Value": "use1-mci1-az1",
            "Version": 1,
            "LastModifiedDate": "2021-09-08T10:05:37.571000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/use1-mci1-az1",
            "DataType": "text"
        }
    ]
}
```

**View Wavelength Zone details**  
You can view Wavelength Zones by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/wavelength-zones
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/wavelength-zones
```

------

The command returns information like the following\. This example has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/wavelength-zones/apne1-wl1-nrt-wlz1",
            "Type": "String",
            "Value": "apne1-wl1-nrt-wlz1",
            "Version": 3,
            "LastModifiedDate": "2020-12-15T14:16:04.715000-08:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/wavelength-zones/apne1-wl1-nrt-wlz1",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/wavelength-zones/use1-wl1-atl-wlz1",
            "Type": "String",
            "Value": "use1-wl1-atl-wlz1",
            "Version": 3,
            "LastModifiedDate": "2020-09-22T13:10:11.783000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/wavelength-zones/use1-wl1-atl-wlz1",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/wavelength-zones/use1-wl1-bos-wlz1",
            "Type": "String",
            "Value": "use1-wl1-bos-wlz1",
            "Version": 3,
            "LastModifiedDate": "2020-08-06T07:11:23.225000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/wavelength-zones/use1-wl1-bos-wlz1",
            "DataType": "text"
        }
    ]
}
```

**View all parameters and values under a local zone**  
You can view all parameter data for a local zone by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/"
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/"
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry",
            "Type": "String",
            "Value": "US",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.405000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion",
            "Type": "String",
            "Value": "US-CA",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.466000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/location",
            "Type": "String",
            "Value": "US West (Los Angeles)",
            "Version": 1,
            "LastModifiedDate": "2021-01-12T11:45:21.151000-08:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/location",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group",
            "Type": "String",
            "Value": "us-west-2-lax-1",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.597000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone",
            "Type": "String",
            "Value": "usw2-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.783000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region",
            "Type": "String",
            "Value": "us-west-2",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.704000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group",
            "Type": "String",
            "Value": "us-west-2-lax-1",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:36:58.534000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group",
            "DataType": "text"
        }
    ]
}
```

**View local zone parameter names only**  
You can view just the names of local zone parameters by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/local-zones/usw2-lax1-az1 \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/local-zones/usw2-lax1-az1 ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. 

```
[
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group"
]
```