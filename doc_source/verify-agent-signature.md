# Verifying the signature of the SSM Agent<a name="verify-agent-signature"></a>

The AWS Systems Manager Agent \(SSM Agent\) deb and rpm installer packages for Linux instances are cryptographically signed\. You can use a public key to verify that the agent package is original and unmodified\. If there is any damage or alteration to the files, the verification fails\. You can verify the signature of the installer package using either RPM or GPG\.

To find the correct signature file for your instance's architecture and operating system, see the following table\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.


| Architecture | Operating system | Signature file URL | Agent download file name | 
| --- | --- | --- | --- | 
| Intel 64\-bit \(x86\_64\) |  Amazon Linux, Amazon Linux 2, CentOS, RHEL, Oracle Linux, SLES  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm.sig`  |  `amazon-ssm-agent.rpm`  | 
| Intel 64\-bit \(x86\_64\) |  Debian Server, Ubuntu Server  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_amd64/amazon-ssm-agent.deb.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb.sig`  | amazon\-ssm\-agent\.deb | 
| Intel 32\-bit \(x86\) |  Amazon Linux, Amazon Linux 2, CentOS, RHEL  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_386/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm.sig`  |  `amazon-ssm-agent.rpm`  | 
| Intel 32\-bit \(x86\) |  Ubuntu Server  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_386/amazon-ssm-agent.deb.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb.sig`  |  `amazon-ssm-agent.deb`  | 
| ARM 64\-bit \(arm64\) |  Amazon Linux, Amazon Linux 2, CentOS, RHEL  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm.sig`  | amazon\-ssm\-agent\.rpm | 

**To verify the SSM Agent package on a Linux server**

1. Copy the following public key, and save it to a file named `amazon-ssm-agent.gpg`\.

   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   Version: GnuPG v2.0.22 (GNU/Linux)
   
   mQENBF98p2YBCADgfK6NJS/1UFMEBq+DbHrLGCPR7uabN7KByIWJ6X0gGqxad0y7
   kP+M2YhWVlteeytpJgEEzKFIXkv7vZdRIjCrgIiNISdvDyYOTNQ2n5Ck5XPnJTQg
   n5HIRccvc+Lwdidl8auiCYteDCDCGM5EPb7vUrbrg+y4RkXeBNErzo7rbVnWW4QC
   z8x6EVLb24w/AONHLxywwunagorWiVBP6snrBoz2d2wQYAfpPmPsoLRAURiMnubG
   bDOM9hb5bGi2OY92L9fVChVRGJnxMNYPCQWFyUovRis9fKnmP1LopUmlNSmSqUj1
   AD7WRDMGn2Ruf+HYEZuY+pDD/C2ejcJtjDJTABEBAAG0J1NTTSBBZ2VudCA8c3Nt
   LWFnZW50LXNpZ25lckBhbWF6b24uY29tPokBPwQTAQIAKQUCX3ynZgIbLwUJAsaY
   gAcLCQgHAwIBBhUIAgkKCwQWAgMBAh4BAheAAAoJEFT09W5pPsohHGQIALMvf8oq
   wEU5gph5SlrjYTIqZqsvyV8RKsUEFin5EDkeLC5ALpsby6rAWnobCy2Ce1p4buS+
   sA/PFKkraVWtpmqOOkCZoBJTWZyR3KtY7y2pTUWl7aaj20NEO/nPI1VH/E47iH7m
   scYAOxbNOcEbRiip7AdXZXK7nKda51q/b6G92fM86pl8VPBAh6ijMNmEEZxIAWH2
   AGY7Y9imwnp+UpUUwsJb3/L0asqMecPrYJLGWke6EYGPuDfxYb1+YOuZOY/mjDJJ
   z6f7G2nCuDMniabydk3269eLRPuRHUq4P5Sv+I/zdJI4B8lOJfJRpy/mwGwAU74l
   s7csneMjUO2zIzaJAhwEEAECAAYFAl98p2YACgkQfdCXo9rX9fzFHw//akOS57o3
   lyQySKmbEpAhDrEcg4NGqidlp3NjqkxKmmK5GMwC+wJS+hmwuBiMH1knSaxc/0ie
   XmtxHsmDn8JmREypkfUS+vAONlmsuFJUjXipa5cAP4YjPMTW7HNxC/WrLV6NSuQZ
   5nweVeXAQPxjOoNaAOOk1hlUuGdypPxCNV6NYLm5W7jz1buDYOhNwPvVP63wy1BK
   ME4HzE94ggCxnXdafJU2KR11Mj/9LRFeDJ8X8huSKOFNOy2IotuW5VmxlDvbkvDT
   ceelqWJjh5CsWKmWActoxqtyiedQqxgsxFuwqVIWxP758C3NP1zpxvr8SXxdJBy3
   8U4iHC3I89zlX4x4tPiMn3vQOq+RhnZEzEphrmPkQAaq6H160hHxQz44DoM8jDIn
   f/EbWKPkw+p5679JUrXIZDOYP2OlbKoAY4axfCwvjIqAQ5KWFQyKmWyoRwTl4IrC
   bAXqljtqzyF20g2puNpxpvxT8CF+YaKYPKqXAbZkBQoOoPBbEGGG19BX5rCBehTx
   QwBAgmmk7FG162TY2uivbwjmguh4DM4PgEoHtsgg9UVM+A+M5tIuEeTC5jWgzEcf
   VkwTY6N+3XNvAnYNobND8mvN+QAJG7NpryX1fNBaxGsze3QBL42v/zFmG6VSfINp
   4H01UHp8Pmidk8axmi+w6hoqB+uDo3lgd6U=
   =c8Y2
   -----END PGP PUBLIC KEY BLOCK-----
   ```

