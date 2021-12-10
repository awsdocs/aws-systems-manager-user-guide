# Running Auto Scaling groups with associations<a name="systems-manager-state-manager-asg"></a>

The best practice when using associations to run Auto Scaling groups is to use tag targets\. Not using tags might cause you to reach the association limit\. 

If all instances are tagged with the same key and value, you only need one association to run your Auto Scaling group\. The following procedure describes how to create such an association\.

**To create an association that runs Auto Scaling groups**

1. Ensure all instances in the Auto Scaling group are tagged with the same key and value\. For more instructions on tagging instances, see [Tagging Auto Scaling groups and instances](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-tagging.html) in the *AWS Auto Scaling User Guide*\. 

1. Create an association by using the procedure in [Creating associations](sysman-state-assoc.md)\. 

   If you're working in the console, choose **Specify instance tags** in the **Targets** field\. For **Instance tags**, enter the **Tag** key and value for your Auto Scaling group\.

   If you're using the AWS Command Line Interface \(AWS CLI\), specify `--targets Key=tag:tag-key,Values=tag-value` where the key and value match what you tagged your instances with\. 