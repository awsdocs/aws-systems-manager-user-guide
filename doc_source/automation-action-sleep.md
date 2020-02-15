# aws:sleep – Delay an automation execution<a name="automation-action-sleep"></a>

Delays Automation execution for a specified amount of time\. This action uses the International Organization for Standardization \(ISO\) 8601 date and time format\. For more information about this date and time format, see [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)\.

**Input**  
You can delay execution for a specified duration\. 

------
#### [ YAML ]

```
name: sleep
action: aws:sleep
inputs:
  Duration: PT10M
```

------
#### [ JSON ]

```
{
   "name":"sleep",
   "action":"aws:sleep",
   "inputs":{
      "Duration":"PT10M"
   }
}
```

------

You can also delay execution until a specified date and time\. If the specified date and time has passed, the action proceeds immediately\. 

------
#### [ YAML ]

```
name: sleep
action: aws:sleep
inputs:
  Timestamp: '2020-01-01T01:00:00Z'
```

------
#### [ JSON ]

```
{
    "name": "sleep",
    "action": "aws:sleep",
    "inputs": {
        "Timestamp": "2020-01-01T01:00:00Z"
    }
}
```

------

**Note**  
Automation currently supports a maximum delay of 604800 seconds \(7 days\)\.

Duration  
An ISO 8601 duration\. You can't specify a negative duration\.   
Type: String  
Required: No

Timestamp  
An ISO 8601 timestamp\. If you don't specify a value for this parameter, then you must specify a value for the `Duration` parameter\.   
Type: String  
Required: NoOutput

None  