1. Import the public key into your keyring, and note the returned key value\.

------
#### [ GPG ]

   ```
   gpg --import amazon-ssm-agent.gpg
   ```

------
#### [ RPM ]

   ```
   rpm --import amazon-ssm-agent.gpg
   ```

------

1. Verify the fingerprint\. Be sure to replace *key\-value* with the value from the preceding step\. We recommend that you use GPG to verify the fingerprint even if you use RPM to verify the installer package\.

   ```
   gpg --fingerprint key-value
   ```

   This command returns output similar to the following\.

   ```
   pub   2048R/693ECA21 2020-10-06 [expires: 2022-03-29]
      Key fingerprint = 8108 A07A 9EBE 248E 3F1C  63F2 54F4 F56E 693E CA21
   uid                  SSM Agent <ssm-agent-signer@amazon.com>
   ```

   The fingerprint should match the following:

   `8108 A07A 9EBE 248E 3F1C 63F2 54F4 F56E 693E CA21`

   If the fingerprint doesn't match, don't install the agent\. Contact AWS Support\.

1. If you're using GPG to verify the installer package, download the signature file according to your instance's architecture and operating system if you have not already done so\. Note that RPM packages already contain the needed signature for RPM verification\.

1. Verify the installer package signature\. Be sure to replace the *signature\-filename* and *agent\-download\-filename* with the values you specified when downloading the signature file and agent\.

------
#### [ GPG ]

   ```
   gpg --verify signature-filename agent-download-filename
   ```

------
#### [ RPM ]

   ```
   rpm --checksig agent-download-filename
   ```

------

   This command returns output similar to the following\.

------
#### [ GPG ]

   ```
   gpg: Signature made Wed 07 Oct 2020 05:52:47 PM UTC using RSA key ID 693ECA21
   gpg: Good signature from "SSM Agent <ssm-agent-signer@amazon.com>"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg: There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 8108 A07A 9EBE 248E 3F1C 63F2 54F4 F56E 693E CA21
   ```

------
#### [ RPM ]

   ```
   amazon-ssm-agent-2.3.1319.0-1.amzn2.x86_64.rpm: rsa sha1 (md5) pgp md5 OK
   ```

------

   When using GPG, if the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact AWS Support and don't install the agent\. The warning message about the trust doesn't mean that the signature is invalid, only that you have not verified the public key\. A key is trusted only if you or someone who you trust has signed it\.

   When using RPM, if `pgp` is missing from the output and you have imported the public key, then the agent is not signed\. If the output contains the phrase `NOT OK (MISSING KEYS: (MD5) key-id)`, check whether you performed the procedure correctly\. If you continue to get this response, contact AWS Support and don't install the agent\.