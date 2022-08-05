# Reference: Cron and rate expressions for Systems Manager<a name="reference-cron-and-rate-expressions"></a>

When you create a State Manager association or a maintenance window in AWS Systems Manager, you specify a schedule for when the window or the association should run\. You can specify a schedule as either a time\-based entry, called a *cron expression*, or a frequency\-based entry, called a *rate expression*\. 

## General information about cron and rate expressions<a name="reference-cron-and-rate-expressions-intro"></a>

The following information applies to cron and rate expressions for both maintenance windows and associations\.

**Single\-run schedules**  
When you create an association or a maintenance window, you can specify a timestamp in Coordinated Universal Time \(UTC\) format so that it runs once at the specified time\. For example: `"at(2020-07-07T15:55:00)"`

**Schedule offsets**  
Associations and maintenance windows support *schedule offsets* for cron expressions only\. A schedule offset is the number of days to wait after the date and time specified by a cron expression before running the association or maintenance window\. For example, the following cron expression schedules an association or maintenance window to run the third Tuesday of every month at 11:30 PM\.  

```
cron(30 23 ? * TUE#3 *)
```
If the schedule offset is `2`, the maintenance window won't run until 11:30 PM two days later\.  
If you create an association or a maintenance window with a cron expression that targets a day that has already passed in the current period, but add a schedule offset date that falls in the future, the association or maintenance window won't run in the period\. It will go into effect in the following period\. For example, if you specify a cron expression that would have run a maintenance window yesterday and add a schedule offset of two days, the maintenance window won't run tomorrow\. 

**Required fields**  
Cron expressions for maintenance windows have six required fields\. Cron expressions for associations have five\. \(State Manager doesn't currently support specifying months in cron expressions for associations\.\) An additional field, the `Seconds` field \(the first in a cron expression\), is optional\. Fields are separated by a space\.    
**Cron expression examples**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-cron-and-rate-expressions.html)

**Supported values**  
The following table shows supported values for required cron entries\.    
**Supported values for cron expressions**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-cron-and-rate-expressions.html)
You can't specify a value in the day\-of\-month and in the day\-of\-week fields in the same cron expression\. If you specify a value in one of the fields, use a ? \(question mark\) in the other field\.

**Wildcards for cron expressions**  
The following table shows the wildcard values that cron expressions support\.  
Cron expressions that lead to rates faster than five \(5\) minute aren't supported\. Support for specifying both a day\-of\-week and a day\-of\-month value isn't complete\. Use the question mark \(?\) character in one of these fields\.   
**Supported wildcards for cron expressions**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-cron-and-rate-expressions.html)

**Rate expressions**  
Rate expressions have the following two required fields\. Fields are separated by spaces\.    
**Required fields for rate expressions**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-cron-and-rate-expressions.html)
If the value is equal to `1`, then the unit must be singular\. Similarly, for values greater than `1`, the unit must be plural\. For example, `rate(1 hours)` and `rate(5 hour)` aren't valid, but `rate(1 hour)` and `rate(5 hours)` are valid\.

