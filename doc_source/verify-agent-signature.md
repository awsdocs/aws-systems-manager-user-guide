# Verifying the signature of the SSM Agent<a name="verify-agent-signature"></a>

The AWS Systems Manager Agent \(SSM Agent\) deb and rpm installer packages for Linux instances are cryptographically signed\. You can use a public key to verify that the agent package is original and unmodified\. If there is any damage or alteration to the files, the verification fails\. You can verify the signature of the installer package using either RPM or GPG\. The following information is for SSM Agent versions 3\.1\.1141\.0 or later\.

**Important**  
The public key shown later in this topic expires on 2023\-09\-05 \(September 5, 2023\)\. Systems Manager will publish a new public key in this topic before the old one expires\. We encourage you to subscribe to the RSS feed for this topic to get a notification when the new key is available\.

To find the correct signature file for your instance's architecture and operating system, see the following table\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.


| Architecture | Operating system | Signature file URL | Agent download file name | 
| --- | --- | --- | --- | 
| x86\_64 |  Amazon Linux, Amazon Linux 2, Amazon Linux 2023, CentOS, RHEL, Oracle Linux, SLES  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm.sig`  |  `amazon-ssm-agent.rpm`  | 
| x86\_64 |  Debian Server, Ubuntu Server  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_amd64/amazon-ssm-agent.deb.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb.sig`  | amazon\-ssm\-agent\.deb | 
| x86 |  Amazon Linux, Amazon Linux 2, Amazon Linux 2023, CentOS, RHEL  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_386/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm.sig`  |  `amazon-ssm-agent.rpm`  | 
| x86 |  Ubuntu Server  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_386/amazon-ssm-agent.deb.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb.sig`  |  `amazon-ssm-agent.deb`  | 
| ARM64 |  Amazon Linux, Amazon Linux 2, Amazon Linux 2023, CentOS, RHEL  |  `https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm.sig` `https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm.sig`  | amazon\-ssm\-agent\.rpm | 

------
#### [ GPG ]

**To verify the SSM Agent package on a Linux server**

