# Connect using Remote Desktop<a name="fleet-rdp"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to connect to your Windows Server 2012R2 and later instances using the Remote Desktop Protocol \(RDP\)\. These Remote Desktop sessions powered by NICE DCV provide secure connections to your instances directly from your browser\. With Fleet Manager, you can connect a maximum of four instances per browser window\. Currently, Fleet Manager supports only English language inputs\. Though you can connect to instances with Fleet Manager using RDP without opening any inbound ports, Fleet Manager only supports operating system configurations that use the default RDP port 3389 at this time\. If you've changed the value of the listening port for RDP on your instance, Fleet Manager fails to establish the connection\. When connecting to your instance, you can use Windows credentials or the Amazon EC2 key pair \(\.pem file\) associated with the instance for authentication\. For information about Amazon EC2 key pairs, see [Amazon EC2 key pairs and Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) and [Amazon EC2 key pairs and Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances* and *Amazon EC2 User Guide for Windows Instances*\.

Alternatively, if you're authenticated to the AWS Management Console using AWS Single Sign\-On, Fleet Manager integrates with AWS SSO so you can connect to your instances without providing additional credentials\. Fleet Manager supports AWS SSO authenticated RDP connections in the same AWS Region where you enabled AWS SSO and user names can be a maximum of 16 characters\. For AWS SSO authenticated RDP connections, Fleet Manager creates a local user on the instance that persists after the connection ends\. AWS SSO authenticated RDP connections are not supported for nodes that are Microsoft Active Directory domain controllers\.

**Note**  
Fleet Manager RDP connections have a maximum session duration of 60 minutes\. When that duration is reached, Fleet Manager disconnects the session\. You can reconnect to the same session by using your credentials\.

Because Fleet Manager uses Session Manager to connect to Windows instances using RDP, you must complete the prerequisites for Session Manager before using this feature\. Session Manager is a capability of AWS Systems Manager\. Session preferences in the AWS account and AWS Region are applied when connecting to your instances using RDP\. For information about setting up Session Manager, see [Setting up Session Manager](session-manager-getting-started.md)\.

In addition to the required AWS Identity and Access Management \(IAM\) permissions for Systems Manager and Session Manager, the user or role you use to access the console must allow the following actions:
+ `ssm-guiconnect:CancelConnection`
+ `ssm-guiconnect:GetConnection`
+ `ssm-guiconnect:StartConnection`

**To connect to instances using RDP with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance that you want to connect to using RDP\.

1. In the **Node actions** menu, choose **Connect with Remote Desktop**\.

1. Choose your preferred **Authentication type**\. If you choose **User credentials**, enter the user name and password for the Windows user account that you want to use when connecting to the instance\. If you choose **Key pair**, choose the **Browse local machine** option to browse your local machine and choose the PEM key associated with your instance, or copy and paste the contents into the empty field after choosing the **Paste key pair content** option\.

1. Select **Connect**\.