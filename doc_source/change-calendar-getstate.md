# Get the State of the Change Calendar<a name="change-calendar-getstate"></a>

You can get the overall state of the calendar, or the state at a specific time\. You can also show the next time that the calendar state changes from `OPEN` to `CLOSED`, or vice versa\.

You can do this task only by using the `GetCalendarState` API\. The procedure in this section uses the AWS CLI\.
+ Run the following command to show the state of one or more calendar entries at a specific time\. The `--calendar-names` parameter is required, but `--at-time` is optional\.

  ```
  aws ssm get-calendar-state --calendar-names ["Calendar_name_or_document_ARN_1","Calendar_name_or_document_ARN_2"] --at-time "ISO_8601_time_format"
  ```

  The following is an example\.

  ```
  aws ssm get-calendar-state --calendar-names ["arn:aws:ssm:us-east-2:123456789012:document/MyChangeCalendarDocument","arn:aws:ssm:us-east-2:123456789012:document/SupportOffHours"] --at-time "2019-11-25T11:05:14-0700"
  ```

  The results show the state of the calendar \(whether the calendar is of type `DEFAULT_OPEN` or `DEFAULT_CLOSED`\) for the specified calendar entries that are owned by or shared with your account, at the time specified as the value of `--at-time`, and the time of the next transition\. If you do not add the `--at-time` parameter, the current time is used\.