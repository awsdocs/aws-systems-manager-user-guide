# Calling the ECS optimized AMI public parameter<a name="parameter-store-public-parameters-ecs"></a>

The Amazon Elastic Container Service \(Amazon ECS\) service publishes the name of the latest Amazon ECS optimized Amazon Machine Image \(AMI\) as a public parameter\. Users are encouraged to use this AMI when creating a new Amazon Elastic Compute Cloud \(Amazon EC2\) cluster for Amazon ECS because the optimized AMI includes bug fixes and feature updates\. Use the following command to view the name of the latest Amazon ECS optimized AMI\.

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ecs/optimized-ami/amazon-linux/recommended",
            "Type": "String",
            "Value": "{\"schema_version\":1,\"image_name\":\"amzn-ami-2018.03.p-amazon-ecs-optimized\",\"image_id\":\"ami-01a82c3fce2c3ba58\",\"os\":\"Amazon Linux\",\"ecs_runtime_version\":\"Docker version 18.06.1-ce\",\"ecs_agent_version\":\"1.27.0\"}",
            "Version": 22,
            "LastModifiedDate": 1555434745.425,
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/ecs/optimized-ami/amazon-linux/recommended"
        }
    ],
    "InvalidParameters": []
}
```