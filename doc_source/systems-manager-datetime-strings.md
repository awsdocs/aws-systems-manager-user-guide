# Reference: Creating formatted date and time strings for Systems Manager<a name="systems-manager-datetime-strings"></a>

Many AWS Systems Manager API actions accept filters to limit the number of results returned by a request\. Some of these API actions accept filters that require a formatted string to represent a specific date and time\. For example, the `DescribeSessions` API action accepts the `InvokedAfter` and `InvokedBefore` keys as some of the valid values for a `SessionFilter` object\. Another example is the `DescribeAutomationExecutions` API action, which accepts the `StartTimeBefore` and `StartTimeAfter` keys as some of the valid values for an `AutomationExecutionFilter` object\. The values you provide for these keys when filtering your requests must match the ISO 8601 standard\. For information about ISO 8601, see [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)\.

These formatted date and time strings are not limited to filters\. There are also API actions that require an ISO 8601 formatted string to represent a specific date and time when providing a value for a request parameter\. For example, the `AtTime` request parameter for the `GetCalendarState` action\. These strings are difficult to create\. Use the examples in this topic to create formatted date and time strings to use with Systems Manager API actions\.

## Formatting date and time strings for Systems Manager<a name="systems-manager-datetime-strings-format"></a>

The following is an example of an ISO 8601 formatted date and time string\.

```
2020-05-08T15:16:43Z
```

This represents May 8th, 2020 at 15:16 Universal Coordinated Time \(UTC\)\. The calendar date portion of the string is represented by a four\-digit year, two\-digit month, and two\-digit day separated by hyphens\. This can be represented in the following format\.

```
YYYY-MM-DD
```

The time portion of the string begins with the letter "T" as a delimiter, and then is represented by a two\-digit hour, two\-digit minute, and two\-digit second separated by colons\. This can be represented in the following format\.

```
hh:mm:ss
```

The time portion of the string ends with the letter "Z", denoting the UTC standard\.

## Creating custom date and time strings for Systems Manager<a name="systems-manager-datetime-strings-custom"></a>

You can create custom date and time strings from your local machine using your preferred command line tool\. The syntax you use to create an ISO 8601 formatted date and time string differs depending on your local machine's operating system\. The following are examples of how you can use `date` from GNU's coreutils on Linux, or PowerShell on Windows to create an ISO 8601 formatted date and time string\.

------
#### [ coreutils ]

```
date '+%Y-%m-%dT%H:%M:%SZ'
```

------
#### [ PowerShell ]

```
(Get-Date).ToString("yyyy-MM-ddTH:mm:ssZ")
```

------

When working with Systems Manager API actions, you might need to create historical date and time strings for reporting or troubleshooting purposes\. The following are examples of how you can create and use custom historical ISO 8601 formatted date and time strings for the AWS Tools for PowerShell and AWS CLI\.

------
#### [ AWS CLI ]
+ Retrieve the last week of command history for an SSM document\.

  ```
  lastWeekStamp=$(date '+%Y-%m-%dT%H:%M:%SZ' -d '7 days ago')
  
  docFilter='{"key":"DocumentName","value":"AWS-RunPatchBaseline"}'
  timeFilter='{"key":"InvokedAfter","value":'\"$lastWeekStamp\"'}'
  
  commandFilters=[$docFilter,$timeFilter]
  
  aws ssm list-commands \
      --filters $commandFilters
  ```
+ Retrieve the last week of automation execution history\.

  ```
  lastWeekStamp=$(date '+%Y-%m-%dT%H:%M:%SZ' -d '7 days ago')
  
  aws ssm describe-automation-executions \
      --filters Key=StartTimeAfter,Values=$lastWeekStamp
  ```
+ Retrieve the last month of session history\.

  ```
  lastWeekStamp=$(date '+%Y-%m-%dT%H:%M:%SZ' -d '30 days ago')
  
  aws ssm describe-sessions \
      --state History \
      --filters key=InvokedAfter,value=$lastWeekStamp
  ```

------
#### [ AWS Tools for PowerShell ]
+ Retrieve the last week of command history for an SSM document\.

  ```
  $lastWeekStamp = (Get-Date).AddDays(-7).ToString("yyyy-MM-ddTH:mm:ssZ")
  
  $docFilter = @{
      Key="DocumentName"
      Value="AWS-InstallWindowsUpdates"
  }
  $timeFilter = @{
      Key="InvokedAfter"
      Value=$lastWeekStamp
  }
  
  $commandFilters = $docFilter,$timeFilter
  
  Get-SSMCommand `
      -Filters $commandFilters
  ```
+ Retrieve the last week of automation execution history\.

  ```
  $lastWeekStamp = (Get-Date).AddDays(-7).ToString("yyyy-MM-ddTH:mm:ssZ")
  
  Get-SSMAutomationExecutionList `
      -Filters @{Key="StartTimeAfter";Values=$lastWeekStamp}
  ```
+ Retrieve the last month of session history\.

  ```
  $lastWeekStamp = (Get-Date).AddDays(-30).ToString("yyyy-MM-ddTH:mm:ssZ")
  
  Get-SSMSession `
      -State History `
      -Filters @{Key="InvokedAfter";Value=$lastWeekStamp}
  ```

------