# Working with documents<a name="documents-using"></a>

This section includes information about how to use and work with SSM documents\.

**Topics**
+ [Using SSM documents in State Manager Associations](#with-state-manager-associations)
+ [Comparing SSM document versions](#comparing-versions)
+ [Create an SSM document \(console\)](#create-ssm-console)
+ [Create an SSM document \(command line\)](#create-ssm-document-cli)
+ [Create an SSM document \(API\)](#create-ssm-document-api)
+ [Deleting custom SSM documents](#deleting-documents)
+ [Running documents from remote locations](documents-running-remote-github-s3.md)
+ [Sharing SSM documents](documents-ssm-sharing.md)
+ [Searching for SSM documents](ssm-documents-searching.md)

## Using SSM documents in State Manager Associations<a name="with-state-manager-associations"></a>

If you create an SSM document for State Manager, a capability of AWS Systems Manager, you must associate the document with your managed instances after you add the document to the system\. For more information, see [Creating associations](sysman-state-assoc.md)\.

Keep in mind the following details when using SSM documents in State Manager associations\.
+ You can assign multiple documents to a target by creating different State Manager associations that use different documents\. 
+ If you create a document with conflicting plugins \(for example, domain join and remove from domain\), the last plugin run will be the final state\. State Manager doesn't validate the logical sequence or rationality of the commands or plugins in your document\.
+ When processing documents, instance associations are applied first, and next tagged group associations are applied\. If an instance is part of multiple tagged groups, then the documents that are part of the tagged group won't be run in any particular order\. If an instance is directly targeted through multiple documents by its instance ID, there is no particular order of execution\. 
+ If you change the default version of an SSM Policy document for State Manager, any association that uses the document will start using the new default version the next time Systems Manager applies the association to the instance\.
+ If you create an association using an SSM document that was shared with you, and then the owner stops sharing the document with you, your associations no longer have access to that document\. However, if the owner shares the same SSM document with you again later, your associations automatically remap to it\.

## Comparing SSM document versions<a name="comparing-versions"></a>

You can compare the differences in content between versions of AWS Systems Manager \(SSM\) documents in the Systems Manager Documents console\. When comparing versions of an SSM document, differences between the content of the versions are highlighted\.

**To compare SSM document content \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document whose content you want to compare\.

1. On the **Content** tab, select **Compare versions**, and choose the version of the document you want to compare the content to\.

## Create an SSM document \(console\)<a name="create-ssm-console"></a>

After you create the content for your custom SSM document, as described in [Writing SSM document content](documents-creating-content.md#writing-ssm-doc-content), you can use the Systems Manager console to create an SSM document using your content\.

**To create an SSM document \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create command or session**\.

1. Enter a descriptive name for the document 

1. \(Optional\) For **Target type**, specify the type of resources the document can run on\.

1. In the **Document type** list, choose the type of document you want to create\.

1. Delete the brackets in the **Content** field, and then paste the document content you created earlier\.

1. \(Optional\) In the **Document tags** section, apply one or more tag key name/value pairs to the document\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a document to identify the type of tasks it runs, the type of operating systems it targets, and the environment it runs in\. In this case, you could specify the following key name/value pairs:
   + `Key=TaskType,Value=MyConfigurationUpdate`
   + `Key=OS,Value=AMAZON_LINUX_2`
   + `Key=Environment,Value=Production`

   For more information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.

1. Choose **Create document** to save the document\.

## Create an SSM document \(command line\)<a name="create-ssm-document-cli"></a>

After you create the content for your custom AWS Systems Manager \(SSM\) document, as described in [Writing SSM document content](documents-creating-content.md#writing-ssm-doc-content), you can use the AWS Command Line Interface \(AWS CLI\) or AWS Tools for PowerShell to create an SSM document using your content\. This is shown in the following command\.

**Before you begin**  
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\. For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

Run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

```
aws ssm create-document \
--content file://path/to/file/documentContent.json \
--name "document-name" \
--document-type "Command" \
--tags "Key=tag-key,Value=tag-value"
```

------
#### [ Windows ]

```
aws ssm create-document ^
--content file://C:\path\to\file\documentContent.json ^
--name "document-name" ^
--document-type "Command" ^
--tags "Key=tag-key,Value=tag-value"
```

------
#### [ PowerShell ]

```
$json = Get-Content -Path "C:\path\to\file\documentContent.json" | Out-String
New-SSMDocument `
-Content $json `
-Name "document-name" `
-DocumentType "Command" `
-Tags "Key=tag-key,Value=tag-value"
```

------

If successful, the command returns a response similar to the following\.

```
{
"DocumentDescription":{
  "CreatedDate":1.585061751738E9,
  "DefaultVersion":"1",
  "Description":"MyCustomDocument",
  "DocumentFormat":"JSON",
  "DocumentType":"Command",
  "DocumentVersion":"1",
  "Hash":"0d3d879b3ca072e03c12638d0255ebd004d2c65bd318f8354fcde820dEXAMPLE",
  "HashType":"Sha256",
  "LatestVersion":"1",
  "Name":"Example",
  "Owner":"111122223333",
  "Parameters":[
     --truncated--
  ],
  "PlatformTypes":[
     "Windows",
     "Linux"
  ],
  "SchemaVersion":"0.3",
  "Status":"Creating",
  "Tags": [
        {
            "Key": "Purpose",
            "Value": "Test"
        }
    ]
}
}
```

## Create an SSM document \(API\)<a name="create-ssm-document-api"></a>

After you create the content for your custom AWS Systems Manager \(SSM\) document, as described in [Writing SSM document content](documents-creating-content.md#writing-ssm-doc-content), you can use your preferred SDK to call the AWS Systems Manager [CreateDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateDocument.html) API operation to create an SSM document using your content\. The JSON or YAML string for the `Content` request parameter is generally read from a file\. The following sample functions create an SSM document using the SDKs for Python, Go, and Java\.

------
#### [ Python ]

```
import boto3

ssm = boto3.client('ssm')
filepath = '/path/to/file/documentContent.yaml'

def createDocumentApiExample():
with open(filepath) as openFile:
    documentContent = openFile.read()
    createDocRequest = ssm.create_document(
        Content = documentContent,
        Name = 'createDocumentApiExample',
        DocumentType = 'Automation',
        DocumentFormat = 'YAML' 
    )
    print(createDocRequest)

createDocumentApiExample()
```

------
#### [ Go ]

```
package main

import (
"github.com/aws/aws-sdk-go/aws"
"github.com/aws/aws-sdk-go/aws/session"
"github.com/aws/aws-sdk-go/service/ssm"

"fmt"
"io/ioutil"
"log"
)


func main() {
openFile, err := ioutil.ReadFile("/path/to/file/documentContent.yaml")
if err != nil {
    log.Fatal(err)
}
documentContent := string(openFile)
sesh := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable}))

ssmClient := ssm.New(sesh)
createDocRequest, err := ssmClient.CreateDocument(&ssm.CreateDocumentInput{
    Content: &documentContent,
    Name:  aws.String("createDocumentApiExample"),
    DocumentType: aws.String("Automation"),
    DocumentFormat: aws.String("YAML"),
})
result := *createDocRequest
fmt.Println(result)
}
```

------
#### [ Java ]

```
import java.io.IOException;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Paths;

import com.amazonaws.AmazonClientException;
import com.amazonaws.AmazonServiceException;
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagement;
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagementClientBuilder;
import com.amazonaws.services.simplesystemsmanagement.model.*;

public class createDocumentApiExample {
public static void main(String[] args) {
try {
  createDocumentMethod(getDocumentContent());
}
catch (IOException e) {
  e.printStackTrace();
}
}
public static String getDocumentContent() throws IOException {
  String filepath = new String("/path/to/file/documentContent.yaml");
    byte[] encoded = Files.readAllBytes(Paths.get(filepath));
    String documentContent = new String(encoded, StandardCharsets.UTF_8);
    return documentContent;
}

public static void createDocumentMethod (final String documentContent) {
  AWSSimpleSystemsManagement ssm = AWSSimpleSystemsManagementClientBuilder.defaultClient();
  final CreateDocumentRequest createDocRequest = new CreateDocumentRequest()
    .withContent(documentContent)
    .withName("createDocumentApiExample")
    .withDocumentType("Automation")
    .withDocumentFormat("YAML");
  final CreateDocumentResult result = ssm.createDocument(createDocRequest);
}
}
```

------

For more information about creating custom document content, see [Data elements and parameters](documents-syntax-data-elements-parameters.md)\.

## Deleting custom SSM documents<a name="deleting-documents"></a>

If you no longer want to use a custom SSM document, you can delete it by using either the AWS Command Line Interface \(AWS CLI\) or the AWS Systems Manager console\. 

**To delete an SSM document \(AWS CLI\)**

1. Before you delete the document, we recommend that you disassociate all instances that are associated with the document\.

   Run the following command to disassociate an instance from a document\.

   ```
   aws ssm delete-association --instance-id "123456789012" --name "documentName"
   ```

   There is no output if the command succeeds\.

1. Run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux ]

   ```
   aws ssm delete-document \
       --name "document-name" \
       --document-version "document-version" \
       --version-name "version-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm delete-document ^
       --name "document-name" ^
       --document-version "document-version" ^
       --version-name "version-name"
   ```

------
#### [ PowerShell ]

   ```
   Delete-SSMDocument `
       -Name "document-name" `
       -DocumentVersion 'document-version' `
       -VersionName 'version-name'
   ```

------

   There is no output if the command succeeds\.
**Important**  
If the `document-version` or the `version-name` are not provided, all versions of the document are deleted\.

**To delete an SSM document \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Select the document you want to delete\.

1. Select **Delete**\. When prompted to delete the document, select **Delete**\.