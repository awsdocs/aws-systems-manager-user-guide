# Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server<a name="sysman-proxy-with-ssm-agent-al-python-requests"></a>

To patch an instance that is using a proxy and that was created from an Amazon Linux AMI, Patch Manager requires a recent version of the Python `requests` module to be installed on the instance\. We recommend always upgrading to the most recently released version\.

To ensure the latest version of the Python `requests` module is installed, follow these steps:

1. Sign in to the Amazon Linux instance, or use the **AWS\-RunShellScript** SSM document in Run Command, and run the following command on the instance: 

   ```
   pip list | grep requests
   ```
   + If the module is installed, the request returns the version number in a response similar to the following:

     ```
     requests (1.2.3) 
     ```
   + If the module is not installed, run the following command to install it:

     ```
     pip install requests
     ```
   + If pip itself is not installed, run the following command to install it:

     ```
     sudo yum install -y python-pip
     ```

1. If the module is installed, but the version listed is earlier than `2.18.4` \(such as `1.2.3` shown in the previous step\), run the following command to upgrade to the latest version of the Python `requests` module:

   ```
   pip install requests --upgrade
   ```