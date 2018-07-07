# Uninstall SSM Agent from Linux Instances<a name="sysman-uninstall-agent"></a>

Use the following commands to uninstall SSM Agent\.

**Amazon Linux, Amazon Linux 2, RHEL, and CentOS**

```
sudo yum erase amazon-ssm-agent –y
```

**Ubuntu Server**

```
sudo dpkg -r amazon-ssm-agent
```

**SLES**

```
sudo rpm --erase amazon-ssm-agent
```