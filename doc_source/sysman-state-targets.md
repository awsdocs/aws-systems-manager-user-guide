# Create an Association Using the 'Targets' Parameter \(CLI\)<a name="sysman-state-targets"></a>

You can create associations on tens, hundreds, or thousands of instances by using the `targets` parameter\. The `targets` parameter accepts a `Key,Value` combination based on Amazon EC2 tags that you specified for your instances\. When you run the request to create the association, the system locates and attempts to create the association on all instances that match the specified criteria\. For more information about the `targets` parameter, see, [Sending Commands to a Fleet](send-commands-multiple.md)\. For more information about Amazon EC2 tags, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide*\.

The following AWS CLI examples show you how to use the `targets` parameter when creating associations\. 

```
aws ssm create-association --targets Key=tag:TagKey,Values=TagValue --name AWS-UpdateSSMAgent --schedule "cron(0 0 2 ? * SUN *)"
```

Create an association for a managed instance named "ws\-0123456789012345"

```
aws ssm create-association --association-name value --targets Key=Instance Ids,Values=ws-0123456789 --name AWS-UpdateSSMAgent --schedule "cron(0 0 2 ? * SUN *)"
```

**Note**  
If you remove an instance from a tagged group that’s associated with a document, then the instance will be dissociated from the document\. 