# Customizing the display and using filters<a name="Explorer-using-filters"></a>

You can customize widget layout in Systems Manager Explorer by using a drag\-and\-drop capability\. You can also customize the OpsData and OpsItems displayed in Explorer by using filters, as described in this topic\. 

## Customizing widget layout<a name="Explorer-using-filters-customize"></a>

Use the following procedure to customize widget layout in Explorer\.

**To customize widget layout**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose a widget that you want to move\.

1. Click and hold the name of the widget and then drag it to its new location\.  
![\[Moving a widget in Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-customize.png)

1. Repeat this process for each widget that you want to reposition\.

If you decide that you don't like the new layout, choose **Reset layout** to move all widgets back to their original location\.

## Using filters to the change the data displayed in Explorer<a name="Explorer-using-filters-filtering"></a>

By default, Explorer displays data for the current AWS account and the current Region\. If you create one or more resource data syncs, you can use filters to change which sync is active\. You can then choose to display data for a specific Region or all Regions\. You can also use the Search bar to filter on different OpsItem and key\-tag criteria\.

**To change the data displayed in Explorer by using filters**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. In the **Filter** section, use the **Select a resource data sync** list to choose a sync\.

1. Use the **Regions** list to choose either a specific AWS Region or choose **All Regions**\.

1. Choose the Search bar, and then choose the criteria on which to filter the data\.  
![\[Using the filters Search bar in Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-filters.png)

1. Press Enter\.

Explorer retains the filter options you selected if you close and reopen the page\.