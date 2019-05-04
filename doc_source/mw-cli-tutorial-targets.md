# Step 2: Register a Target Instance with the Maintenance Window \(AWS CLI\)<a name="mw-cli-tutorial-targets"></a>

In this step, you register a target with your new Maintenance Window\. In other words, you specify which instance to update when the Maintenance Window runs\.

For examples of registering more than one instance at a time using instance IDs, and using instance tags to identify targets, see [Examples: Register Targets with a Maintenance Window](mw-cli-tutorial-targets-examples.md)\.

**To register a target instance with a Maintenance Window \(AWS CLI\)**

1. Run the following command:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --resource-type "INSTANCE" --target "Key=InstanceIds,Values=i-1234567890EXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
      "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

1. Now run the following command to view details about your Maintenance Window target:

   ```
   aws ssm describe-maintenance-window-targets --window-id "mw-0c5ed765acEXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
      "Targets":[
         {
            "ResourceType":"INSTANCE",
            "WindowId":"mw-0c5ed765acEXAMPLE",
            "Targets":[
               {
                  "Values":[
                     "i-1234567890EXAMPLE"
                  ],
                  "Key":"InstanceIds"
               }
            ],
            "WindowTargetId":"a1b2c3d4-a1b2-a1b2-a1b2-a1b2c3d4"
         }
   }
   ```

1. Make a note of your Maintenance Window target ID \(the `WindowTargetId` value\)\. You can use it in [Step 3: Register Tasks with the Maintenance Window](mw-cli-tutorial-tasks.md)\. 