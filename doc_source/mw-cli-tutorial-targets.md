# Step 2: Register a target instance with the maintenance window \(AWS CLI\)<a name="mw-cli-tutorial-targets"></a>

In this step, you register a target with your new maintenance window\. In this case, you specify which instance to update when the maintenance window runs\. 

For an example of registering more than one instance at a time using instance IDs, examples of using tags to identify multiple instances, and examples of specifying resource groups as targets, see [Examples: Register targets with a maintenance window](mw-cli-tutorial-targets-examples.md)\.

**Note**  
You should already have created an EC2 instance to use in this step, as described in the [Maintenance Windows tutorial prerequisites](maintenance-windows-tutorials.md)\.

**To register a target instance with a maintenance window \(AWS CLI\)**

1. Run the following command on your local machine:

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --resource-type "INSTANCE" \
       --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --resource-type "INSTANCE" ^
       --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE"
   ```

------

   The system returns information like the following:

   ```
   {
      "WindowTargetId":"e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
   }
   ```

1. Now run the following command on your local machine to view details about your maintenance window target:

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-targets \
       --window-id "mw-0c50858d01EXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-targets ^
       --window-id "mw-0c50858d01EXAMPLE"
   ```

------

   The system returns information like the following:

   ```
   {
       "Targets": [
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE",
               "ResourceType": "INSTANCE",
               "Targets": [
                   {
                       "Key": "InstanceIds",
                       "Values": [
                           "i-02573cafcfEXAMPLE"
                       ]
                   }
               ]
           }
       ]
   }
   ```

Continue to [Step 3: Register a task with the maintenance window \(AWS CLI\)](mw-cli-tutorial-tasks.md)\. 