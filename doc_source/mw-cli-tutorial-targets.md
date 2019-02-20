# Step 2: Register Target Instances with the Maintenance Window \(AWS CLI\)<a name="mw-cli-tutorial-targets"></a>

In this step, you'll register a target with your new Maintenance Window\. In other words, you are specifying which instance the Maintenance Window will update\.

**To register target instances with a Maintenance Window \(AWS CLI\)**

1. Run the following command:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --target "Key=InstanceIds,Values=i-1234567890EXAMPLE" --resource-type "INSTANCE"
   ```

   The system returns information like the following, which includes a Maintenance Window target ID\. You'll use this ID in a later step to register a task for this Maintenance Window\.

   ```
   {
      "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

**Alternative commands**  
You could instead register multiple instances using the following command\.

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets "Key=InstanceIds,Values=i-1234567890EXAMPLE,i-abcdefghiEXAMPLE" --resource-type "INSTANCE"
   ```

   You could also register instances using Amazon EC2 tags\. For example:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets "Key=tag:Environment,Values=Prod" "Key=Role,Values=Web" --resource-type "INSTANCE"
   ```

1. Run the following command to display the targets for a Maintenance Window:

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
         },
         {
            "ResourceType":"INSTANCE",
            "WindowId":"mw-9a8b7c6d5eEXAMPLE",
            "Targets":[
               {
                  "Values":[
                     "i-456jkl321EXAMPLE",
                     "i-abcdefghiEXAMPLE"
                  ],
                  "Key":"InstanceIds"
               }
            ],
            "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
         },
         {
            "ResourceType":"INSTANCE",
            "WindowId":"mw-0c5ed765acEXAMPLE",
            "Targets":[
               {
                  "Values":[
                     "Prod"
                  ],
                  "Key":"tag:Environment"
               },
               {
                  "Values":[
                     "Web"
                  ],
                  "Key":"tag:Role"
               }
            ],
            "WindowTargetId":"1111aaa-2222-3333-4444-1111aaa "
         }
      ]
   }
   ```