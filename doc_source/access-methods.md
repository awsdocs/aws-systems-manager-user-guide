# Accessing Systems Manager<a name="access-methods"></a>

You can work with Systems Manager in any of the following ways:

**Systems Manager Console**  
The [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/) is a browser\-based interface to access and use Systems Manager\.

**AWS Command Line Tools**  
The AWS command line tools let you issue commands at your system's command line to perform Systems Manager and other AWS tasks, and is supported on Windows, Linux, and macOS\. Using the CLI can be faster and more convenient than using the console\. The command line tools also are useful if you want to build scripts that perform AWS tasks\.   
AWS provides two sets of command line tools: the [AWS Command Line Interface](https://aws.amazon.com/cli/) \(AWS CLI\) and the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. For information about installing and using the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For information about installing and using the Tools for Windows PowerShell, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\.  
On your Windows Server instances, Windows PowerShell 3\.0 or later is required to run certain SSM documents \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. The framework includes PowerShell\.

**AWS SDKs**  
AWS provides software development kits \(SDKs\) that consist of libraries and sample code for various programming languages and platforms \(for example, [Java](https://aws.amazon.com/sdk-for-java/), [Python](https://aws.amazon.com/sdk-for-python/), [Ruby](https://aws.amazon.com/sdk-for-ruby/), [\.NET](https://aws.amazon.com/sdk-for-net/), [iOS and Android](https://aws.amazon.com/mobile/resources/), and [others](https://aws.amazon.com/tools/#sdk)\)\. The SDKs provide a convenient way to create programmatic access to Systems Manager\. For information about the AWS SDKs, including how to download and install them, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/#sdk)\.