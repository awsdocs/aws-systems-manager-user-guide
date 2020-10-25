# Configuration Compliance walkthrough \(AWS CLI\)<a name="sysman-compliance-walk"></a>

The following procedure walks you through the process of using the [PutComplianceItems](https://docs.aws.amazon.com/ssm/latest/APIReference/API_PutComplianceItems.html) API action to assign custom compliance metadata to a resource\. You can also use this API action to manually assign patch or association compliance metadata to an instance, as shown in the following walkthrough\. For more information about custom compliance, see [About custom compliance](sysman-compliance-about.md#sysman-compliance-custom)\.

**To assign custom compliance metadata to a managed instance \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to assign custom compliance metadata to an instance\. Currently the only supported resource type is `ManagedInstance`\.

------
#### [ Linux ]

   ```
   aws ssm put-compliance-items \
       --resource-id instance_ID \
       --resource-type ManagedInstance \
       --compliance-type Custom:user-defined_string \
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value \
       --items Id=user-defined_ID,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT
   ```

------
#### [ Windows ]

   ```
   aws ssm put-compliance-items ^
       --resource-id instance_ID ^
       --resource-type ManagedInstance ^
       --compliance-type Custom:user-defined_string ^
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value ^
       --items Id=user-defined_ID,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT
   ```

------

1. Repeat the previous step to assign additional custom compliance metadata to one or more instances\. You can also manually assign patch or association compliance metadata to managed instances by using the following commands:

   Association compliance metadata

------
#### [ Linux ]

   ```
   aws ssm put-compliance-items \
       --resource-id instance_ID \
       --resource-type ManagedInstance \
       --compliance-type Association \
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value \
       --items Id=user-defined_ID,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT
   ```

------
#### [ Windows ]

   ```
   aws ssm put-compliance-items ^
       --resource-id instance_ID ^
       --resource-type ManagedInstance ^
       --compliance-type Association ^
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value ^
       --items Id=user-defined_ID,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT
   ```

------

   Patch compliance metadata

------
#### [ Linux ]

   ```
   aws ssm put-compliance-items \
       --resource-id instance_ID \
       --resource-type ManagedInstance \
       --compliance-type Patch \
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value,ExecutionId=user-defined_ID,ExecutionType=Command  \
       --items Id=for_example, KB12345,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT,Details="{PatchGroup=name_of_group,PatchSeverity=the_patch_severity, for example, CRITICAL}"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-compliance-items ^
       --resource-id instance_ID ^
       --resource-type ManagedInstance ^
       --compliance-type Patch ^
       --execution-summary ExecutionTime=user-defined_time_and/or_date_value,ExecutionId=user-defined_ID,ExecutionType=Command  ^
       --items Id=for_example, KB12345,Title=user-defined_title,Severity=one_or_more_comma-separated_severities:CRITICAL, MAJOR, MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT,Details="{PatchGroup=name_of_group,PatchSeverity=the_patch_severity, for example, CRITICAL}"
   ```

------

1. Run the following command to view a list of compliance items for a specific managed instance\. Use filters to drill down into specific compliance data\.

------
#### [ Linux ]

   ```
   aws ssm list-compliance-items \
       --resource-ids instance_ID \
       --resource-types ManagedInstance \
       --filters one_or_more_filters
   ```

------
#### [ Windows ]

   ```
   aws ssm list-compliance-items ^
       --resource-ids instance_ID ^
       --resource-types ManagedInstance ^
       --filters one_or_more_filters
   ```

------

   The following examples show you how to use this command with filters\.

------
#### [ Linux ]

   ```
   aws ssm list-compliance-items \
       --resource-ids i-02573cafcfEXAMPLE \
       --resource-type ManagedInstance \
       --filters Key=DocumentName,Values=AWS-RunPowerShellScript Key=Status,Values=NON_COMPLIANT,Type=NotEqual Key=Id,Values=cee20ae7-6388-488e-8be1-a88ccEXAMPLE Key=Severity,Values=UNSPECIFIED
   ```

------
#### [ Windows ]

   ```
   aws ssm list-compliance-items ^
       --resource-ids i-02573cafcfEXAMPLE ^
       --resource-type ManagedInstance ^
       --filters Key=DocumentName,Values=AWS-RunPowerShellScript Key=Status,Values=NON_COMPLIANT,Type=NotEqual Key=Id,Values=cee20ae7-6388-488e-8be1-a88ccEXAMPLE Key=Severity,Values=UNSPECIFIED
   ```

------

------
#### [ Linux ]

   ```
   aws ssm list-resource-compliance-summaries \
       --filters Key=OverallSeverity,Values=UNSPECIFIED
   ```

------
#### [ Windows ]

   ```
   aws ssm list-resource-compliance-summaries ^
       --filters Key=OverallSeverity,Values=UNSPECIFIED
   ```

------

------
#### [ Linux ]

   ```
   aws ssm list-resource-compliance-summaries \
       --filters Key=OverallSeverity,Values=UNSPECIFIED Key=ComplianceType,Values=Association Key=InstanceId,Values=i-02573cafcfEXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm list-resource-compliance-summaries ^
       --filters Key=OverallSeverity,Values=UNSPECIFIED Key=ComplianceType,Values=Association Key=InstanceId,Values=i-02573cafcfEXAMPLE
   ```

------

1. Run the following command to view a summary of compliance statuses\. Use filters to drill down into specific compliance data\.

   ```
   aws ssm list-resource-compliance-summaries --filters One or more filters.
   ```

   The following examples show you how to use this command with filters\.

------
#### [ Linux ]

   ```
   aws ssm list-resource-compliance-summaries \
       --filters Key=ExecutionType,Values=Command
   ```

------
#### [ Windows ]

   ```
   aws ssm list-resource-compliance-summaries ^
       --filters Key=ExecutionType,Values=Command
   ```

------

------
#### [ Linux ]

   ```
   aws ssm list-resource-compliance-summaries \
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=OverallSeverity,Values=CRITICAL
   ```

------
#### [ Windows ]

   ```
   aws ssm list-resource-compliance-summaries ^
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=OverallSeverity,Values=CRITICAL
   ```

------

1. Run the following command to view a summary count of compliant and non\-compliant resources for a compliance type\. Use filters to drill down into specific compliance data\.

   ```
   aws ssm list-compliance-summaries --filters One or more filters.
   ```

   The following examples show you how to use this command with filters\.

------
#### [ Linux ]

   ```
   aws ssm list-compliance-summaries \
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=PatchGroup,Values=TestGroup
   ```

------
#### [ Windows ]

   ```
   aws ssm list-compliance-summaries ^
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=PatchGroup,Values=TestGroup
   ```

------

------
#### [ Linux ]

   ```
   aws ssm list-compliance-summaries \
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=ExecutionId,Values=4adf0526-6aed-4694-97a5-14522EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm list-compliance-summaries ^
       --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=ExecutionId,Values=4adf0526-6aed-4694-97a5-14522EXAMPLE
   ```

------