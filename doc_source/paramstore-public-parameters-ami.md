# Calling AMI public parameters<a name="paramstore-public-parameters-ami"></a>

Amazon EC2 AMI public parameters are available for Amazon Linux/Amazon Linux 2 and Windows Server from the following paths:
+ Amazon Linux/Amazon Linux 2: `/aws/service/ami-amazon-linux-latest`
+ Windows Server: `/aws/service/ami-windows-latest`

## Calling AMI public parameters for Amazon Linux and Amazon Linux 2<a name="public-parameters-ami-linux"></a>

You can view a list of all Amazon Linux and Amazon Linux 2 AMIs in the current AWS Region by using the following command in the AWS CLI\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/ami-amazon-linux-latest \
    --query 'Parameters[].Name'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/ami-amazon-linux-latest ^
    --query Parameters[].Name
```

------

The command returns information like the following\.

```
[
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-pv-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-pv-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-arm64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-minimal-hvm-arm64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-pv-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-pv-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-minimal-hvm-x86_64-ebs"
]
```

You can view details about these AMIs, including the AMI IDs and Amazon Resource Names \(ARNs\), by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service/ami-amazon-linux-latest" \
    --region region
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service/ami-amazon-linux-latest" ^
    --region region
```

------

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

The command returns information like the following\. This example output has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
            "Type": "String",
            "Value": "ami-0d75cc1d706735521",
            "Version": 7,
            "LastModifiedDate": 1543873943.358,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-0cd3dfa4e37921605",
            "Version": 7,
            "LastModifiedDate": 1543873943.47,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
            "Type": "String",
            "Value": "ami-0a0e3ff8af8d19497",
            "Version": 7,
            "LastModifiedDate": 1543873943.576,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs",
            "Type": "String",
            "Value": "ami-0786a9626196d6dac",
            "Version": 7,
            "LastModifiedDate": 1543873943.682,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs"
        },
```

You can view details of a specific AMI by using the [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API action with the full AMI name, including the path\. Here is an example command\.

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 \
    --region us-west-2
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 ^
    --region us-west-2
```

------

The command returns the following information\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-061392db613a6357b",
            "Version": 16,
            "LastModifiedDate": 1552519670.776,
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
        }
    ],
    "InvalidParameters": []
}
```

## Calling AMI public parameters for Windows Server<a name="public-parameters-ami-windows"></a>

You can view a list of all Windows Server AMIs in the current AWS Region by using the following command in the AWS CLI\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/ami-windows-latest \
    --query 'Parameters[].Name'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/ami-windows-latest ^
    --query Parameters[].Name
```

------

The command returns information like the following\. This example output has been truncated for space\.

```
[
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Simplified-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Traditional-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Dutch-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Hungarian-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Japanese-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
    "/aws/service/ami-windows-latest/Windows_Server-2016-German-Full-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2016-Japanese-Full-SQL_2017_Web",
    "/aws/service/ami-windows-latest/amzn2-ami-hvm-2.0.20190313-x86_64-gp2-SQL_2017_Express",
    "/aws/service/ami-windows-latest/amzn2-ami-hvm-2.0.20191217.0-x86_64-gp2-mono",
    "/aws/service/ami-windows-latest/EC2LaunchV2_Preview-Windows_Server-2019-English-Core-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-English-64Bit-SQL_2016_SP2_Standard",
    "/aws/service/ami-windows-latest/Windows_Server-2012-RTM-Italian-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2016-English-Deep-Learning",
    "/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-SQL_2014_SP3_Enterprise",
    "/aws/service/ami-windows-latest/Windows_Server-2016-Korean-Full-SQL_2016_SP2_Standard",
    "/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-EKS_Optimized-1.17",
]
```

You can view details about these AMIs, including the AMI IDs and Amazon Resource Names \(ARNs\), by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service//aws/service/ami-windows-latest" \
    --region region
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service//aws/service/ami-windows-latest" ^
    --region region
```

------

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

The command returns information like the following\. This example output has been truncated for space\.

```
{
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
            "Type": "String",
            "Value": "ami-0b19f0a8c08620b34",
            "Version": 53,
            "LastModifiedDate": "2020-09-10T19:49:05.760000-07:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2016-German-Full-Base",
            "Type": "String",
            "Value": "ami-068badb2c4fe9d831",
            "Version": 53,
            "LastModifiedDate": "2020-09-10T19:53:44.049000-07:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/ami-windows-latest/Windows_Server-2016-German-Full-Base",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2016-Japanese-Full-SQL_2017_Web",
            "Type": "String",
            "Value": "ami-0b12e609d6733dac7",
            "Version": 20,
            "LastModifiedDate": "2020-09-10T19:55:24.166000-07:00",
            "ARN": "arn:aws:ssm:us-east-1::parameter/aws/service/ami-windows-latest/Windows_Server-2016-Japanese-Full-SQL_2017_Web",
            "DataType": "text"
        },
```

You can view details of a specific AMI by using the [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API action with the full AMI name, including the path\. Here is an example command\.

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers \
    --region us-west-2
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers ^
    --region us-west-2
```

------

The command returns the following information\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
            "Type": "String",
            "Value": "ami-0315ac376f3dac169",
            "Version": 53,
            "LastModifiedDate": "2020-09-10T19:50:01.955000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
            "DataType": "text"
        }
    ],
    "InvalidParameters": []
}
```