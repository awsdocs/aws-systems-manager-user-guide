# Step 2: Register a Target Instance with the Maintenance Window \(AWS CLI\)<a name="mw-cli-tutorial-targets"></a>

In this step, you register a target with your new maintenance window\. This means that you specify which instance to update when the maintenance window runs\. 

For an example of registering more than one instance at a time using instance IDs, and examples of using instance tags to identify multiple targets, see [Examples: Register Targets with a Maintenance Window](mw-cli-tutorial-targets-examples.md)\.

**Note**  
You should already have created an Amazon EC2 instance to use in this step, as described in the [Maintenance Windows tutorial prerequisites](maintenance-windows-tutorials.md)\.

**To register a target instance with a maintenance window \(AWS CLI\)**

1. Run the following command:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" --resource-type "INSTANCE" --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE"
   ```

   The system returns information like the following

   ```
   {
      "WindowTargetId":"e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
   }
   ```

1. Now run the following command to view details about your maintenance window target:

   ```
   aws ssm describe-maintenance-window-targets --window-id "mw-0c50858d01EXAMPLE"
   ```

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

Continue to [Step 3: Register a Task with the Maintenance Window \(AWS CLI\)](mw-cli-tutorial-tasks.md)\. 