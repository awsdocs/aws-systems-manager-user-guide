# aws:branch – Run conditional automation steps<a name="automation-action-branch"></a>

The `aws:branch` action enables you to create a dynamic Automation workflow that evaluates different choices in a single step and then jumps to a different step in the Automation document based on the results of that evaluation\. 

When you specify the `aws:branch` action for a step, you specify `Choices` that the workflow must evaluate\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the Automation document, or a dynamic value generated as the output from the previous step\. The Automation workflow evaluates each choice by using a Boolean expression\. If the first choice is true, then the workflow jumps to the step designated for that choice\. If the first choice is false, the workflow evaluates the next choice\. The workflow continues evaluating each choice until it process a true choice\. The workflow then jumps to the designated step for the true choice\.

If none of the choices are true, the workflow checks to see if the step contains a `default` value\. A default value defines a step that the workflow should jump to if none of the choices are true\. If no `default` value is specified for the step, then the Automation workflow processes the next step in the document\.

The `aws:branch` action supports complex choice evaluations by using a combination of `And`, `Not`, and `Or` operators\. For more information about how to use `aws:branch`, including example documents and examples that use different operators, see [Creating dynamic Automation workflows with conditional branching](automation-branchdocs.md)\.

**Input**  
Specify one or more `Choices` in a step\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the Automation document, or a dynamic value generated as the output from the previous step\. Here is a YAML sample that evaluates a parameter\.

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
+ **NextStep**: The next step in the Automation document to process if the designated choice is true\.
+ **Variable**: Specify either the name of a parameter that is defined in the `Parameters` section of the Automation document\. Or specify an output object from a previous step in the Automation document\. For more information about creating variables for `aws:branch`, see [About creating the output variable](automation-branchdocs.md#automation-branchdocs-awsbranch-creating-output)\.
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
When you create an Automation document, the system validates each operation in the document\. If an operation is not supported, the system returns an error when you try to create the document\.

Default  
The name of a step the workflow should jump to if none of the `Choices` are true\.  
Type: String  
Required: No

**Note**  
The `aws:branch` action supports `And`, `Or`, and `Not` operators\. For examples of `aws:branch` that use operators, see [Creating dynamic Automation workflows with conditional branching](automation-branchdocs.md)\.