**Topics**
+ [General information about cron and rate expressions](#reference-cron-and-rate-expressions-intro)
+ [Cron and rate expressions for associations](#reference-cron-and-rate-expressions-association)
+ [Cron and rate expressions for maintenance windows](#reference-cron-and-rate-expressions-maintenance-window)

## Cron and rate expressions for associations<a name="reference-cron-and-rate-expressions-association"></a>

This section includes examples of cron and rate expressions for State Manager associations\. Before you create one of these expressions, be aware of the following information:
+ Associations support the following cron expressions: every 1/2, 1, 2, 4, 8, or 12 hours; every day, every week, every *n*th day, or the last *x* day of the month at a specific time\.
+ Associations support the following rate expressions: intervals of 30 minutes or greater and less than 31 days\.
+ If you specify the optional `Seconds` field, its value can be 0 \(zero\)\. For example: `cron(0 */30 * * * ? *)`
+ For an association that collects metadata for Inventory, a capability of AWS Systems Manager, we recommend using a rate expression\.
+ State Manager doesn't currently support specifying months in cron expressions for associations\.

Associations support cron expressions that include a day of the week and the number sign \(\#\) to designate the *n*th day of a month to run an association\. Here is an example that runs a cron schedule on the third Tuesday of every month at 23:30 UTC:

`cron(30 23 ? * TUE#3 *)`

Here is an example that runs on the second Thursday of every month at midnight UTC:

`cron(0 0 ? * THU#2 *)`

Associations also support the \(L\) sign to indicate the last *X* day of the month\. Here is an example that runs a cron schedule on the last Tuesday of every month at midnight UTC:

`cron(0 0 ? * 3L *)`

To further control when an association runs, for example if you want to run an association two days after patch Tuesday, you can specify an offset\. An *offset* defines how many days to wait after the scheduled day to run an association\. For example, if you specified a cron schedule of `cron(0 0 ? * THU#2 *)`, you could specify the number 3 in the **Schedule offset** field to run the association each Sunday after the second Thursday of the month\.

To use offsets, you must either choose the **Apply association only at the next specified Cron interval** option in the console or you must specify the use `--apply-only-at-cron-interval` parameter from the command line\. This option tells State Manager not to run an association immediately after you create it\.

The following table presents cron examples for associations\.


**Cron examples for associations**  

| Example | Details | 
| --- | --- | 
|  cron\(0/30 \* \* \* ? \*\)  |  Every 30 minutes  | 
|  cron\(0 0/1 \* \* ? \*\)  |  Every hour  | 
|  cron\(0 0/2 \* \* ? \*\)  |  Every 2 hours  | 
|  cron\(0 0/4 \* \* ? \*\)  |  Every 4 hours  | 
|  cron\(0 0/8 \* \* ? \*\)  |  Every 8 hours  | 
|  cron\(0 0/12 \* \* ? \*\)  |  Every 12 hours  | 
|  cron\(15 13 ? \* \* \*\)  |  Every day at 1:15 PM  | 
|  cron\(15 13 ? \* MON \*\)  |  Every Monday at 1:15 PM  | 
|  cron\(30 23 ? \* TUE\#3 \*\)  |  The third Tuesday of every month at 11:30 PM  | 

Here are some rate examples for associations\.


**Rate examples for associations**  

| Example | Details | 
| --- | --- | 
|  rate\(30 minutes\)  |  Every 30 minutes  | 
|  rate\(1 hour\)  |  Every hour  | 
|  rate\(5 hours\)  |  Every 5 hours  | 
|  rate\(15 days\)  |  Every 15 days  | 

****AWS CLI examples for associations****  
To create State Manager associations using the AWS CLI, you include the `--schedule-expression` parameter with a cron or rate expression\. The following examples use the AWS CLI on a local Linux machine\.

**Note**  
By default, when you create a new association, the system runs it immediately after it's created and then according to the schedule you specified\. Specify `--apply-only-at-cron-interval` so that the association doesn't run immediately after you create it\. This parameter isn't supported for rate expressions\.

```
aws ssm create-association \
    --association-name "My-Cron-Association" \
    --schedule-expression "cron(0 2 ? * SUN *)" \
    --targets Key=tag:ServerRole,Values=WebServer \
    --name AWS-UpdateSSMAgent
```

```
aws ssm create-association \
    --association-name "My-Rate-Association" \
    --schedule-expression "rate(7 days)" \
    --targets Key=tag:ServerRole,Values=WebServer \
    --name AWS-UpdateSSMAgent
```

```
aws ssm create-association \
    --association-name "My-Rate-Association" \
    --schedule-expression "at(2020-07-07T15:55:00)" \
    --targets Key=tag:ServerRole,Values=WebServer \
    --name AWS-UpdateSSMAgent \
    --apply-only-at-cron-interval
```

## Cron and rate expressions for maintenance windows<a name="reference-cron-and-rate-expressions-maintenance-window"></a>

This section includes examples of cron and rate expressions for maintenance windows\.

Unlike State Manager associations, maintenance windows support all cron and rate expressions\. This includes support for values in the seconds field\.

For example, the following 6\-field cron expression runs a maintenance window at 9:30 AM every day\.

```
cron(30 09 ? * * *)
```

By adding a value to the `Seconds` field, the following 7\-field cron expression runs a maintenance window at 9:30:24 AM every day\.

```
cron(24 30 09 ? * * *)
```

The following table provides additional 6\-field cron examples for maintenance windows\.


**Cron examples for maintenance windows**  

| Example | Details | 
| --- | --- | 
|  cron\(0 2 ? \* THU\#3 \*\)  |  02:00 AM the third Thursday of every month  | 
|  cron\(15 10 ? \* \* \*\)  |  10:15 AM every day  | 
|  cron\(15 10 ? \* MON\-FRI \*\)  |  10:15 AM every Monday, Tuesday, Wednesday, Thursday and Friday  | 
|  cron\(0 2 L \* ? \*\)  |  02:00 AM on the last day of every month  | 
|  cron\(15 10 ? \* 6L \*\)  |  10:15 AM on the last Friday of every month  | 

The following table provides rate examples for maintenance windows\.


**Rate examples for maintenance windows**  

| Example | Details | 
| --- | --- | 
|  rate\(30 minutes\)  |  Every 30 minutes  | 
|  rate\(1 hour\)  |  Every hour  | 
|  rate\(5 hours\)  |  Every 5 hours  | 
|  rate\(25 days\)  |  Every 25 days  | 

****AWS CLI examples for maintenance windows****  
To create maintenance windows using the AWS CLI, you include the `--schedule` parameter with a cron or rate expression or a timestamp\. The following examples use the AWS CLI on a local Linux machine\. 

```
aws ssm create-maintenance-window \
    --name "My-Cron-Maintenance-Window" \
    --allow-unassociated-targets \
    --schedule "cron(0 16 ? * TUE *)" \
    --schedule-timezone "America/Los_Angeles" \
    --start-date 2021-01-01T00:00:00-08:00 \
    --end-date 2021-06-30T00:00:00-08:00 \
    --duration 4 \
    --cutoff 1
```

```
aws ssm create-maintenance-window \
    --name "My-Cron-Offset-Maintenance-Window" \
    --allow-unassociated-targets \
    --schedule "cron(30 23 ? * TUE#3 *)" \
    --duration 4 \
    --cutoff 1 \
    --schedule-offset 2
```

```
aws ssm create-maintenance-window \
    --name "My-Rate-Maintenance-Window" \
    --allow-unassociated-targets \
    --schedule "rate(7 days)" \
    --duration 4 \
    --schedule-timezone "America/Los_Angeles" \
    --cutoff 1
```

```
aws ssm create-maintenance-window \
    --name "My-TimeStamp-Maintenance-Window" \
    --allow-unassociated-targets \
    --schedule "at(2021-07-07T13:15:30)" \
    --duration 4 \
    --schedule-timezone "America/Los_Angeles" \
    --cutoff 1
```

**More info**  
[CRON expression](https://en.wikipedia.org/wiki/Cron#CRON_expression) at the *Wikipedia website*