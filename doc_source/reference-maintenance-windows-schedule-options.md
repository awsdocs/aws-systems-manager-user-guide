# Reference: Maintenance Window Scheduling and Active Period Options<a name="reference-maintenance-windows-schedule-options"></a>

When you create a Maintenance Window, you must specify how often the Maintenance Window runs by using a [Cron or Rate expression](reference-cron-and-rate-expressions.md)\. Optionally, you can specify a date range during which the Maintenance Window can run on its regular schedule, as well as a time zone on which to base that regular schedule\. 

Be aware, however, that the time zone option and the start date/end date options do not influence each other\. Any start date and end date times that you specify \(with or without an offset for your time zone\) determine only the *valid period* during which the Maintenance Window can run on its schedule\. A time zone option determines the international time zone that the Maintenance Window schedule is based on *during* its valid period\.

**Note**  
You specify start and end dates in ISO\-8601 timestamp format\. For example: `2019-04-07T14:29:00-08:00`  
You specify time zones in Internet Assigned Numbers Authority \(IANA\) format\. For example: `America/Chicago`, `Europe/Berlin` or `Asia/Tokyo`

**Example 1**  
Say that you use the AWS CLI to create a Maintenance Window with the following options:
+ `--start-date 2019-01-01T00:00:00-05:00`
+ `--schedule-timezone "America/Los_Angeles"`
+ `--schedule "cron(0 09 ? * FRI *)"`

For example:

```
aws ssm create-maintenance-window --name "My-LAX-Maintenance-Window" --start-date 2019-01-01T00:00:00-08:00 --schedule-timezone "America/Los_Angeles" --schedule "cron(0 09 ? * FRI *)"
```

This means that the Maintenance Window won't be able to run until after its specified start date and time, which is at Midnight US Eastern Time on Tuesday, January 1, 2019\. \(This time zone is five hours behind UTC time\.\) Taken together, the `--schedule-timezone` and `--schedule` values mean that the Maintenance Window will run at 9 AM every Friday in the US Pacific Time Zone \(represented by "America/Los Angeles" in IANA format\)\. The first execution in the enabled period will be on Friday, January 4th, 2019, at 9 AM US Pacific Time\.

**Example 2**  
Suppose that next you create a Maintenance Window with these options:
+ `--start-date 2019-01-01T00:03:15+09:00`
+ `--end-date 2019-06-30T00:06:15+09:00`
+ `--schedule-timezone "Asia/Tokyo"`
+ `--schedule "rate(7 days)"`

For example:

```
aws ssm create-maintenance-window --name "My-NRT-Maintenance-Window" --start-date 2019-01-01T00:03:15+09:00 --end-date 2019-06-30T00:06:15+09:00 --schedule-timezone "Asia/Tokyo" --schedule "rate(7 days)""
```

The enabled period for this Maintenance Window begins at 3:15 AM Japan Standard Time on January 1, 2019\. The valid period for this Maintenance Window ends at 6:15 AM Japan Standard Time on Sunday, June 30, 2019\. \(This time zone is nine hours ahead of UTC time\.\) Taken together, the `--schedule-timezone` and `--schedule` values mean that the Maintenance Window will run at 3:15 AM every Tuesday in the Japan Standard Time Zone \(represented by "Asia/Tokyo" in IANA format\)\. This is because the Maintenance Window runs every seven days, and it becomes active at 3:15 AM on Tuesday, January 1st\. The last execution will be at 3:15 AM Japan Standard Time on Tuesday, June 25, 2019\. This is the last Tuesday before the enabled Maintenance Window period ends five days later\.

**Related Content**
+ [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)
+ [Create a Maintenance Window \(Console\)](sysman-maintenance-create-mw.md)
+ [Tutorial: Create and Configure a Maintenance Window \(CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [CreateMaintenanceWindow](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateMaintenanceWindow.html) in the *AWS Systems Manager API Reference*
+ [create\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-maintenance-window.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*
+ [Time Zone Database](https://www.iana.org/time-zones) on the IANA website