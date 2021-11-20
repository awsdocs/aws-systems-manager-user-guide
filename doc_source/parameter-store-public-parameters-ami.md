# Calling AMI public parameters<a name="parameter-store-public-parameters-ami"></a>

Amazon Elastic Compute Cloud \(Amazon EC2\) Amazon Machine Image \(AMI\) public parameters are available for Amazon Linux, Amazon Linux 2, and Windows Server from the following paths:
+ Amazon Linux and Amazon Linux 2: `/aws/service/ami-amazon-linux-latest`
+ Windows Server: `/aws/service/ami-windows-latest`



## Calling AMI public parameters for Amazon Linux and Amazon Linux 2<a name="public-parameters-ami-linux"></a>

You can view a list of all Amazon Linux and Amazon Linux 2 AMIs in the current AWS Region by using the following command in the AWS Command Line Interface \(AWS CLI\)\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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
            "Value": "ami-02f31e7644d23a001",
            "Version": 32,
            "LastModifiedDate": "2021-10-04T14:51:40.313000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-0a787ac2e0c399e8b",
            "Version": 32,
            "LastModifiedDate": "2021-10-04T14:51:40.424000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
            "Type": "String",
            "Value": "ami-0437136c909273ff3",
            "Version": 32,
            "LastModifiedDate": "2021-10-04T14:51:40.533000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
            "DataType": "text"
        },
```

You can view details of a specific AMI by using the [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API operation with the full AMI name, including the path\. Here is an example command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters \
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 \
    --region us-east-2
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 ^
    --region us-east-2
```

------

The command returns the following information\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-074cce78125f09d61",
            "Version": 51,
            "LastModifiedDate": "2021-10-06T16:50:43.294000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
            "DataType": "text"
        }
    ],
    "InvalidParameters": []
}
```

## Calling AMI public parameters for Windows Server<a name="public-parameters-ami-windows"></a>

You can view a list of all Windows Server AMIs in the current AWS Region by using the following command in the AWS CLI\.

------
#### [ Linux & macOS ]

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
    "/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-EKS_Optimized-1.17",
    "/aws/service/ami-windows-latest/amzn2-ami-hvm-2.0.20191217.0-x86_64-gp2-mono",
    "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-English-64Bit-SQL_2016_SP2_Standard",
    "/aws/service/ami-windows-latest/Windows_Server-2012-RTM-Italian-64Bit-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2016-English-Deep-Learning",
    "/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-SQL_2014_SP3_Enterprise",
    "/aws/service/ami-windows-latest/Windows_Server-2019-English-Core-EKS_Optimized-1.21",
    "/aws/service/ami-windows-latest/Windows_Server-2019-Italian-Full-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2019-Japanese-Full-SQL_2017_Enterprise",
    "/aws/service/ami-windows-latest/Windows_Server-2022-Italian-Full-Base",
    "/aws/service/ami-windows-latest/Windows_Server-2022-Portuguese_Brazil-Full-Base",
    "/aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ContainersLatest",
    "/aws/service/ami-windows-latest/EC2LaunchV2_Preview-Windows_Server-2019-English-Core-Base",
]
```

You can view details about these AMIs, including the AMI IDs and Amazon Resource Names \(ARNs\), by using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service/ami-windows-latest" \
    --region region
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service/ami-windows-latest" ^
    --region region
```

------

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

The command returns information like the following\. This example output has been truncated for space\.

```
{
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Simplified-64Bit-Base",
            "Type": "String",
            "Value": "ami-0b7899820bf83a6a1",
            "Version": 70,
            "LastModifiedDate": "2021-09-16T17:20:07.716000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Simplified-64Bit-Base",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Traditional-64Bit-Base",
            "Type": "String",
            "Value": "ami-0fb7114182bd85f9f",
            "Version": 70,
            "LastModifiedDate": "2021-09-16T17:20:22.660000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Chinese_Traditional-64Bit-Base",
            "DataType": "text"
        },
        {
            "Name": "/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Dutch-64Bit-Base",
            "Type": "String",
            "Value": "ami-0ae137ae6fe86b15a",
            "Version": 70,
            "LastModifiedDate": "2021-09-16T17:20:40.545000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-windows-latest/Windows_Server-2012-R2_RTM-Dutch-64Bit-Base",-- Mor            "DataType": "text"
        },
```

You can view details of a specific AMI by using the [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API operation with the full AMI name, including the path\. Here is an example command\.

------
#### [ Linux & macOS ]

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
            "Value": "ami-06eeda984b8f887b7",
            "Version": 66,
            "LastModifiedDate": "2021-09-16T17:30:55.502000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-windows-latest/Windows_Server-2016-English-Core-Containers",
            "DataType": "text"
        }
    ],
    "InvalidParameters": []
}
```