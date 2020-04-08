# AWS Systems Manager Change Calendar<a name="systems-manager-change-calendar"></a>

Systems Manager Change Calendar lets you set up date and time ranges when actions you specify \(for example, in [Systems Manager Automation](systems-manager-automation.md) documents\) may or may not be performed in your AWS account\. In Change Calendar, these ranges are called *events*\. When you create a Change Calendar entry, you are creating a [Systems Manager document](sysman-ssm-docs.md) of the type `ChangeCalendar`\. In Change Calendar, the document stores [iCalendar 2\.0](https://icalendar.org/) data in plaintext format\. Events that you add to the Change Calendar entry become part of the document\.

A Change Calendar entry can be one of two types:

**`DEFAULT_OPEN`**, or Open by default  
When a calendar entry is open by default, actions can run by default, but are blocked from running during associated events\. During events, the state of a `DEFAULT_OPEN` calendar is `CLOSED`\.

**`DEFAULT_CLOSED`**, or Closed by default  
When a calendar entry is closed by default, actions that are tracking Change Calendar do not run by default, but can run during events associated with the calendar entry\. During events, the state of a `DEFAULT_CLOSED` calendar is `OPEN`\.

## Who should use Change Calendar?<a name="systems-manager-change-calendar-who"></a>
+ Any AWS customer who creates or runs Systems Manager Automation documents\.
+ Administrators who are responsible for keeping the configurations of AWS Systems Manager managed instances consistent, stable, and functional\.

## Benefits of Change Calendar<a name="systems-manager-change-calendar-benefits"></a>

The following are some benefits of Systems Manager Change Calendar\.
+ **Review changes before they're applied**

  A Change Calendar entry can help ensure that potentially destructive Automation changes to your environment are reviewed before they're applied\.
+ **Apply changes only during appropriate times**

  Change Calendar entries help keep your environment stable during event times\. For example, you can create a Change Calendar entry to block changes when you expect high demand on your resources, such as during a conference or public marketing promotion\. A calendar entry can also block changes when you expect limited administrator support, such as during vacations or holidays\. You can use a calendar entry to allow changes except for certain times of the day or week when there is limited administrator support to troubleshoot failed actions or deployments\.
+ **Get the current or upcoming state of the calendar**

  You can run the Systems Manager `GetCalendarState` API operation to show you the current state of the calendar, the state at a specified time, or the next time that the calendar state is scheduled to change\.

**Topics**
+ [Who should use Change Calendar?](#systems-manager-change-calendar-who)
+ [Benefits of Change Calendar](#systems-manager-change-calendar-benefits)
+ [Getting started with Change Calendar](systems-manager-change-calendar-prereqs.md)
+ [Working with Change Calendar](systems-manager-change-calendar-working.md)
+ [Add Change Calendar dependencies to Automation documents](systems-manager-change-calendar-automations.md)