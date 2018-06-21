# Operating Systems Supported by Patch Manager<a name="patch-manager-supported-oses"></a>

The Patch Manager capability does not support all the same operating systems versions that are supported by other AWS Systems Manager capabilities\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

Patch Manager can be used to patch operating systems listed in the following table\.


| Operating System | Details | 
| --- | --- | 
|  Linux  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-supported-oses.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-supported-oses.html) Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.  | 
|  Windows  |  Windows Server 2008 through Windows Server 2016, including R2 versions\. Patch Manager provides all patches for supported operating systems within hours of their being made available by Microsoft\.  | 

For a list of operating systems supported overall by Systems Manager, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.