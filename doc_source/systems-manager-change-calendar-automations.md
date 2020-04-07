# Add Change Calendar Dependencies to Automation Documents<a name="systems-manager-change-calendar-automations"></a>

To make Automation actions adhere to Systems Manager Change Calendar, add a step in an Automation document that uses the [`aws:assertAwsResourceProperty`](automation-action-assertAwsResourceProperty.md) action\. Configure the action to run `GetCalendarState` to verify that a specified calendar entry is in the state that you want \(`OPEN` or `CLOSED`\)\. The Automation document is only allowed to continue to the next step if the calendar state is `OPEN`\. The following is a YAML\-based sample excerpt of an Automation document that cannot advance to the next step, `LaunchInstance`, unless the calendar state matches `OPEN`, the state specified in `DesiredValues`\.

```
mainSteps:
...
    name: MyCheckCalendarStateStep
    action: 'aws:assertAwsResourceProperty'
    inputs:
    Service: ssm
    Api: GetCalendarState
    PropertySelector: CalendarState
    DesiredValues:
    - OPEN
    CalendarNameOrARN: 'arn:aws:ssm:us-east-1:123456789012:document/SaleDays'
    description: Use GetCalendarState to determine whether a calendar is open or closed.
    nextStep: LaunchInstance
    name: LaunchInstance
    action: 'aws:executeScript'
    inputs:
    Runtime: python3.6 
...
```