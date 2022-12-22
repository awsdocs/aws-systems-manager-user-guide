# `aws:branch` – Run conditional automation steps<a name="automation-action-branch"></a>

The `aws:branch` action allows you to create a dynamic automation that evaluates different choices in a single step and then jumps to a different step in the runbook based on the results of that evaluation\. 

When you specify the `aws:branch` action for a step, you specify `Choices` that the automation must evaluate\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the runbook, or a dynamic value generated as the output from the previous step\. The automation evaluates each choice by using a Boolean expression\. If the first choice is true, then the automation jumps to the step designated for that choice\. If the first choice is false, the automation evaluates the next choice\. The automation continues evaluating each choice until it process a true choice\. The automation then jumps to the designated step for the true choice\.

If none of the choices are true, the automation checks to see if the step contains a `default` value\. A default value defines a step that the automation should jump to if none of the choices are true\. If no `default` value is specified for the step, then the automation processes the next step in the runbook\.

The `aws:branch` action supports complex choice evaluations by using a combination of `And`, `Not`, and `Or` operators\. For more information about how to use `aws:branch`, including example runbooks and examples that use different operators, see [Using conditional statements in runbooks](automation-branch-condition.md)\.

**Input**  
Specify one or more `Choices` in a step\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the runbook, or a dynamic value generated as the output from the previous step\. Here is a YAML sample that evaluates a parameter\.

```
mainSteps:
- name: chooseOS
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runWindowsCommand
      Variable: "{{Name of a parameter defined in the Parameters section. For example: OS_name}}"
      StringEquals: windows
    - NextStep: runLinuxCommand
      Variable: "{{Name of a parameter defined in the Parameters section. For example: OS_name}}"
      StringEquals: linux
    Default:
      sleep3
```

Here is a YAML sample that evaluates output from a previous step\.

```
mainSteps:
- name: chooseOS
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{Name of a response object. For example: GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{Name of a response object. For example: GetInstance.platform}}"
      StringEquals: Linux
    Default:
      sleep3
```

Choices  
One or more expressions that the Automation should evaluate when determining the next step to process\. Choices are evaluated by using a Boolean expression\. Each choice must define the following options:  
+ **NextStep**: The next step in the runbook to process if the designated choice is true\.
+ **Variable**: Specify either the name of a parameter that is defined in the `Parameters` section of the runbook\. Or specify an output object from a previous step in the runbook\. For more information about creating variables for `aws:branch`, see [About creating the output variable](automation-branch-condition.md#branch-action-output)\.
+ **Operation**: The criteria used to evaluate the choice\. The `aws:branch` action supports the following operations:

**String operations**
  + StringEquals
  + EqualsIgnoreCase
  + StartsWith
  + EndsWith
  + Contains

**Numeric operations**
  + NumericEquals
  + NumericGreater
  + NumericLesser
  + NumericGreaterOrEquals
  + NumericLesser
  + NumericLesserOrEquals

**Boolean operation**
  + BooleanEquals
**Important**  
When you create a runbook, the system validates each operation in the runbook\. If an operation isn't supported, the system returns an error when you try to create the runbook\.

Default  
The name of a step the automation should jump to if none of the `Choices` are true\.  
Type: String  
Required: No

**Note**  
The `aws:branch` action supports `And`, `Or`, and `Not` operators\. For examples of `aws:branch` that use operators, see [Using conditional statements in runbooks](automation-branch-condition.md)\.