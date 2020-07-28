# Step 1: Create the maintenance window \(AWS CLI\)<a name="mw-cli-tutorial-create-mw"></a>

In this step, you create a maintenance window and specify its basic options, such as name, schedule, and duration\. In later steps, you choose the instance it updates and the task it runs\.

In our example, you create a maintenance window that runs every five minutes\. Normally, you would not run a maintenance window this frequently\. However, this rate lets you see your tutorial results quickly\. We'll show you how to change to a less frequent rate after the task has run successfully\.

**Note**  
For an explanation of how the various schedule\-related options for maintenance windows relate to one another, see [Reference: Maintenance window scheduling and active period options](maintenance-windows-schedule-options.md)\.  
For more information about working with the `--schedule` option, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

**To create a maintenance window \(AWS CLI\)**

1. Open the AWS CLI and run the following command on your local machine to create a maintenance window that does the following:
   + Runs every five minutes for up to two hours \(as needed\)\.
   + Prevents new tasks from starting within one hour of the end of the maintenance window execution\.
   + Allows unassociated targets \(instances that you haven't registered with the maintenance window\)\.
   + Indicates through the use of custom tags that its creator intends to use it in a tutorial\.

------
#### [ Linux ]

   ```
   aws ssm create-maintenance-window \
       --name "My-First-Maintenance-Window" \
       --schedule "rate(5 minutes)" \
       --duration 2 \
       --cutoff 1 \
       --allow-unassociated-targets \
       --tags "Key=Purpose,Value=Tutorial"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-maintenance-window ^
       --name "My-First-Maintenance-Window" ^
       --schedule "rate(5 minutes)" ^
       --duration 2 ^
       --cutoff 1 ^
       --allow-unassociated-targets ^
       --tags "Key"="Purpose","Value"="Tutorial"
   ```

------

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-0c50858d01EXAMPLE"
   }
   ```

1. Now run this command to view details about this and any other maintenance windows already in your account:

   ```
   aws ssm describe-maintenance-windows
   ```

   The system returns information like the following:

   ```
   {
      "WindowIdentities":[
         {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Name": "My-First-Maintenance-Window",
               "Enabled": true,
               "Duration": 2,
               "Cutoff": 1,
               "NextExecutionTime": "2019-05-11T16:46:16.991Z"
         }
      ]
   }
   ```

Continue to [Step 2: Register a target instance with the maintenance window \(AWS CLI\)](mw-cli-tutorial-targets.md)\.