# Handling timeouts in Automation documents<a name="automation-handling-timeouts"></a>

The `timeoutSeconds` property is shared by all Automation actions\. You can use this property to specify the execution timeout value for an action\. Further, you can change how an action timing out affects the Automation workflow and overall execution status\. You can accomplish this by also defining the `onFailure` and `isCritical` shared properties for an action\.

For example, depending on your use case, you might want your Automation to continue to a different action and not affect the overall status of the Automation if an action times out\. In this example, you specify the length of time to wait before the action times out using the `timeoutSeconds` property\. Then you specify the action, or step, the Automation should go to if there is a timeout\. Specify a value using the format `step:step_name` for the `onFailure` property rather than the default value of `Abort`\. By default, if an action times out, the Automation execution status will be `Timed Out`\. To prevent a timeout from affecting the Automation execution status, specify `false` for the `isCritical` property\.

The following example shows how to define the shared properties for an action described in this scenario\.

------
#### [ YAML ]

```
- name: verifyImageAvailability
  action: 'aws:waitForAwsResourceProperty'
  timeoutSeconds: 600
  isCritical: false
  onFailure: 'step:getCurrentImageState'
  inputs:
    Service: ec2
    Api: DescribeImages
    ImageIds:
      - '{{ createImage.newImageId }}'
    PropertySelector: '$.Images[0].State'
    DesiredValues:
      - available
  nextStep: copyImage
```

------
#### [ JSON ]

```
{
    "name": "verifyImageAvailability",
    "action": "aws:waitForAwsResourceProperty",
    "timeoutSeconds": 600,
    "isCritical": false,
    "onFailure": "step:getCurrentImageState",
    "inputs": {
        "Service": "ec2",
        "Api": "DescribeImages",
        "ImageIds": [
            "{{ createImage.newImageId }}"
        ],
        "PropertySelector": "$.Images[0].State",
        "DesiredValues": [
            "available"
        ]
    },
    "nextStep": "copyImage"
}
```

------

For more information about properties shared by all Automation actions, see [Properties shared by all actions](automation-actions.md#automation-common)\.