1. Copy the following public key, and save it to a file named `amazon-ssm-agent.gpg`\.

   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   Version: GnuPG v2.0.22 (GNU/Linux)
   
   mQENBGIxF/8BCADv014neDCfkpdj79/XVeQVy0Wz9LSiB/iksc1jTPaCgD/9ojdQ
   10LfEFEyLoeTEhX5WBu0Ry7oKW9AK51kscMjTHwdFnzXsT4tAoSXxh7lbgdfhpVm
   bJ0bVArrzKIQ8JOE2lrn6LgVcGTtbPGURNNNRD1nZEgZm6wni+ZoplsXmsj0wD7f
   I5zhk/e+OyrsolpNWBJB0vf6JXVV2MauZKGlwRR4pZoSw5yPOa0rZDtOTtPbUX5C
   lWGLtdQ3848YvgjMzK9GeEqK9n6yQx5potlvxJ6TCZsZTwXXF5LyPuv2y6U22075
   JjMMX7noNnVnipKMj+l7x5fis+X+gafF/PbTABEBAAG0J1NTTSBBZ2VudCA8c3Nt
   LWFnZW50LXNpZ25lckBhbWF6b24uY29tPokBPwQTAQIAKQUCYjEX/wIbLwUJAsaY
   gAcLCQgHAwIBBhUIAgkKCwQWAgMBAh4BAheAAAoJEN2BphdWuqVJUKoIANHALkLq
   xsUco2JwymOorf+1icVtL8MSdi87lIhxfIGWaGN5CkzrkBAJlIyf/C+hVcLzR9rQ
   DWIJakLWE3XPb4g8fWyr5VlOoYbcGLCky0fL5O0pWEnF2ecQMMSpwkdv9zx7qUoo
   PssEpuwz5kIOYp2ENy21IPkMGpny8MCbzQ+sHysLWiJ/b0aWX9giPuMe5vTO3djM
   CPtyA5CeG3BMawPOaDQvjxB+DnWCg1HslgdzpZiSsusuZ8u3xKaehEMiB/Li2BO9
   yZMAeG6iok4Dn01ZVVpU9mftZKIm/T5WBX5x+TBhQ1b30MQcN61kFEe0Gll3ReTu
   CPEuDwAb4WruFkaJAhwEEAECAAYFAmIxGAAACgkQfdCXo9rX9fy5yQ/+PIBXWQc4
   D/a6/nEaGM/FrLDLgPSieBCbU4TpvB7qPg6gJUX8CA+h8cZ06wDgcdi9sJ3MwTnQ
   Ze1OzZ8AJroRP6XhwVeNEbeedBbmr7irSg8lIdyXZed0G0T+7SX/MDEyup16vRxW
   k2UyBCXYqnxBHXeTKf9GxH0nODpcGPGByqjfmSB3nj2wZN0g8SWWz6oEWcXv218B
   FJyJj7W2bQsbMXoHlILP28Ec5QN1r8cC1b1nQsmx4120XSKFWvi8trG2+dDb58LR
   1afsEW8OhJwsJcba1YIMznxMbWpfyZww2S6g7rFahm1wKCxMkHIZ+Fca6axKoK9Y
   KJaEPn9rbhh11XsgKBNIIP1h0eGmQTAvM01dWI9895fiaK3pQkCxV7in6dTxi8Jy
   7iJBbORStxsospBJzLf+0Ca3yvILxySg1Q2EuOKuN2VW7N/l3IffJ85DVjjQgh6A
   T4L6ViK/0L6ww5n8tboKB/Jz9OUDGf2idxhQe8WenIogAU3y4ZGUyzcZHMg9lRke
   hdLYGtqRATdWuwFQbwjPeBNovulqKOPXU9BLEezz8gMtd6/aW/UQA33xuZlh959o
   DHhGwWDXEJzhrIlFAljkb7rsIhhjrg/R2usSIi78i1jFkGsVqRET2/avn7/kBcgL
   yIk43DugjkN04nzHfULMJmEm02uVumgSJzQ=
   =rGEs
   -----END PGP PUBLIC KEY BLOCK-----
   ```

1. Import the public key into your keyring, and note the returned key value\.

   ```
   gpg --import amazon-ssm-agent.gpg
   ```

1. Verify the fingerprint\. Be sure to replace *key\-value* with the value from the preceding step\. We recommend that you use GPG to verify the fingerprint even if you use RPM to verify the installer package\.

   ```
   gpg --fingerprint key-value
   ```

   This command returns output similar to the following\.

   ```
   pub   2048R/56BAA549 2022-03-15 [expires: 2023-09-05]
       Key fingerprint = 2BC7 C7C2 67BB D505 EAA4  91E6 DD81 A617 56BA A549
   uid                  SSM Agent <ssm-agent-signer@amazon.com>
   ```

   The fingerprint should match the following\.

   `2BC7 C7C2 67BB D505 EAA4 91E6 DD81 A617 56BA A549 `

   If the fingerprint doesn't match, don't install the agent\. Contact AWS Support\.

1. Download the signature file according to your instance's architecture and operating system if you haven't already done so\.

1. Verify the installer package signature\. Be sure to replace the *signature\-filename* and *agent\-download\-filename* with the values you specified when downloading the signature file and agent\.

   ```
   gpg --verify signature-filename agent-download-filename
   ```

   This command returns output similar to the following\.

   ```
   gpg: Signature made Mon 21 Mar 2022 05:52:47 PM UTC using RSA key ID 693ECA21
   gpg: Good signature from "SSM Agent <ssm-agent-signer@amazon.com>"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg: There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 2BC7 C7C2 67BB D505 EAA4 91E6 DD81 A617 56BA A549
   ```

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact AWS Support and don't install the agent\. The warning message about the trust doesn't mean that the signature isn't valid, only that you haven't verified the public key\. A key is trusted only if you or someone who you trust has signed it\. If the output includes the phrase `Can't check signature: No public key`, verify you downloaded SSM Agent version 3\.1\.1141\.0 or later\.

------
#### [ RPM ]

**To verify the SSM Agent package on a Linux server**

