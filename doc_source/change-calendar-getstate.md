# Getting the state of a change calendar<a name="change-calendar-getstate"></a>

You can get the overall state of a calendar or the state of a calendar at a specific time in Change Calendar, a capability in AWS Systems Manager\. You can also show the next time that the calendar state changes from `OPEN` to `CLOSED`, or vice versa\.

You can do this task only by using the `GetCalendarState` API operation\. The procedure in this section uses the AWS Command Line Interface \(AWS CLI\)\.

**To get the state of a change calendar**
+ Run the following command to show the state of one or more calendars at a specific time\. The `--calendar-names` parameter is required, but `--at-time` is optional\.

------
#### [ Linux & macOS ]

  ```
  aws ssm get-calendar-state \
      --calendar-names "Calendar_name_or_document_ARN_1" "Calendar_name_or_document_ARN_2" \
      --at-time "ISO_8601_time_format"
  ```

  The following is an example\.

  ```
  aws ssm get-calendar-state \
      --calendar-names "arn:aws:ssm:us-east-2:123456789012:document/MyChangeCalendarDocument" "arn:aws:ssm:us-east-2:123456789012:document/SupportOffHours" \
      --at-time "2020-07-30T11:05:14-0700"
  ```

------
#### [ Windows ]

  ```
  aws ssm get-calendar-state ^
      --calendar-names "Calendar_name_or_document_ARN_1" "Calendar_name_or_document_ARN_2" ^
      --at-time "ISO_8601_time_format"
  ```

  The following is an example\.

  ```
  aws ssm get-calendar-state ^
      --calendar-names "arn:aws:ssm:us-east-2:123456789012:document/MyChangeCalendarDocument" "arn:aws:ssm:us-east-2:123456789012:document/SupportOffHours" ^
      --at-time "2020-07-30T11:05:14-0700"
  ```

------

  The command returns information like the following\.

  ```
  {
      "State": "OPEN",
      "AtTime": "2020-07-30T16:18:18Z",
      "NextTransitionTime": "2020-07-31T00:00:00Z"
  }
  ```

  The results show the state of the calendar \(whether the calendar is of type `DEFAULT_OPEN` or `DEFAULT_CLOSED`\) for the specified calendar entries that are owned by or shared with your account, at the time specified as the value of `--at-time`, and the time of the next transition\. If you don't add the `--at-time` parameter, the current time is used\.
**Note**  
If you specify more than one calendar in a request, the command returns the status of `OPEN` only if all calendars in the request are open\. If one or more calendars in the request are closed, the status returned is `CLOSED`\.