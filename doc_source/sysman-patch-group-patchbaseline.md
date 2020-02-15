# Add a Patch Group to a Patch Baseline<a name="sysman-patch-group-patchbaseline"></a>

To associate a specific patch baseline with your instances, you must add the patch group value to the patch baseline\. By registering the patch group with a patch baseline, you can ensure that the correct patches are installed during a patching operation\. For more information about patch groups, see [About Patch Groups](sysman-patch-patchgroups.md)\.

**To add a patch group to a patch baseline \(Console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

1. In the **Patch Baselines** list, choose the patch baseline you want to configure for your patch group\.

1. Choose **Actions**, then **Modify patch groups**\.

1. Enter the tag value you added to your managed instances in the previous section, then choose **Add**\.

**To add a patch group to a patch baseline \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md)\.

1. Run the following command to associate a `Patch Group` tag value to the specified patch baseline\.

------
#### [ Linux ]

   ```
   aws ssm register-patch-baseline-for-patch-group \
       --baseline-id "pb-0123456789abcdef0" \
       --patch-group "Development"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-patch-baseline-for-patch-group ^
       --baseline-id "pb-0123456789abcdef0" ^
       --patch-group "Development"
   ```

------

   The system returns information like the following:

   ```
   {
     "PatchGroup": "Development",
     "BaselineId": "pb-0123456789abcdef0"
   }
   ```