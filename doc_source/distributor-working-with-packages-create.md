# Create a Package<a name="distributor-working-with-packages-create"></a>

To create a package, prepare your ZIP files of installable assets, one ZIP file per operating system platform\. At least one ZIP file is required to create a package\.

Next, create a JSON manifest\. The manifest includes pointers to your package code files\. When you have your required code files added to a folder, and the manifest has been populated with correct values, upload your package to an Amazon S3 bucket\.

Different platforms might sometimes use the same ZIP file, but all ZIP files that you attach to your package must be listed in the `Files` section of the manifest\. The maximum number of ZIP files that you can attach to a single document is 20\. The maximum size of each ZIP file is 1 GB\. For more information about supported platforms, see [Supported Package Platforms and Architectures](what-is-distributor.md#what-is-a-package-platforms)\.

An example package, [ExamplePackage\.zip](samples/ExamplePackage.zip), is available for you to download from our website\. The example package includes a completed JSON manifest and three ZIP files\.

**Topics**
+ [Step 1: Create the ZIP Files](#packages-zip)
+ [Step 2: Create the JSON Package Manifest](#packages-manifest)
+ [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)

## Step 1: Create the ZIP Files<a name="packages-zip"></a>

The foundation of your package is at least one ZIP file of software or installable assets\. A package includes one ZIP file per operating system that you want to support, unless one ZIP file can be installed on multiple operating systems\. For example, Red Hat Enterprise Linux and Amazon Linux instances can typically run the same \.RPM executable files, so you need attach only one ZIP file to your package to support both operating systems\.

The following items are required in each ZIP file\.
+ An install and uninstall script\. Windows\-based instances require PowerShell scripts \(scripts named `install.ps1` and `uninstall.ps1`\)\. Linux\-based instances require shell scripts \(scripts named `install.sh` and `uninstall.sh`\)\. SSM Agent executes the instructions in the install and uninstall scripts\.

   For example, your installation scripts might run an installer, like an RPM or MSI, they might copy files, or set configuration settings\.
+ An executable file, installer packages \(RPM, DEB, MSI, etc\.\), other scripts, or configuration files, etc\.

For examples of ZIP files, including sample install and uninstall scripts, download the example package, [ExamplePackage\.zip](samples/ExamplePackage.zip)\.

## Step 2: Create the JSON Package Manifest<a name="packages-manifest"></a>

After you prepare and ZIP your installable files, create a JSON manifest\. The following is a template\. The parts of the manifest template are described in the procedure in this section\. You can use a JSON editor to create this manifest in a separate file\. Alternatively, you can author the manifest in the AWS Systems Manager console when you create a package\.

```
{
  "schemaVersion": "2.0",
  "version": "your_version",
  "publisher": "optional_publisher_name",
  "packages": {
    "platform": {
      "platform_version": {
        "architecture": {
          "file": "ZIP_file_name1.zip"
        }
      }
    },
    "another_platform": {
      "platform_version": {
        "architecture": {
          "file": "ZIP_file_name2.zip"
        }
      }
    },
    "another_platform": {
      "platform_version": {
        "architecture": {
          "file": "ZIP_file_name3.zip"
        }
      }
    }
  },
  "files": {
    "ZIP_file_name1.zip": {
      "checksums": {
        "sha256": "checksum"
      }
    },
    "ZIP_file_name2.zip": {
      "checksums": {
        "sha256": "checksum"
      }
    }
  }
}
```

1. Add the schema version to your manifest\. In this release, the schema version is always `2.0`\.

   ```
   { "schemaVersion": "2.0",
   ```

1. Add a user\-defined package version to your manifest\. This is also the value of **Version name** that you specify when you add your package to Distributor\. It becomes part of the AWS Systems Manager document that Distributor creates when you add your package\. You also provide this value as an input in the `AWS-ConfigureAWSPackage` document to install a version of the package other than the latest\. A `version` value can contain letters, numbers, underscores, hyphens, and periods, and be a maximum of 128 characters in length\. We recommend that you use a human\-readable package version to make it easier for you and other administrators to specify exact package versions when you deploy\. The following is an example\.

   ```
   "version": "1.0.1",
   ```

1. \(Optional\) Add a publisher name\. The following is an example\.

   ```
   "publisher": "MyOrganization",
   ```

1. Add packages\. The `"packages"` section describes the platforms, release versions, and architectures supported by the ZIP files in your package\. For more information, see [Supported Package Platforms and Architectures](what-is-distributor.md#what-is-a-package-platforms)\.

   The *platform\_version* can be the wildcard value `_any`\. Use it to indicate that a ZIP file supports any release of the platform\. However, a *platform\_version* value must match the exact release version of the operating system AMI that you are targeting\. The following are suggested resources for getting the correct value of the operating system\.
   + On a Windows\-based instance, the release version is available as Windows Management Instrumentation \(WMI\) data\. You can run the following Command Prompt command on a Windows\-based instance to get version information, then parse the results for `version`\. This command does not show the version for Windows Server Nano; the version value for Windows Server Nano is `nano`\.

     ```
     wmic OS get /format:list
     ```
   + On a Linux\-based instance, get the version by first scanning for operating system release \(the first command in the following list\)\. Look for the value of `VERSION_ID`\. If that does not return results you need, run the second command to get LSB release information from the `/etc/lsb-release` file, and look for the value of `DISTRIB_RELEASE`\. If these methods fail, you can usually find the release based on the distribution\. For example, on Debian, you can scan the `/etc/debian_version` file, or on Red Hat Enterprise Linux, the `/etc/redhat-release` file\.

     ```
     cat /etc/os-release
     lsb_release -a
     hostnamectl
     ```

   ```
   "packages": {
       "platform": {
         "platform_version": {
           "architecture": {
             "file": "ZIP_file_name1.zip"
           }
         }
       },
       "another_platform": {
         "platform_version": {
           "architecture": {
             "file": "ZIP_file_name2.zip"
           }
         }
       },
       "another_platform": {
         "platform_version": {
           "architecture": {
             "file": "ZIP_file_name3.zip"
           }
         }
       }
     }
   ```

   The following is an example\. In this example, the operating system platform is `amazon`, the supported release version is `2016.09`, the architecture is `x86_64`, and the ZIP file that supports this platform is `test.zip`\.

   ```
   {
       "amazon": {
           "2016.09": {
               "x86_64": {
                   "file": "test.zip"
               }
           }
       }
   },
   ```

   You can add the wildcard value `_any` to indicate that the package supports all versions of the parent element\. For example, to indicate that the package is supported on any release version of Amazon Linux, your package statement should be similar to the following\. You can use the `_any` wildcard at the version or architecture levels to support all versions of a platform, or all architectures in a version, or all versions and all architectures of a platform\.

   ```
   {
       "amazon": {
           "_any": {
               "x86_64": {
                   "file": "test.zip"
               }
           }
       }
   },
   ```

   The following example adds `_any` to show that the first package, `data1.zip`, is supported for all architectures of Amazon Linux 2016\.09\. The second package, `data2.zip`, is supported for all releases of Amazon Linux, but only for instances with `x86_64` architecture\. Both the `2016.09` and `_any` versions are entries under `amazon`\. There is one platform \(Amazon Linux\), but different supported versions, architectures, and associated ZIP files\.

   ```
   {
       "amazon": {
           "2016.09": {
               "_any": {
                   "file": "data1.zip"
               }
           },
           "_any": {
               "x86_64": {
                   "file": "data2.zip"
               }
           }
       }
   }
   ```

   You can refer to a ZIP file more than once in the `"packages"` section of the manifest, if the ZIP file supports more than one platform\. For example, if you have a ZIP file that supports both Red Hat Enterprise Linux and Amazon Linux, you would have two entries in the `"packages"` section pointing to the same ZIP file, as shown in the following example\.

   ```
   {
       "amazon": {
           "2018.03": {
               "x86_64": {
                   "file": "test.zip"
               }
           }
       },
       "redhat": {
           "_any": {
               "x86_64": {
                   "file": "test.zip"
               }
           }
       }
   },
   ```

1. Add the list of ZIP files that are part of this package from step 4\. Each file entry requires the file name and `sha256` hash value checksum\. Checksum values in the manifest must match the `sha256` hash value in the zipped assets to prevent package installation from failing\.

   To get the exact checksum from your installables, you can run the following commands\. On Linux, run `cat file_name.zip | openssl dgst -sha256`\. On Windows, run the `Get-FileHash -Path path_to_ZIP_file` cmdlet in [PowerShell](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-6)\.

   The `"files"` section of the manifest includes one reference to each of the ZIP files in your package\.

   ```
   "files": {
           "test-agent-x86.deb.zip": {
               "checksums": {
                   "sha256": "EXAMPLE2706223c7616ca9fb28863a233b38e5a23a8c326bb4ae241dcEXAMPLE"
               }
           },
           "test-agent-x86_64.deb.zip": {
               "checksums": {
                   "sha256": "EXAMPLE572a745844618c491045f25ee6aae8a66307ea9bff0e9d1052EXAMPLE"
               }
           },
           "test-agent-x86_64.nano.zip": {
               "checksums": {
                   "sha256": "EXAMPLE63ccb86e830b63dfef46995af6b32b3c52ce72241b5e80c995EXAMPLE"
               }
           },
           "test-agent-rhel5-x86.nano.zip": {
               "checksums": {
                   "sha256": "EXAMPLE13df60aa3219bf117638167e5bae0a55467e947a363fff0a51EXAMPLE"
               }
           },
           "test-agent-x86.msi.zip": {
               "checksums": {
                   "sha256": "EXAMPLE12a4abb10315aa6b8a7384cc9b5ca8ad8e9ced8ef1bf0e5478EXAMPLE"
               }
           },
           "test-agent-x86_64.msi.zip": {
               "checksums": {
                   "sha256": "EXAMPLE63ccb86e830b63dfef46995af6b32b3c52ce72241b5e80c995EXAMPLE"
               }
           },
           "test-agent-rhel5-x86.rpm.zip": {
               "checksums": {
                   "sha256": "EXAMPLE13df60aa3219bf117638167e5bae0a55467e947a363fff0a51EXAMPLE"
               }
           },
           "test-agent-rhel5-x86_64.rpm.zip": {
               "checksums": {
                   "sha256": "EXAMPLE7ce8a2c471a23b5c90761a180fd157ec0469e12ed38a7094d1EXAMPLE"
               }
           }
       }
   ```

1. After you have added your package information, save and close the manifest file\.

The following is an example of a completed manifest\. In this example, you have a ZIP file, `NewPackage_LINUX.zip`, that supports more than one platform, but is referenced in the `"files"` section only once\.

```
{
    "schemaVersion": "2.0",
    "version": "1.7.1",
    "publisher": "Amazon Web Services",
    "packages": {
        "windows": {
            "_any": {
                "x86_64": {
                    "file": "NewPackage_WINDOWS.zip"
                }
            }
        },
        "amazon": {
            "_any": {
                "x86_64": {
                    "file": "NewPackage_LINUX.zip"
                }
            }
        },
        "ubuntu": {
            "_any": {
                "x86_64": {
                    "file": "NewPackage_LINUX.zip"
                }
            }
        }
    },
    "files": {
        "NewPackage_WINDOWS.zip": {
            "checksums": {
                "sha256": "EXAMPLEc2c706013cf8c68163459678f7f6daa9489cd3f91d52799331EXAMPLE"
            }
        },
        "NewPackage_LINUX.zip": {
            "checksums": {
                "sha256": "EXAMPLE2b8b9ed71e86f39f5946e837df0d38aacdd38955b4b18ffa6fEXAMPLE"
            }
        }
    }
}
```

### Package Example<a name="package-manifest-examples"></a>

An example package, [ExamplePackage\.zip](samples/ExamplePackage.zip), is available for you to download from our website\. The example package includes a completed JSON manifest and three ZIP files\.

## Step 3: Upload the Package and Manifest to an Amazon S3 Bucket<a name="packages-upload-s3"></a>

Prepare your package by copying or moving all ZIP files into a folder or directory\. A valid package requires the manifest that you created in [Step 2: Create the JSON Package Manifest](#packages-manifest) and all ZIP files identified in the manifest file list\.

1. Copy or move all ZIP archive files that you specified in the manifest to a folder or directory\.

1. Create a bucket or choose an existing bucket\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. For more information about how to run an AWS CLI command to create a bucket, see [https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html) in the *AWS CLI Command Reference*\.

1. Upload the folder to the bucket\. For more information, see [Add an Object to a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. If you plan to paste your JSON manifest into the AWS Systems Manager console, do not upload the manifest\. For more information about how to run an AWS CLI command to upload files to a bucket, see [https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html) in the *AWS CLI Command Reference*\.

1. On the bucket's home page, choose the folder that you uploaded\.

1. On the uploaded folder's properties pane at right, copy the **Link** URL to the folder, and paste it in a convenient location\. You need this URL to add your package to Distributor\.