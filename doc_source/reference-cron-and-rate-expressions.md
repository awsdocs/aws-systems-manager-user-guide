# Reference: Cron and Rate Expressions for Systems Manager<a name="reference-cron-and-rate-expressions"></a>

When you create an AWS Systems Manager Maintenance Window or a State Manager association, you specify a schedule for when the window or the association should run\. You can specify a schedule in the form of either a time\-based entry, called a *cron expression*, or a frequency\-based entry, called a *rate expression*\. For Maintenance Windows, you can also specify a time stamp in Coordinated Universal Time \(UTC\) format when you create a Maintenance Window so that it runs once at the specified time\.

When you create either type of resource programmatically or by using a command line tool such as the AWS CLI, you must specify a schedule parameter with a valid cron or rate expression \(or time stamp for Maintenance Windows\) in the correct format\.

When you use the AWS Systems Manager console to create a Maintenance Window or association, you can specify a schedule using a valid cron or rate expression\. You can also use tools in the user interface that simplify the process of creating your schedule\. 

**Maintenance Window Examples**  
To create Maintenance Windows using the AWS CLI, you include the \-\-schedule parameter with a cron or rate expression or a time stamp\. For example: 

```
aws ssm create-maintenance-window --name "My-Cron-Maintenance-Window" --schedule "cron(0 16 ? * TUE *)" --schedule-timezone "America/Los_Angeles" --start-date 2019-01-01T00:00:00-08:00 --end-date 2019-06-30T00:00:00-08:00 --duration 4 --cutoff 1
```

```
aws ssm create-maintenance-window --name "My-Rate-Maintenance-Window" --schedule "rate(7 days)" --duration 4 --schedule-timezone "America/Los_Angeles" --cutoff 1
```

```
aws ssm create-maintenance-window --name "My-TimeStamp-Maintenance-Window" --schedule "at(2019-07-07T13:15:30)" --duration 4 --schedule-timezone "America/Los_Angeles" --cutoff 1
```

**Association Examples**  
To create State Manager associations using the AWS CLI, you include the \-\-schedule\-expression parameter with a cron or rate expression\. For example:

```
aws ssm create-association --association-name "My-Cron-Association" --schedule-expression "cron(0 0 2 ? * SUN *)" --targets Key=tag:ServerRole,Values=WebServer --name AWS-UpdateSSMAgent
```

```
aws ssm create-association --association-name "My-Rate-Association" --schedule-expression "rate(7 days)" --targets Key=tag:ServerRole,Values=WebServer --name AWS-UpdateSSMAgent
```

