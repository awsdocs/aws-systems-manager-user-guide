# Step 1: Create the Maintenance Window \(AWS CLI\)<a name="mw-cli-tutorial-create-mw"></a>

In this step, you'll create a Maintenance Window and specify its basic options, such as schedule and duration\. In later steps, you'll choose the instances it will update and the tasks that it will run\.

**To create a Maintenance Window \(AWS CLI\)**

1. Open the AWS CLI and run the following commands to create a Maintenance Window that runs every 2 minutes for 2 hours, in the United States Pacific time zone, with a 1 hour cutoff, and that allows unassociated targets, and that is tagged to indicate that it is for a production environment:

   ```
   aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --schedule "rate(2 minutes)" --duration 2 --schedule-timezone "America/Los_Angeles" --tags "Key=Environment,Value=Production" --cutoff 1 --allow-unassociated-targets
   ```
**Note**  
In practice, a Maintenance Window task would not normally run this frequently\. However, for the purpose of these tutorials, this rate will allow you to see your Maintenance Window execution results quickly\. You could instead, for example, create a Maintenance Window that runs every 4 hours for 2 hours, in the United States Pacific time zone, with a 1 hour cutoff, and that allows unassociated targets, by running this command:  

   ```
   aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --start-date 2019-01-01T00:00:00-05:00 --schedule-timezone "America/Los_Angeles" --tags "Key=Environment,Value=Production"  --schedule "cron(0 0 */4 * * ? *)" --duration 2 --cutoff 1 --allow-unassociated-targets
   ```
For information about creating cron expressions for the `schedule` parameter, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.  
For an explanation of how the various schedule\-related options for Maintenance Windows relate to one another, see [Reference: Maintenance Window Scheduling and Active Period Options](reference-maintenance-windows-schedule-options.md)\.

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-0c5ed765acEXAMPLE"
   }
   ```

1. To list this and any other Maintenance Windows created in your AWS account in your current AWS Region, run the following command:

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