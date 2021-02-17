# Step 3: Install a TLS certificate on on\-premises servers and VMs<a name="hybrid-tls-certificate"></a>

A Transport Layer Security \(TLS\) certificate must be installed on each managed instance you use with Systems Manager\. AWS services use these certificates to encrypt calls to other AWS services\.

A TLS certificate is already installed by default on each EC2 instance created from any Amazon Machine Image \(AMI\)\. Most modern operating systems include the required TLS certificate from Amazon Trust Services CAs in their trust store\.

If you find the required Amazon Trust Services CA certificates are not installed on your base operating systems, on instances created from AMIs that are not supplied by Amazon, or on your own on\-premises servers and VMs, you must install and enable a certificate from [Amazon Trust Services](https://www.amazontrust.com/repository/), or use AWS Certificate Manager \(ACM\) to create and manage certificates for a supported integrated service\.

Each of your managed instances must have one of the following Transport Layer Security \(TLS\) certificates installed\.
+ Amazon Root CA 1
+ Starfield Services Root Certificate Authority \- G2
+ Starfield Class 2 Certificate Authority

For information about using ACM, see the *[AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)*\.

If certificates in your computing environment are managed by a Group Policy Object \(GPO\), then you might need to configure Group Policy to include one of these certificates\.

For more information about the Amazon Root and Starfield certificates, see the blog post [How to Prepare for AWS’s Move to Its Own Certificate Authority](http://aws.amazon.com/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/)\.

Continue to [Step 4: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\.