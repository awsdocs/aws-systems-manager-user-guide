# Uninstall SSM Agent from Linux Instances<a name="sysman-uninstall-agent"></a>

Use the following commands to uninstall SSM Agent\.

**Amazon Linux, RHEL, or Cent OS**

```
sudo yum erase amazon-ssm-agent –y
```

**Ubuntu 18.04 and later (using snaps)**

```
sudo snap remove amazon-ssm-agent
```

**Ubuntu 16.04 and earlier (using DEB packages)**

```
sudo dpkg -r amazon-ssm-agent
```

**SLES**

```
sudo rpm --erase amazon-ssm-agent
```
