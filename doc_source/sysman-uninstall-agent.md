# Uninstall SSM Agent from Linux instances<a name="sysman-uninstall-agent"></a>

Use the following commands to uninstall AWS Systems Manager Agent \(SSM Agent\)\.

**Amazon Linux, Amazon Linux 2, CentOS, Oracle Linux, and Red Hat Enterprise Linux**

```
sudo yum erase amazon-ssm-agent --assumeyes
```

**Debian Server**

```
sudo dpkg -r amazon-ssm-agent
```

**SUSE Linux Enterprise Server \(SLES\)**

```
sudo rpm --erase amazon-ssm-agent
```

**Ubuntu Server**
+ **deb package installations**:

  ```
  sudo dpkg -r amazon-ssm-agent
  ```
+ **snap package installations**:

  ```
  sudo snap remove amazon-ssm-agent
  ```