1. Copy the following public key, and save it to a file named `amazon-ssm-agent.gpg`\.

   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   Version: GnuPG v2.0.22 (GNU/Linux)
   
   mQENBGIxF/8BCADv014neDCfkpdj79/XVeQVy0Wz9LSiB/iksc1jTPaCgD/9ojdQ
   10LfEFEyLoeTEhX5WBu0Ry7oKW9AK51kscMjTHwdFnzXsT4tAoSXxh7lbgdfhpVm
   bJ0bVArrzKIQ8JOE2lrn6LgVcGTtbPGURNNNRD1nZEgZm6wni+ZoplsXmsj0wD7f
   I5zhk/e+OyrsolpNWBJB0vf6JXVV2MauZKGlwRR4pZoSw5yPOa0rZDtOTtPbUX5C
   lWGLtdQ3848YvgjMzK9GeEqK9n6yQx5potlvxJ6TCZsZTwXXF5LyPuv2y6U22075
   JjMMX7noNnVnipKMj+l7x5fis+X+gafF/PbTABEBAAG0J1NTTSBBZ2VudCA8c3Nt
   LWFnZW50LXNpZ25lckBhbWF6b24uY29tPokBPwQTAQIAKQUCYjEX/wIbLwUJAsaY
   gAcLCQgHAwIBBhUIAgkKCwQWAgMBAh4BAheAAAoJEN2BphdWuqVJUKoIANHALkLq
   xsUco2JwymOorf+1icVtL8MSdi87lIhxfIGWaGN5CkzrkBAJlIyf/C+hVcLzR9rQ
   DWIJakLWE3XPb4g8fWyr5VlOoYbcGLCky0fL5O0pWEnF2ecQMMSpwkdv9zx7qUoo
   PssEpuwz5kIOYp2ENy21IPkMGpny8MCbzQ+sHysLWiJ/b0aWX9giPuMe5vTO3djM
   CPtyA5CeG3BMawPOaDQvjxB+DnWCg1HslgdzpZiSsusuZ8u3xKaehEMiB/Li2BO9
   yZMAeG6iok4Dn01ZVVpU9mftZKIm/T5WBX5x+TBhQ1b30MQcN61kFEe0Gll3ReTu
   CPEuDwAb4WruFkaJAhwEEAECAAYFAmIxGAAACgkQfdCXo9rX9fy5yQ/+PIBXWQc4
   D/a6/nEaGM/FrLDLgPSieBCbU4TpvB7qPg6gJUX8CA+h8cZ06wDgcdi9sJ3MwTnQ
   Ze1OzZ8AJroRP6XhwVeNEbeedBbmr7irSg8lIdyXZed0G0T+7SX/MDEyup16vRxW
   k2UyBCXYqnxBHXeTKf9GxH0nODpcGPGByqjfmSB3nj2wZN0g8SWWz6oEWcXv218B
   FJyJj7W2bQsbMXoHlILP28Ec5QN1r8cC1b1nQsmx4120XSKFWvi8trG2+dDb58LR
   1afsEW8OhJwsJcba1YIMznxMbWpfyZww2S6g7rFahm1wKCxMkHIZ+Fca6axKoK9Y
   KJaEPn9rbhh11XsgKBNIIP1h0eGmQTAvM01dWI9895fiaK3pQkCxV7in6dTxi8Jy
   7iJBbORStxsospBJzLf+0Ca3yvILxySg1Q2EuOKuN2VW7N/l3IffJ85DVjjQgh6A
   T4L6ViK/0L6ww5n8tboKB/Jz9OUDGf2idxhQe8WenIogAU3y4ZGUyzcZHMg9lRke
   hdLYGtqRATdWuwFQbwjPeBNovulqKOPXU9BLEezz8gMtd6/aW/UQA33xuZlh959o
   DHhGwWDXEJzhrIlFAljkb7rsIhhjrg/R2usSIi78i1jFkGsVqRET2/avn7/kBcgL
   yIk43DugjkN04nzHfULMJmEm02uVumgSJzQ=
   =rGEs
   -----END PGP PUBLIC KEY BLOCK-----
   ```

1. Import the public key into your keyring, and note the returned key value\.

   ```
   rpm --import amazon-ssm-agent.gpg
   ```

1. Verify the fingerprint\. Be sure to replace *key\-value* with the value from the preceding step\. We recommend that you use GPG to verify the fingerprint even if you use RPM to verify the installer package\.

   ```
   gpg --fingerprint key-value
   ```

   This command returns output similar to the following\.

   ```
   pub   2048R/56BAA549 2022-03-15 [expires: 2023-09-05]
       Key fingerprint = 2BC7 C7C2 67BB D505 EAA4  91E6 DD81 A617 56BA A549
   uid                  SSM Agent <ssm-agent-signer@amazon.com>
   ```

   The fingerprint should match the following\.

   `2BC7 C7C2 67BB D505 EAA4 91E6 DD81 A617 56BA A549 `

   If the fingerprint doesn't match, don't install the agent\. Contact AWS Support\.

1. Verify the installer package signature\. Be sure to replace the *signature\-filename* and *agent\-download\-filename* with the values you specified when downloading the signature file and agent\.

   ```
   rpm --checksig agent-download-filename
   ```

   This command returns output similar to the following\.

   ```
   amazon-ssm-agent-3.1.1141.0-1.amzn2.x86_64.rpm: rsa sha1 (md5) pgp md5 OK
   ```

   If `pgp` is missing from the output and you have imported the public key, then the agent isn't signed\. If the output contains the phrase `NOT OK (MISSING KEYS: (MD5) key-id)`, check whether you performed the procedure correctly and verify you downloaded SSM Agent version 3\.1\.1141\.0 or later\. If you continue to get this response, contact AWS Support and don't install the agent\.

------