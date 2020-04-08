# Uninstall SSM Agent from Linux instances<a name="sysman-uninstall-agent"></a>

Use the following commands to uninstall SSM Agent\.

**Amazon Linux, Amazon Linux 2, RHEL, and CentOS**

```
sudo yum erase amazon-ssm-agent –y
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

**Debian Server**

```
sudo dpkg -r amazon-ssm-agent
```

**SLES**

```
sudo rpm --erase amazon-ssm-agent
```