# Viewing aggregated counts of change requests \(command line\)<a name="change-requests-review-aggregate-command-line"></a>

You can view aggregated counts of change requests by using the [GetOpsSummary](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetOpsSummary.html) API action\. This API action can return counts for a single AWS account in a single Region or for multiple accounts and multiple Regions\.

**Note**  
If you want to view aggregated counts of change requests for multiple AWS accounts and multiple Regions, you must set up and configure a resource data sync\. For more information, see [Configuring resource data sync for Inventory](sysman-inventory-datasync.md)\.

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) to view aggregated counts of change requests\. 

**To view aggregated counts of change requests**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run one of the following commands\. 

   **Single account and Region**

   This command returns a count of all change requests for the AWS account and Region for which your AWS CLI session is currently configured\.

------
#### [ Linux ]

   ```
   aws ssm get-ops-summary \
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal \
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------
#### [ Windows ]

   ```
   aws ssm get-ops-summary ^
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal ^
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------

   The call returns information like the following\.

   ```
   {
       "Entities": [
           {
               "Data": {
                   "AWS:OpsItem": {
                       "Content": [
                           {
                               "Count": "38",
                               "Status": "Open"
                           }
                       ]
                   }
               }
           }
       ]
   }
   ```

   **Multiple accounts and/or Regions**

   This command returns a count of all change requests for the AWS accounts and Regions specified in the resource data sync\.

------
#### [ Linux ]

   ```
   aws ssm get-ops-summary \
   --sync-name resource_data_sync_name \
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal \
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------
#### [ Windows ]

   ```
   aws ssm get-ops-summary ^
   --sync-name resource_data_sync_name ^
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal ^
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------

   The call returns information like the following\.

   ```
   {
       "Entities": [
           {
               "Data": {
                   "AWS:OpsItem": {
                       "Content": [
                           {
                               "Count": "43",
                               "Status": "Open"
                           },
                           {
                               "Count": "2",
                               "Status": "Resolved"
                           }
                       ]
                   }
               }
           }
       ]
   }
   ```

   **Multiple accounts and a specific Region**

   This command returns a count of all change requests for the AWS accounts specified in the resource data sync\. However, it only returns data from the Region specified in the command\.

------
#### [ Linux ]

   ```
   aws ssm get-ops-summary \
   --sync-name resource_data_sync_name \
   --filters Key=AWS:OpsItem.SourceRegion,Values='Region',Type=Equal Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal \
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------
#### [ Windows ]

   ```
   aws ssm get-ops-summary ^
   --sync-name resource_data_sync_name ^
   --filters Key=AWS:OpsItem.SourceRegion,Values='Region',Type=Equal Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal ^
   --aggregators AggregatorType=count,AttributeName=Status,TypeName=AWS:OpsItem
   ```

------

   **Multiple accounts and Regions with output grouped by Region**

   This command returns a count of all change requests for the AWS accounts and Regions specified in the resource data sync\. The output displays count information per Region\.

------
#### [ Linux ]

   ```
   aws ssm get-ops-summary \
   --sync-name resource_data_sync_name \
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal \
   --aggregators '[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"Status","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceRegion"}]}]'
   ```

------
#### [ Windows ]

   ```
   aws ssm get-ops-summary ^
   --sync-name resource_data_sync_name ^
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal ^
   --aggregators '[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"Status","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceRegion"}]}]'
   ```

------

   The call returns information like the following\.

   ```
   {
           "Entities": [
               {
                   "Data": {
                       "AWS:OpsItem": {
                           "Content": [
                               {
                                   "Count": "38",
                                   "SourceRegion": "us-east-1",
                                   "Status": "Open"
                               },
                               {
                                   "Count": "4",
                                   "SourceRegion": "us-east-2",
                                   "Status": "Open"
                               },
                               {
                                   "Count": "1",
                                   "SourceRegion": "us-west-1",
                                   "Status": "Open"
                               },
                               {
                                   "Count": "2",
                                   "SourceRegion": "us-east-2",
                                   "Status": "Resolved"
                               }
                           ]
                       }
                   }
               }
           ]
       }
   ```

   **Multiple accounts and Regions with output grouped by accounts and Regions**

   This command returns a count of all change requests for the AWS accounts and Regions specified in the resource data sync\. The output groups the count information by accounts and Regions\.

------
#### [ Linux ]

   ```
   aws ssm get-ops-summary \
   --sync-name resource_data_sync_name \
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal \
   --aggregators '[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"Status","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceAccountId","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceRegion"}]}]}]'
   ```

------
#### [ Windows ]

   ```
   aws ssm get-ops-summary ^
   --sync-name resource_data_sync_name ^
   --filters Key=AWS:OpsItem.OpsItemType,Values="/aws/changerequests",Type=Equal ^
   --aggregators '[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"Status","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceAccountId","Aggregators":[{"AggregatorType":"count","TypeName":"AWS:OpsItem","AttributeName":"SourceRegion"}]}]}]'
   ```

------

   The call returns information like the following\.

   ```
   {
       "Entities": [
           {
               "Data": {
                   "AWS:OpsItem": {
                       "Content": [
                           {
                               "Count": "38",
                               "SourceAccountId": "123456789012",
                               "SourceRegion": "us-east-1",
                               "Status": "Open"
                           },
                           {
                               "Count": "4",
                               "SourceAccountId": "111122223333",
                               "SourceRegion": "us-east-2",
                               "Status": "Open"
                           },
                           {
                               "Count": "1",
                               "SourceAccountId": "111122223333",
                               "SourceRegion": "us-west-1",
                               "Status": "Open"
                           },
                           {
                               "Count": "2",
                               "SourceAccountId": "444455556666",
                               "SourceRegion": "us-east-2",
                               "Status": "Resolved"
                           },
                           {
                               "Count": "1",
                               "SourceAccountId": "222222222222",
                               "SourceRegion": "us-east-1",
                               "Status": "Open"
                           }
                       ]
                   }
               }
           }
       ]
   }
   ```