**Topics**
+ [General Information About Cron and Rate Expressions](#reference-cron-and-rate-expressions-intro)
+ [Cron and Rate Expressions for Associations](#reference-cron-and-rate-expressions-association)
+ [Cron and Rate Expressions for Maintenance Windows](#reference-cron-and-rate-expressions-maintenance-window)

## General Information About Cron and Rate Expressions<a name="reference-cron-and-rate-expressions-intro"></a>

Cron expressions for Systems Manager have six required fields\. A seventh field, the `Seconds` field \(the first in a cron expression\), is optional\. Fields are separated by a space\.


**Cron Expression Examples**  

| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 10 | \* | \* | ? | \* | Run at 10:00 am \(UTC\) every day | 
| 15 | 12 | \* | \* | ? | \* | Run at 12:15 PM \(UTC\) every day | 
| 0 | 18 | ? | \* | MON\-FRI | \* | Run at 6:00 PM \(UTC\) every Monday through Friday | 
| 0 | 8 | 1 | \* | ? | \* | Run at 8:00 AM \(UTC\) every 1st day of the month | 
| 0/15 | \* | \* | \* | ? | \* | Run every 15 minutes | 
| 0/10 | \* | ? | \* | MON\-FRI | \* | Run every 10 minutes Monday through Friday | 
| 0/5 | 8\-17 | ? | \* | MON\-FRI | \* | Run every 5 minutes Monday through Friday between 8:00 AM and 5:55 PM \(UTC\) | 

**Supported Values**  
The following table shows supported values for required cron entries\.

**Note**  
Cron expressions for associations do not support all these values\. For information, see [Cron and Rate Expressions for Associations](#reference-cron-and-rate-expressions-association)\.


**Supported Values for Cron Expressions**  

| Field | Values | Wildcards | 
| --- | --- | --- | 
| Minutes | 0\-59 | , \- \* / | 
| Hours | 0\-23 | , \- \* / | 
| Day\-of\-month | 1\-31 | , \- \* ? / L W | 
| Month | 1\-12 or JAN\-DEC | , \- \* / | 
| Day\-of\-week | 1\-7 or SUN\-SAT | , \- \* ? / L | 
| Year | 1970\-2199 | , \- \* / | 

**Note**  
You cannot specify a value in the Day\-of\-month and in the Day\-of\-week fields in the same cron expression\. If you specify a value in one of the fields, you must use a ? \(question mark\) in the other field\.

**Wildcards**  
The following table shows the wildcard values that cron expressions support\.


**Supported Wildcards for Cron Expressions**  

| Wildcard | Description | 
| --- | --- | 
| , | The , \(comma\) wildcard includes additional values\. In the Month field, JAN,FEB,MAR would include January, February, and March\. | 
| \- | The \- \(dash\) wildcard specifies ranges\. In the Day field, 1\-15 would include days 1 through 15 of the specified month\. | 
| \* | The \* \(asterisk\) wildcard includes all values in the field\. In the Hours field, \* would include every hour\. | 
| / | The / \(forward slash\) wildcard specifies increments\. In the Minutes field, you could enter 1/10 to specify every tenth minute, starting from the first minute of the hour\. So 1/10 specifies the first, 11th, 21st, and 31st minute, and so on\. | 
| ? | The ? \(question mark\) wildcard specifies one or another\. In the Day\-of\-month field you could enter 7 and if you didn't care what day of the week the 7th was, you could enter ? in the Day\-of\-week field\. | 
| L | The L wildcard in the Day\-of\-month or Day\-of\-week fields specifies the last day of the month or week\. | 
| W | The W wildcard in the Day\-of\-month field specifies a weekday\. In the Day\-of\-month field, 3W specifies the day closest to the third weekday of the month\. | 

**Note**  
Cron expressions that lead to rates faster than five \(5\) minute are not supported\. Support for specifying both a day\-of\-week and a day\-of\-month value is not complete\. You must currently use the question mark \(?\) character in one of these fields\. 

For more information about cron expressions, see [CRON expression](https://en.wikipedia.org/wiki/Cron#CRON_expression) at the *Wikipedia website*\.

**Rate Expressions**  
Rate expressions have the following two required fields\. Fields are separated by white space\.


**Required Fields for Rate Expressions**  

| Field | Values | 
| --- | --- | 
|  Value  |  positive number, such as `1` or `15`  | 
|  Unit  |  `minute` `minutes` `hour` `hours` `day` `days`  | 

If the value is equal to `1`, then the unit must be singular\. Similarly, for values greater than `1`, the unit must be plural\. For example, `rate(1 hours)` and `rate(5 hour)` are not valid, but `rate(1 hour)` and `rate(5 hours)` are valid\.

## Cron and Rate Expressions for Associations<a name="reference-cron-and-rate-expressions-association"></a>

This section includes examples of cron and rate expressions for State Manager associations\. Before you create one of these expressions, be aware of the following restrictions\.
+ Associations only support the following cron expressions: every 1/2, 1, 2, 4, 8, or 12 hours; every day or every week at a specific time\.
+ Associations only support the following rate expressions: intervals of 30 minutes or greater and less than 31 days\.
+ If you specify the optional `Seconds` field, its value can only be 0 \(zero\)\. For example: `cron(0 */30 * * * ? *)`

The following table presents cron examples for associations using the required six fields\.


**Cron Examples for Associations**  

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

Here are some rate examples for associations\.


**Rate Examples for Associations**  

| Example | Details | 
| --- | --- | 
|  rate\(30 minutes\)  |  Every 30 minutes  | 
|  rate\(1 hour\)  |  Every hour  | 
|  rate\(5 hours\)  |  Every 5 hours  | 
|  rate\(15 days\)  |  Every 15 days  | 

## Cron and Rate Expressions for Maintenance Windows<a name="reference-cron-and-rate-expressions-maintenance-window"></a>

This section includes examples of cron and rate expressions for Maintenance Windows\.

Unlike State Manager associations, Maintenance Windows support all cron and rate expressions, including values other than 0 \(zero\) in the seconds field\. 

For example, the following 6\-field cron expression runs a Maintenance Window at 9:30 AM every day:

```
cron(30 09 ? * * *)
```

By adding a value to the `Seconds` field, the following 7\-field cron expression runs a Maintenance Window at 9:30:24 AM every day:

```
cron(24 30 09 ? * * *)
```

The following table provides additional 6\-field cron examples for Maintenance Windows\.


**Cron Examples for Maintenance Windows**  

| Example | Details | 
| --- | --- | 
|  cron\(0 2 ? \* THU\#3 \*\)  |  02:00 AM the third Thursday of every month  | 
|  cron\(15 10 ? \* \* \*\)  |  10:15 AM every day  | 
|  cron\(15 10 ? \* MON\-FRI \*\)  |  10:15 AM every Monday, Tuesday, Wednesday, Thursday and Friday  | 
|  cron\(0 2 L \* ? \*\)  |  02:00 AM on the last day of every month  | 
|  cron\(15 10 ? \* 6L \*\)  |  10:15 AM on the last Friday of every month  | 

The following table provides rate examples for Maintenance Windows\.


**Rate Examples for Maintenance Windows**  

| Example | Details | 
| --- | --- | 
|  rate\(30 minutes\)  |  Every 30 minutes  | 
|  rate\(1 hour\)  |  Every hour  | 
|  rate\(5 hours\)  |  Every 5 hours  | 
|  rate\(25 days\)  |  Every 25 days  | 