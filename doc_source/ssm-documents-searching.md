# Searching for SSM documents<a name="ssm-documents-searching"></a>

You can search the AWS Systems Manager document store for SSM documents by using either free text search or a filter\-based search\. This section describes how to use both capabilities to find an SSM document\. 

## Using free text search<a name="ssm-documents-searching-free-text"></a>

The search box on the Systems Manager **Documents** page supports free text search\. Free text search compares the search term or terms that you enter against the document name in each SSM document\. If you enter a single search term, for example **ansible**, then Systems Manager returns all SSM documents where this term was discovered\. If you enter multiple search terms, then Systems Manager searches by using an `OR` statement\. For example, if you specify **ansible** and **linux**, then search returns all documents with *either* keyword in their name\.

If you enter a free text search term and choose a search option, such as **Platform type**, then search uses an `AND` statement and returns all documents with the keyword in their name and the specified platform type\.

**Note**  
Note the following details about free text search\.  
Free text search is *not* case sensitive\.
Search terms require a minimum of three characters and have a maximum of 20 characters\.
Free text search accepts up to five search terms\.
If you enter a space between search terms, the system includes the space when searching\.
You can combine free text search with other search options such as **Document type** or **Platform type**\.
The **Document Name Prefix** filter and free text search can't be used together\. They are mutually exclusive\.

**To search for an SSM document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Enter your search terms in the search box, and press Enter\.

### Performing free text document search by using the AWS CLI<a name="ssm-documents-searching-free-text-cli"></a>

**To perform a free text document search by using the CLI**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. To perform free text document search with a single term, run the following command\. In this command, replace *search\_term* with your own information\.

   ```
   aws ssm list-documents --filters Key="SearchKeyword",Values="search_term"
   ```

   Here's an example\.

   ```
   aws ssm list-documents --filters Key="SearchKeyword",Values="aws-asg" --region us-east-2
   ```

   To search using multiple terms that create an `AND` statement, run the following command\. In this command, replace *search\_term\_1* and *search\_term\_2* with your own information\.

   ```
   aws ssm list-documents --filters Key="SearchKeyword",Values="search_term_1","search_term_2","search_term_3" --region us-east-2
   ```

   Here's an example\.

   ```
   aws ssm list-documents --filters Key="SearchKeyword",Values="aws-asg","aws-ec2","restart" --region us-east-2
   ```

## Using filters<a name="ssm-documents-searching-filters"></a>

The Systems Manager **Documents** page automatically displays the following filters when you choose the search box\. 
+ Document name prefix
+ Platform types
+ Document type
+ Tag key

![\[Filter options on SSM Documents page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssm-documents-filters-1.png)

You can search for SSM documents by using a single filter\. If you want to return a more specific set of SSM documents, you can apply multiple filters\. Here is an example of a search that uses the **Platform types** and the **Document name prefix** filters\.

![\[Applying multiple filter options on the SSM Documents page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssm-documents-filters-2.png)

If you apply multiple filters, Systems Manager creates different search statements based on the filters you choose: 
+ If you apply the *same* filter multiple times, for example **Document name prefix**, then Systems Manager searches by using an `OR` statement\. For example, if you specify one filter of **Document name prefix**=**AWS** and a second filter of **Document name prefix**=**Lambda**, then search returns all documents with the prefix "`AWS`" and all documents with the prefix "`Lambda`"\.
+ If you apply *different* filters, for example **Document name prefix** and **Platform types**, then Systems Manager searches by using an `AND` statement\. For example, if you specify a **Document name prefix**=**AWS** filter and a **Platform types**=**Linux** filter, then search returns all documents with the prefix "`AWS`" that are specific to the Linux platform\.

**Note**  
Searches that use filters are case sensitive\. 