# Step 1: Create the Maintenance Window \(AWS CLI\)<a name="mw-cli-tutorial-create-mw"></a>

In this step, you create a Maintenance Window and specify its basic options, such as schedule and duration\. In later steps, you choose the instance it updates and the task it runs\.

**Note**  
Normally, you would not run a Maintenance Window as frequently as every two minutes\. However, for the purpose of this tutorial, this rate lets you see your Maintenance Window execution results quickly\. You could instead, for example, create a Maintenance Window that runs every 4 hours for 2 hours, in the United States Pacific time zone, with a 1 hour cutoff, and that allows unassociated targets, by running this command:  

```
aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --start-date 2019-01-01T00:00:00-05:00 --schedule-timezone "America/Los_Angeles" --tags "Key=Environment,Value=Production"  --schedule "cron(0 0 */4 * * ? *)" --duration 2 --cutoff 1 --allow-unassociated-targets
```
For information about creating cron expressions for the `schedule` parameter, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.  
For an explanation of how the various schedule\-related options for Maintenance Windows relate to one another, see [Reference: Maintenance Window Scheduling and Active Period Options](reference-maintenance-windows-schedule-options.md)\.

**To create a Maintenance Window \(AWS CLI\)**

1. Open the AWS CLI and run the following command to create a Maintenance Window that runs every two minutes for up to two hours \(as needed\), in the United States Pacific time zone, with a one hour cutoff, that allows unassociated targets, and that is tagged to indicate that it is for a production environment:

   ```
   aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --schedule "rate(2 minutes)" --duration 2 --schedule-timezone "America/Los_Angeles" --tags "Key=Environment,Value=Production" --cutoff 1 --allow-unassociated-targets
   ```

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-0c5ed765acEXAMPLE"
   }
   ```

1. Now run this command to view details about this and any other Maintenance Windows already in your account:

   ```
   aws ssm describe-maintenance-windows
   ```

   The system returns information like the following:

   ```
   {
      "WindowIdentities":[
         {
            "Duration":2,
            "ScheduleTimezone": "America/Los_Angeles",
            "Cutoff":1,
            "WindowId":"mw-0c5ed765acEXAMPLE",
            "Enabled":true,
            "Name":"My-First-Maintenance-Window"
         }
      ]
   }
   ```

1. Make a note of your Maintenance Window ID \(the `WindowId` value\)\. You use it in [Step 2: Register a Target Instance with the Maintenance Window \(AWS CLI\)](mw-cli-tutorial-targets.md)\.