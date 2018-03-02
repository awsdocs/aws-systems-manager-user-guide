# Systems Manager Configuration Compliance Manager Walkthrough \(AWS CLI\)<a name="sysman-compliance-walk"></a>

The following procedure walks you through the process of using the [PutComplianceItems](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutComplianceItems.html) API action to assign custom compliance metadata to a resource\. You can also use this API action to manually assign patch or association compliance metadata to an instance, as shown in the following walkthrough\. For more information about custom compliance, see [About Custom Compliance](sysman-compliance-about.md#sysman-compliance-custom)\.

**To assign custom compliance metadata to a managed instance**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to assign custom compliance metadate to an instance\. Currently the only supported resource type is `ManagedInstance`\.

   ```
   aws ssm put-compliance-items --resource-id Instance ID --resource-type ManagedInstance --compliance-type Custom:User-defined string --execution-summary ExecutionTime=User-defined time and/or date value --items Id=User-defined ID,Title=User-defined title,Severity=One or more comma-separated severities:CRITICAL,MAJOR,MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANt
   ```

1. Repeat the previous step to assign additional custom compliance metadata to one or more instances\. You can also manually assign patch or association compliance metadata to managed instances by using the following commands:

   Association compliance metadata

   ```
   aws ssm put-compliance-items --resource-id Instance ID --resource-type ManagedInstance --compliance-type Association --execution-summary ExecutionTime=User-defined time and/or date value --items Id=User-defined ID,Title=User-defined title,Severity=One or more comma-separated severities:CRITICAL,MAJOR,MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT,Details="{DocumentName=The SSM document for the association,DocumentVersion=A version number}"
   ```

   Patch compliance metadata

   ```
   aws ssm put-compliance-items --resource-id Instance ID --resource-type ManagedInstance --compliance-type Patch --execution-summary ExecutionTime=User-defined time and/or date value,ExecutionId=User-defined ID,ExecutionType=Command --items Id=for example, KB12345,Title=User-defined title,Severity=One or more comma-separated severities:CRITICAL,MAJOR,MINOR,INFORMATIONAL, or UNSPECIFIED,Status=COMPLIANT or NON_COMPLIANT,Details="{PatchGroup=Name of group,PatchSeverity=The patch severity, for example, CRITICAL}"
   ```

1. Execute the following command to view a list of compliance items for a specific managed instance\. Use filters to drill\-down into specific compliance data\.

   ```
   aws ssm list-compliance-items --resource-ids Instance ID --resource-types ManagedInstance --filters One or more filters.
   ```

   The following examples show you how to use this command with filters\.

   ```
   aws ssm list-compliance-items --resource-ids i-1234567890abcdef0 --resource-type ManagedInstance --filters Key=DocumentName,Values=AWS-RunPowerShellScript Key=Status,Values=NON_COMPLIANT,Type=NotEqual Key=Id,Values=cee20ae7-6388-488e-8be1-a88cc6c46dcc Key=Severity,Values=UNSPECIFIED
   ```

   ```
   aws ssm list-resource-compliance-summaries --filters Key=OverallSeverity,Values=UNSPECIFIED
   ```

   ```
   aws ssm list-resource-compliance-summaries --filters Key=OverallSeverity,Values=UNSPECIFIED Key=ComplianceType,Values=Association Key=InstanceId,Values=i-1234567890abcdef0 
   ```

1. Execute the following command to view a summary of compliance statuses\. Use filters to drill\-down into specific compliance data\.

   ```
   aws ssm list-resource-compliance-summaries --filters One or more filters.
   ```

   The following examples show you how to use this command with filters\.

   ```
   aws ssm list-resource-compliance-summaries --filters Key=ExecutionType,Values=Command
   ```

   ```
   aws ssm list-resource-compliance-summaries --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=OverallSeverity,Values=CRITICAL
   ```

1. Execute the following command to view a summary count of compliant and non\-compliant resources for a compliance type\. Use filters to drill\-down into specific compliance data\.

   ```
   aws ssm list-compliance-summaries --filters One or more filters.
   ```

   The following examples show you how to use this command with filters\.

   ```
   aws ssm list-compliance-summaries --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=PatchGroup,Values=TestGroup
   ```

   ```
   aws ssm list-compliance-summaries --filters Key=AWS:InstanceInformation.PlatformType,Values=Windows Key=ExecutionId,Values=4adf0526-6aed-4694-97a5-145222f4c2b6
   ```