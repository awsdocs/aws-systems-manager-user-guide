# Calling the EKS optimized AMI public parameter<a name="parameter-store-public-parameters-eks"></a>

The Amazon Elastic Kubernetes Service \(Amazon EKS\) service publishes the name of the latest Amazon EKS optimized Amazon Machine Image \(AMI\) as a public parameter\. We encourage you to use this AMI when adding nodes to an Amazon EKS cluster, as new releases include Kubernetes patches and security updates\. Previously, to guarantee you were using the latest AMI meant checking the Amazon EKS documentation and manually updating any deployment templates or resources with the new AMI ID\.

Use the following command to view the name of the latest Amazon EKS optimized AMI\.

------
#### [ Linux & macOS ]

```
aws ssm get-parameters \
    --names /aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended",
            "Type": "String",
            "Value": "{\"schema_version\":\"2\",\"image_id\":\"ami-0b89776dcfa5f2dee\",\"image_name\":\"amazon-eks-node-1.14-v20200507\",\"release_version\":\"1.14.9-20200507\"}",
            "Version": 11,
            "LastModifiedDate": "2020-05-11T17:34:51.990000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended"
        }
    ],
    "InvalidParameters": []
}
```