# Step 3: Install a TLS certificate on On\-Premises Servers and VMs<a name="hybrid-tls-certificate"></a>

A Transport Layer Security \(TLS\) certificate must be installed on each managed instance you use with Systems Manager\. These certificates encrypt calls to other AWS services\. A TLS certificate is already installed on each Amazon EC2 instance created from any Amazon Machine Image \(AMI\)\.

For base operating systems, or systems in your hybrid environment, you must install a certificate from [Amazon Trust Services](https://www.amazontrust.com/repository/)\.

For more information, see [Prerequisite: TLS Certificates](systems-manager-prereqs.md#prereqs-tls-certificate)\.

Continue to [Step 4: Create a Managed\-Instance Activation for a Hybrid Environment](sysman-managed-instance-activation.md)\.