# Creating composite documents<a name="composite-docs"></a>

A *composite* AWS Systems Manager \(SSM\) document is a custom document that performs a series of actions by running one or more secondary SSM documents\. Composite documents promote *infrastructure as code* by allowing you to create a standard set of SSM documents for common tasks such as boot\-strapping software or domain\-joining instances\. You can then share these documents across AWS accounts in the same AWS Region to reduce SSM document maintenance and ensure consistency\.

For example, you can create a composite document that performs the following actions:

1. Installs all patches in the allow list\.

1. Installs antivirus software\.

1. Downloads scripts from GitHub and runs them\.

In this example, your custom SSM document includes the following plugins to perform these actions:

1. The `aws:runDocument` plugin to run the `AWS-RunPatchBaseline` document, which installs all allow listed patches\.

1. The `aws:runDocument` plugin to run the `AWS-InstallApplication` document, which installs the antivirus software\.

1. The `aws:downloadContent` plugin to download scripts from GitHub and run them\.

Composite and secondary documents can be stored in Systems Manager, GitHub \(public and private repositories\), or Amazon S3\. Composite documents and secondary documents can be created in JSON or YAML\. 

**Note**  
Composite documents can only run to a maximum depth of three documents\. This means that a composite document can call a child document; and that child document can call one last document\.

## Create a composite document<a name="composite-creating"></a>

To create a composite document, add the [`aws:runDocument`](ssm-plugins.md#aws-rundocument) plugin in a custom SSM document and specify the required inputs\. The following is an example of a composite document that performs the following actions:

1. Runs the [`aws:downloadContent`](ssm-plugins.md#aws-downloadContent) plugin to download an SSM document from a GitHub public repository to a local directory called bootstrap\. The SSM document is called StateManagerBootstrap\.yml \(a YAML document\)\.

1. Runs the `aws:runDocument` plugin to run the StateManagerBootstrap\.yml document\. No parameters are specified\.

1. Runs the `aws:runDocument` plugin to run the `AWS-ConfigureDocker pre-defined` SSM document\. The specified parameters install Docker on the instance\.

```
{
  "schemaVersion": "2.2",
  "description": "My composite document for bootstrapping software and installing Docker.",
  "parameters": {
  },
  "mainSteps": [
    {
      "action": "aws:downloadContent",
      "name": "downloadContent",
      "inputs": {
        "sourceType": "GitHub",
        "sourceInfo": "{\"owner\":\"TestUser1\",\"repository\":\"TestPublic\", \"path\":\"documents/bootstrap/StateManagerBootstrap.yml\"}",
        "destinationPath": "bootstrap"
      }
    },
    {
      "action": "aws:runDocument",
      "name": "runDocument",
      "inputs": {
        "documentType": "LocalPath",
        "documentPath": "bootstrap",
        "documentParameters": "{}"
      }
    },
    {
      "action": "aws:runDocument",
      "name": "configureDocker",
      "inputs": {
        "documentType": "SSMDocument",
        "documentPath": "AWS-ConfigureDocker",
        "documentParameters": "{\"action\":\"Install\"}"
      }
    }
  ]
}
```

**More info**  
+ For information about rebooting servers and instances when using Run Command to call scripts, see [Handling reboots when running commands](send-commands-reboot.md)\.
+ For more information about creating an SSM document, see [Creating SSM documents](create-ssm-doc.md)\.
+ For more information about the plugins you can add to a custom SSM document, see [Systems Manager Command document plugin reference](ssm-plugins.md)\.
+ If you simply want to run a document from a remote location \(without creating a composite document\), see [Running Systems Manager Command documents from remote locations](run-remote-documents.md)\.