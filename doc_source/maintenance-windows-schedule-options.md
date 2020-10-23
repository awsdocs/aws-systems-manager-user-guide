# Reference: Maintenance window scheduling and active period options<a name="maintenance-windows-schedule-options"></a>

When you create a maintenance window, you must specify how often the maintenance window runs by using a [Cron or rate expression](reference-cron-and-rate-expressions.md)\. Optionally, you can specify a date range during which the maintenance window can run on its regular schedule, as well as a time zone on which to base that regular schedule\. 

Be aware, however, that the time zone option and the start date/end date options do not influence each other\. Any start date and end date times that you specify \(with or without an offset for your time zone\) determine only the *valid period* during which the maintenance window can run on its schedule\. A time zone option determines the international time zone that the maintenance window schedule is based on *during* its valid period\.

**Note**  
You specify start and end dates in ISO\-8601 timestamp format\. For example: `2021-04-07T14:29:00-08:00`  
You specify time zones in Internet Assigned Numbers Authority \(IANA\) format\. For example: `America/Chicago`, `Europe/Berlin` or `Asia/Tokyo`

**Topics**
+ [Example 1: Specify a maintenance window start date](#schedule-example-start-date)
+ [Example 2: Specify a maintenance window start date and end date](#schedule-example-start-end-date)
+ [Example 3: Create a maintenance window that runs only once](#schedule-example-one-time)
+ [Example 4: Specify the number of schedule offset days for a maintenance window](#schedule-example-schedule-offset)

## Example 1: Specify a maintenance window start date<a name="schedule-example-start-date"></a>

Say that you use the AWS CLI to create a maintenance window with the following options:
+ `--start-date 2021-01-01T00:00:00-08:00`
+ `--schedule-timezone "America/Los_Angeles"`
+ `--schedule "cron(0 09 ? * WED *)"`

For example:

------
#### [ Linux ]

```
aws ssm create-maintenance-window \
    --name "My-LAX-Maintenance-Window" \
    --allow-unassociated-targets \
    --duration 3 \
    --cutoff 1 \
    --start-date 2021-01-01T00:00:00-08:00 \
    --schedule-timezone "America/Los_Angeles" \
    --schedule "cron(0 09 ? * WED *)"
```

------
#### [ Windows ]

```
aws ssm create-maintenance-window ^
    --name "My-LAX-Maintenance-Window" ^
    --allow-unassociated-targets ^
    --duration 3 ^
    --cutoff 1 ^
    --start-date 2021-01-01T00:00:00-08:00 ^
    --schedule-timezone "America/Los_Angeles" ^
    --schedule "cron(0 09 ? * WED *)"
```

------

This means that the first run of the maintenance window won't occur until *after* its specified start date and time, which is at 12:00 AM US Pacific Time on Friday, January 1, 2021\. \(This time zone is eight hours behind UTC time\.\) Note that in this case, the start date and time of the window period don't represent when the maintenance windows first runs\. Taken together, the `--schedule-timezone` and `--schedule` values mean that the maintenance window runs at 9 AM every Wednesday in the US Pacific Time Zone \(represented by "America/Los Angeles" in IANA format\)\. The first execution in the enabled period will be on Wednesday, January 4, 2021, at 9 AM US Pacific Time\.

## Example 2: Specify a maintenance window start date and end date<a name="schedule-example-start-end-date"></a>

Suppose that next you create a maintenance window with these options:
+ `--start-date 2019-01-01T00:03:15+09:00`
+ `--end-date 2019-06-30T00:06:15+09:00`
+ `--schedule-timezone "Asia/Tokyo"`
+ `--schedule "rate(7 days)"`

For example:

------
#### [ Linux ]

```
aws ssm create-maintenance-window \
    --name "My-NRT-Maintenance-Window" \
    --allow-unassociated-targets \
    --duration 3 \
    --cutoff 1 \
    --start-date 2019-01-01T00:03:15+09:00 \
    --end-date 2019-06-30T00:06:15+09:00 \
    --schedule-timezone "Asia/Tokyo" \
    --schedule "rate(7 days)"
```

------
#### [ Windows ]

```
aws ssm create-maintenance-window ^
    --name "My-NRT-Maintenance-Window" ^
    --allow-unassociated-targets ^
    --duration 3 ^
    --cutoff 1 ^
    --start-date 2019-01-01T00:03:15+09:00 ^
    --end-date 2019-06-30T00:06:15+09:00 ^
    --schedule-timezone "Asia/Tokyo" ^
    --schedule "rate(7 days)"
```

------

The enabled period for this maintenance window begins at 3:15 AM Japan Standard Time on January 1, 2019\. The valid period for this maintenance window ends at 6:15 AM Japan Standard Time on Sunday, June 30, 2019\. \(This time zone is nine hours ahead of UTC time\.\) Taken together, the `--schedule-timezone` and `--schedule` values mean that the maintenance window runs at 3:15 AM every Tuesday in the Japan Standard Time Zone \(represented by "Asia/Tokyo" in IANA format\)\. This is because the maintenance window runs every seven days, and it becomes active at 3:15 AM on Tuesday, January 1st\. The last execution is at 3:15 AM Japan Standard Time on Tuesday, June 25, 2019\. This is the last Tuesday before the enabled maintenance window period ends five days later\.

## Example 3: Create a maintenance window that runs only once<a name="schedule-example-one-time"></a>

Now you create a maintenance window with this option:
+ `--schedule "at(2020-07-07T15:55:00)"`

For example:

------
#### [ Linux ]

```
aws ssm create-maintenance-window \
    --name "My-One-Time-Maintenance-Window" \
    --schedule "at(2020-07-07T15:55:00)" \
    --duration 5 \
    --cutoff 2 \
    --allow-unassociated-targets
```

------
#### [ Windows ]

```
aws ssm create-maintenance-window ^
    --name "My-One-Time-Maintenance-Window" ^
    --schedule "at(2020-07-07T15:55:00)" ^
    --duration 5 ^
    --cutoff 2 ^
    --allow-unassociated-targets
```

------

This maintenance window runs just once, at 3:55 PM UTC time on July 7, 2020\. The maintenance window is enabled to run up to five hours, as needed, but new tasks are prevented from starting two hours before the end of the maintenance window period\.

## Example 4: Specify the number of schedule offset days for a maintenance window<a name="schedule-example-schedule-offset"></a>

Now you create a maintenance window with this option:

```
--schedule-offset 2
```

For example:

------
#### [ Linux ]

```
aws ssm create-maintenance-window \
    --name "My-Cron-Offset-Maintenance-Window" \
    --schedule "cron(0 30 23 ? * TUE#3 *)" \
    --duration 4 \
    --cutoff 1 \
    --schedule-offset 2 \
    --allow-unassociated-targets
```

------
#### [ Windows ]

```
aws ssm create-maintenance-window ^
    --name "My-Cron-Offset-Maintenance-Window" ^
    --schedule "cron(0 30 23 ? * TUE#3 *)" ^
    --duration 4 ^
    --cutoff 1 ^
    --schedule-offset 2 ^
    --allow-unassociated-targets
```

------

A schedule offset is the number of days to wait after the date and time specified by a CRON expression before running the maintenance window\.

In the example above, the CRON expression schedules a maintenance window to run the third Tuesday of every month at 11:30 PM: 

```
--schedule "cron(0 30 23 ? * TUE#3 *)
```

However, including `--schedule-offset 2` means that the maintenance window won't run until 11:30 PM two days *after* the third Tuesday of each month\. 

Schedule offsets are supported for CRON expressions only\. 

**Related content**
+ [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)
+ [Create a maintenance window \(console\)](sysman-maintenance-create-mw.md)
+ [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [CreateMaintenanceWindow](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateMaintenanceWindow.html) in the *AWS Systems Manager API Reference*
+ [create\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-maintenance-window.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*
+ [Time Zone Database](https://www.iana.org/time-zones) on the IANA website