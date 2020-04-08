# Create an SSM document \(API\)<a name="create-ssm-document-api"></a>

After you create the content for your custom SSM document, as described in [Writing SSM document content](create-ssm-doc.md#writing-ssm-doc-content), you can use your preferred SDK to call the AWS Systems Manager [CreateDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateDocument.html) API action to create an SSM document using your content\. The JSON or YAML string for the `Content` request parameter is generally read from a file\. The following sample functions create an SSM document using the SDKs for Python, Go, and Java\.

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

For more information on creating custom document content, see [SSM document syntax](sysman-doc-syntax.md)\.