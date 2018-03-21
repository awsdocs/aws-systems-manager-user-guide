# Working with Cron and Rate Expressions for Systems Manager<a name="sysman-cron"></a>

When you create a AWS Systems Manager Maintenance Window or a State Manager association, you specify a schedule for when the window or the association should run\. You can specify a schedule in the form of either a time\-based entry, called a *cron expression*, or a frequency\-based entry, called a *rate expression*\.

If you create a Maintenance Window or an association by using the Amazon EC2 console, then you can use tools in the user interface to create your schedule\. If you want to create the Maintenance Window or the association programmatically or from the command line by using, for example, the AWS CLI, then you must specify a schedule parameter with a cron or rate expression in the correct format\.

**Note**  
To create a Maintenance Windows from the AWS CLI, you use the \-\-schedule parameter with a cron or rate expression\. To create a State Manager associations from the AWS CLI, you use the \-\-scheduleExpression parameter with a cron or rate expression\.

Here are some examples that show the schedule parameter with cron and rate expressions:

**Cron example:** This cron expression runs the association at 4 PM \(16:00\) every Tuesday\.

```
--scheduleExpression "cron(0 16 ? * TUE *)"
```

**Rate example:** This rate expression runs the Maintenance Window or the association every other day\.

```
--schedule "rate(2 days)"
```

**Important**  
State Manager associations have limited options for cron and rate expressions compared to Maintenance Windows\. Before you create one of these expressions for an association, review the restrictions in the following section\.

If you are not familiar with cron and rate expressions, we suggest that you read [General Information About Cron and Rate Expressions](#sysman-cron-intro)\.


+ [Cron and Rate Expressions for Associations](#sysman-cron-association)
+ [Cron and Rate Expressions for Maintenance Windows](#sysman-cron-maintenance-window)
+ [General Information About Cron and Rate Expressions](#sysman-cron-intro)

## Cron and Rate Expressions for Associations<a name="sysman-cron-association"></a>

This section includes examples of cron and rate expressions for State Manager associations\. Before you create one of these expressions, be aware of the following restrictions\.

+ Associations only support the following cron expressions: every 1/2, 1, 2, 4, 8, or 12 hours; every day or every week at a specific time\.

+ Associations only support the following rate expressions: intervals of 30 minutes or greater and less than 31 days\.

Here are some cron examples for associations\.


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

## Cron and Rate Expressions for Maintenance Windows<a name="sysman-cron-maintenance-window"></a>

Unlike State Manager associations, Maintenance Windows support all cron and rate expressions\. Here are some cron examples for Maintenance Windows\.


**Cron Examples for Maintenance Windows**  

| Example | Details | 
| --- | --- | 
|  cron\(0 2 ? 1/1 THU\#3 \*\)  |  02:00 AM the third Thursday of every month  | 
|  cron\(15 10 ? \* \* \*\)  |  10:15 AM every day  | 
|  cron\(0 15 10 ? \* MON\-FRI\)  |  10:15 AM every Monday, Tuesday, Wednesday, Thursday and Friday  | 
|  cron\(0 0 2 L \* ?\)  |  02:00 AM on the last day of every month  | 
|  cron\(0 15 10 ? \* 6L\)  |  10:15 AM on the last Friday of every month  | 

Here are some rate examples for Maintenance Windows\.


**Rate Examples for Maintenance Windows**  

| Example | Details | 
| --- | --- | 
|  rate\(30 minutes\)  |  Every 30 minutes  | 
|  rate\(1 hour\)  |  Every hour  | 
|  rate\(5 hours\)  |  Every 5 hours  | 
|  rate\(25 days\)  |  Every 25 days  | 

## General Information About Cron and Rate Expressions<a name="sysman-cron-intro"></a>

Cron expressions for Systems Manager have six required fields\. Fields are separated by a space\.


****  

| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 10 | \* | \* | ? | \* | Run at 10:00 am \(UTC\) every day | 
| 15 | 12 | \* | \* | ? | \* | Run at 12:15 PM \(UTC\) every day | 
| 0 | 18 | ? | \* | MON\-FRI | \* | Run at 6:00 PM \(UTC\) every Monday through Friday | 
| 0 | 8 | 1 | \* | ? | \* | Run at 8:00 AM \(UTC\) every 1st day of the month | 
| 0/15 | \* | \* | \* | ? | \* | Run every 15 minutes | 
| 0/10 | \* | ? | \* | MON\-FRI | \* | Run every 10 minutes Monday through Friday | 
| 0/5 | 8\-17 | ? | \* | MON\-FRI | \* | Run every 5 minutes Monday through Friday between 8:00 AM and 5:55 PM \(UTC\) | 

The following table shows supported values for required cron entries:


****  

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
Cron expressions support the following wildcards:

+ The , \(comma\) wildcard includes additional values\. In the Month field, JAN,FEB,MAR would include January, February, and March\.

+ The \- \(dash\) wildcard specifies ranges\. In the Day field, 1\-15 would include days 1 through 15 of the specified month\.

+ The \* \(asterisk\) wildcard includes all values in the field\. In the Hours field, \* would include every hour\.

+ The / \(forward slash\) wildcard specifies increments\. In the Minutes field, you could enter 1/10 to specify every tenth minute, starting from the first minute of the hour \(for example, the 11th, 21st, and 31st minute, and so on\)\.

+ The ? \(question mark\) wildcard specifies one or another\. In the Day\-of\-month field you could enter 7 and if you didn't care what day of the week the 7th was, you could enter ? in the Day\-of\-week field\.

+ The L wildcard in the Day\-of\-month or Day\-of\-week fields specifies the last day of the month or week\.

+ The W wildcard in the Day\-of\-month field specifies a weekday\. In the Day\-of\-month field, 3W specifies the day closest to the third weekday of the month\.

**Note**  
Cron expressions that lead to rates faster than 5 minute are not supported\. Support for specifying both a day\-of\-week and a day\-of\-month value is not complete\. You must currently use the '?' character in one of these fields\. 

For more information about cron expressions, see [CRON expression](https://en.wikipedia.org/wiki/Cron#CRON_expression) at the *Wikipedia website*\.

**Rate Expressions**  
Rate expressions have the following two required fields\. Fields are separated by white space\.


****  

| Field | Values | 
| --- | --- | 
|  Value  |  positive number  | 
|  Unit  |  minute\(s\) OR hour\(s\) OR day\(s\)  | 

**Note**  
If the value is equal to 1, then the unit must be singular\. Similarly, for values greater than 1, the unit must be plural\. For example, rate\(1 hours\) and rate\(5 hour\) are not valid, but rate\(1 hour\) and rate\(5 hours\) are valid\.