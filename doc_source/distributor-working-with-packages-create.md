# Create a Package<a name="distributor-working-with-packages-create"></a>

To create a package, prepare your installable software or assets, one file per operating system platform\. At least one file is required to create a package\.

Different platforms might sometimes use the same file, but all files that you attach to your package must be listed in the `Files` section of the manifest\. If you are creating a package by using the simple workflow in the console, the manifest is generated for you\. The maximum number of files that you can attach to a single document is 20\. The maximum size of each file is 1 GB\. For more information about supported platforms, see [Supported Package Platforms and Architectures](distributor.md#what-is-a-package-platforms)\.

When you create a package, you are adding a new [SSM document](sysman-ssm-docs.md)\. The document lets you deploy the package to managed instances\.

An example package, [ExamplePackage\.zip](samples/ExamplePackage.zip), is available for you to download from our website\. The example package includes a completed JSON manifest and three ZIP files\. Although you must zip each software installable and scripts into a ZIP file to create a package in the **Advanced** workflow, you do not zip installable assets in the **Simple** workflow\.

**Topics**
+ [Create a Package \(Simple\)](#distributor-working-with-packages-create-simple)
+ [Create a Package \(Advanced\)](#distributor-working-with-packages-create-adv)

## Create a Package \(Simple\)<a name="distributor-working-with-packages-create-simple"></a>

This section describes how to create a package in Distributor by choosing the **Simple** package creation workflow in the Distributor console\. To create a package, prepare your installable assets, one file per operating system platform\. At least one file is required to create a package\. The **Simple** package creation process generates installation and uninstallation scripts, file hashes, and a JSON\-formatted manifest for you\. The **Simple** workflow handles the process of uploading and zipping your installable files, and creating a new package and associated [SSM document](sysman-ssm-docs.md)\. For more information about supported platforms, see [Supported Package Platforms and Architectures](distributor.md#what-is-a-package-platforms)\.

**To create a package \(simple\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose **Create package**, and then choose **Simple**\.

1. On the **Create package** page, enter a name for your package\. Package names can contain letters, numbers, periods, dashes, and underscores\. The name should be generic enough to apply to all versions of the package attachments, but specific enough to identify the purpose of the package\.

1. In **Version name**, enter a version name\. Version names can be a maximum of 512 characters, and cannot contain special characters\.

1. For **S3 bucket name**, choose an existing S3 bucket from the list\.

1. In **S3 key prefix**, enter the subfolder of the bucket where your installable assets are stored\.

1. In **Upload software**, browse for installable software files with a suffix of `.rpm`, `.msi`, or `.deb`\. You can upload more than one software file in a single action\.

1. For **Target platform**, verify that the target operating system platform shown for each installable file is correct\. If the operating system shown is not correct, choose the correct operating system from the drop\-down list\.

   In the **Simple** package creation workflow, because you upload each installable file only once, extra steps are required to instruct Distributor to target a single file at multiple operating systems\. For example, if you upload an installable software file named `Logtool_v1.1.1.rpm`, you must change some defaults in the **Simple** workflow to target the same software at both Amazon Linux and Ubuntu operating systems\. You can do one of the following to work around this limitation\.
   + Use the **Advanced** workflow instead, zip each installable file into a ZIP file before you begin, and manually author the manifest so that one installable file can be targeted at multiple operating system platforms or versions\. For more information, see [Create a Package \(Advanced\)](#distributor-working-with-packages-create-adv)\.
   + Manually edit the manifest file in the **Simple** workflow so that your ZIP file is targeted at multiple operating system platforms or versions\. For more information about how to do this, see the end of step 4 in [Step 2: Create the JSON Package Manifest](#packages-manifest)\.

1. For **Platform version**, verify that the operating system platform version shown is either **\_any**, or the exact, specific operating system release version to which you want your software to apply\. For more information about specifying an operating system platform version, see step 4 in [Step 2: Create the JSON Package Manifest](#packages-manifest)\.

1. For **Architecture**, choose the correct processor architecture for each installable file from the drop\-down list\. For more information about supported processor architectures, see [Supported Package Platforms and Architectures](distributor.md#what-is-a-package-platforms)\.

1. \(Optional\) Expand **Installation and uninstallation scripts**, and review the scripts that Distributor generates for your installable software\.

1. To add more installable software files, choose **Add software**\. Otherwise, go to the next step\.

1. \(Optional\) Expand **Manifest**, and review the JSON package manifest that Distributor generates for your installable software\. If you changed any information about your software since you began this procedure, such as platform version or target platform, choose **Generate manifest** to show the updated package manifest\.

   You can edit the manifest manually if you want to target a software installable at more than one operating system, as described in step 8\. For more information about editing the manifest, see [Step 2: Create the JSON Package Manifest](#packages-manifest)\.

1. When you finish adding software and reviewing target operating system platform, version, and processor architecture data, choose **Upload software and create package**\.

1. Wait for Distributor to finish uploading your software and creating your package\. Distributor shows upload status for each installable file\. Depending on the number and size of packages you are adding, this can take a few minutes\. Distributor automatically redirects you to the **Package details** page for the new package, but you can choose to open this page yourself after the software is uploaded\. The **Package details** page does not show all information about your package until Distributor finishes the package creation process\. To stop the upload and package creation process, choose **Cancel**\.

1. If Distributor cannot upload any of the software installable files, it displays an **Upload failed** message\. To retry the upload, choose **Retry upload**\. For more information about how to troubleshoot package creation failures, see [Troubleshooting AWS Systems Manager Distributor](distributor-troubleshooting.md)\.

## Create a Package \(Advanced\)<a name="distributor-working-with-packages-create-adv"></a>

In this section, learn about how advanced users can create a package in Distributor after uploading installable assets zipped with installation and uninstallation scripts, and a JSON manifest file, to an Amazon S3 bucket\.

To create a package, prepare your ZIP files of installable assets, one ZIP file per operating system platform\. At least one ZIP file is required to create a package\. Next, create a JSON manifest\. The manifest includes pointers to your package code files\. When you have your required code files added to a folder, and the manifest is populated with correct values, upload your package to an Amazon S3 bucket\.

An example package, [ExamplePackage\.zip](samples/ExamplePackage.zip), is available for you to download from our website\. The example package includes a completed JSON manifest and three ZIP files\.

**Topics**
+ [Step 1: Create the ZIP Files](#packages-zip)
+ [Step 2: Create the JSON Package Manifest](#packages-manifest)
+ [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)
+ [Step 4: Add a Package to Distributor](#distributor-working-with-packages-add)

### Step 1: Create the ZIP Files<a name="packages-zip"></a>

The foundation of your package is at least one ZIP file of software or installable assets\. A package includes one ZIP file per operating system that you want to support, unless one ZIP file can be installed on multiple operating systems\. For example, Red Hat Enterprise Linux and Amazon Linux instances can typically run the same \.RPM executable files, so you need to attach only one ZIP file to your package to support both operating systems\.

The following items are required in each ZIP file:
+ An install and an uninstall script\. Windows\-based instances require PowerShell scripts \(scripts named `install.ps1` and `uninstall.ps1`\)\. Linux\-based instances require shell scripts \(scripts named `install.sh` and `uninstall.sh`\)\. SSM Agent runs the instructions in the install and uninstall scripts\.

   For example, your installation scripts might run an installer, like an RPM or MSI, they might copy files, or set configuration settings\.
+ An executable file, installer packages \(RPM, DEB, MSI, etc\.\), other scripts, or configuration files, etc\.

For examples of ZIP files, including sample install and uninstall scripts, download the example package, [ExamplePackage\.zip](samples/ExamplePackage.zip)\.

### Step 2: Create the JSON Package Manifest<a name="packages-manifest"></a>

After you prepare and zip your installable files, create a JSON manifest\. The following is a template\. The parts of the manifest template are described in the procedure in this section\. You can use a JSON editor to create this manifest in a separate file\. Alternatively, you can author the manifest in the AWS Systems Manager console when you create a package\.

```
{
  "schemaVersion": "2.0",
  "version": "your-version",
  "publisher": "optional-publisher-name",
  "packages": {
    "platform": {
      "platform-version": {
        "architecture": {
          "file": "ZIP-file-name-1.zip"
        }
      }
    },
    "another-platform": {
      "platform-version": {
        "architecture": {
          "file": "ZIP-file-name-2.zip"
        }
      }
    },
    "another-platform": {
      "platform-version": {
        "architecture": {
          "file": "ZIP-file-name-3.zip"
        }
      }
    }
  },
  "files": {
    "ZIP-file-name-1.zip": {
      "checksums": {
        "sha256": "checksum"
      }
    },
    "ZIP-file-name-2.zip": {
      "checksums": {
        "sha256": "checksum"
      }
    }
  }
}
```

**To create a JSON package manifest**

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

1. Add packages\. The `"packages"` section describes the platforms, release versions, and architectures supported by the ZIP files in your package\. For more information, see [Supported Package Platforms and Architectures](distributor.md#what-is-a-package-platforms)\.

   The *platform\-version* can be the wildcard value, `_any`\. Use it to indicate that a ZIP file supports any release of the platform\. However, a *platform\-version* value must match the exact release version of the operating system AMI that you are targeting\. The following are suggested resources for getting the correct value of the operating system\.
   + On a Windows\-based instance, the release version is available as Windows Management Instrumentation \(WMI\) data\. You can run the following Command Prompt command on a Windows\-based instance to get version information, then parse the results for `version`\. This command does not show the version for Windows Server Nano; the version value for Windows Server Nano is `nano`\.

     ```
     wmic OS get /format:list
     ```
   + On a Linux\-based instance, get the version by first scanning for operating system release \(the following command\)\. Look for the value of `VERSION_ID`\.

     ```
     cat /etc/os-release
     ```

     If that does not return the results that you need, run the following command to get LSB release information from the `/etc/lsb-release` file, and look for the value of `DISTRIB_RELEASE`\.

     ```
     lsb_release -a
     ```

     If these methods fail, you can usually find the release based on the distribution\. For example, on Debian, you can scan the `/etc/debian_version` file, or on Red Hat Enterprise Linux, the `/etc/redhat-release` file\.

     ```
     hostnamectl
     ```

   ```
   "packages": {
       "platform": {
         "platform-version": {
           "architecture": {
             "file": "ZIP-file-name-1.zip"
           }
         }
       },
       "another-platform": {
         "platform-version": {
           "architecture": {
             "file": "ZIP-file-name-2.zip"
           }
         }
       },
       "another-platform": {
         "platform-version": {
           "architecture": {
             "file": "ZIP-file-name-3.zip"
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

   You can add the `_any` wildcard value to indicate that the package supports all versions of the parent element\. For example, to indicate that the package is supported on any release version of Amazon Linux, your package statement should be similar to the following\. You can use the `_any` wildcard at the version or architecture levels to support all versions of a platform, or all architectures in a version, or all versions and all architectures of a platform\.

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

   You can refer to a ZIP file more than once in the `"packages"` section of the manifest, if the ZIP file supports more than one platform\. For example, if you have a ZIP file that supports both Red Hat Enterprise Linux and Amazon Linux, you have two entries in the `"packages"` section that point to the same ZIP file, as shown in the following example\.

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

1. Add the list of ZIP files that are part of this package from step 4\. Each file entry requires the file name and `sha256` hash value checksum\. Checksum values in the manifest must match the `sha256` hash value in the zipped assets to prevent the package installation from failing\.

   To get the exact checksum from your installables, you can run the following commands\. On Linux, run `cat file-name.zip | openssl dgst -sha256`\. On Windows, run the `Get-FileHash -Path path-to-ZIP-file` cmdlet in [PowerShell](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-6)\.

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

1. After you add your package information, save and close the manifest file\.

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

#### Package Example<a name="package-manifest-examples"></a>

An example package, [ExamplePackage\.zip](samples/ExamplePackage.zip), is available for you to download from our website\. The example package includes a completed JSON manifest and three ZIP files\.

### Step 3: Upload the Package and Manifest to an Amazon S3 Bucket<a name="packages-upload-s3"></a>

Prepare your package by copying or moving all ZIP files into a folder or directory\. A valid package requires the manifest that you created in [Step 2: Create the JSON Package Manifest](#packages-manifest) and all ZIP files identified in the manifest file list\.

**To upload the package and manifest to Amazon S3**

1. Copy or move all ZIP archive files that you specified in the manifest to a folder or directory\.

1. Create a bucket or choose an existing bucket\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. For more information about how to run an AWS CLI command to create a bucket, see [https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html) in the *AWS CLI Command Reference*\.

1. Upload the folder to the bucket\. For more information, see [Add an Object to a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. If you plan to paste your JSON manifest into the AWS Systems Manager console, do not upload the manifest\. For more information about how to run an AWS CLI command to upload files to a bucket, see [https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html) in the *AWS CLI Command Reference*\.

1. On the bucket's home page, choose the folder that you uploaded\. If you uploaded your files to a subfolder in a bucket, be sure to note the subfolder \(also known as a *prefix*\)\. You need the prefix to add your package to Distributor\.

### Step 4: Add a Package to Distributor<a name="distributor-working-with-packages-add"></a>

You can use the AWS Systems Manager console or the AWS CLI to add a new package to AWS Systems Manager Distributor\. When you add a package, you are adding a new [SSM document](sysman-ssm-docs.md)\. The document lets you deploy the package to managed instances\.

**Topics**
+ [Adding a Package \(Console\)](#create-pkg-console)
+ [Adding a Package \(AWS CLI\)](#create-pkg-cli)

#### Adding a Package \(Console\)<a name="create-pkg-console"></a>

You can use the AWS Systems Manager console to create a package\. Have ready the name of the bucket to which you uploaded your package in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)\.

**To add a package to Distributor \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Distributor**\.

1. On the Distributor home page, choose **Create package**, and then choose **Advanced**\.

1. On the **Create package** page, enter a name for your package\. Package names can contain letters, numbers, periods, dashes, and underscores\. The name should be generic enough to apply to all versions of the package attachments, but specific enough to identify the purpose of the package\.

1. In **Version name**, enter the exact value of the `version` entry in your manifest file\.

1. For **S3 bucket name**, choose the name of the bucket to which you uploaded your ZIP files and manifest in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)\.

1. In **S3 key prefix**, enter the subfolder of the bucket where your ZIP files and manifest are stored\.

1. In **Manifest**, choose **Extract from package** to use a manifest that you have uploaded to the S3 bucket with your ZIP files\.

   \(Optional\) If you did not upload your JSON manifest to the S3 bucket where you stored your ZIP files, choose **New manifest**\. You can author or paste the entire manifest in the JSON editor field\. For more information about how to create the JSON manifest, see [Step 2: Create the JSON Package Manifest](#packages-manifest)\.

1. When you are finished with the manifest, choose **Create package**\.

1. Wait for Distributor to create your package from your ZIP files and manifest\. Depending on the number and size of packages you are adding, this can take a few minutes\. Distributor automatically redirects you to the **Package details** page for the new package, but you can choose to open this page yourself after the software is uploaded\. The **Package details** page does not show all information about your package until Distributor finishes the package creation process\. To stop the upload and package creation process, choose **Cancel**\.

#### Adding a Package \(AWS CLI\)<a name="create-pkg-cli"></a>

You can use the AWS CLI to create a package\. Have the URL ready from the bucket to which you uploaded your package in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)\.

**To add a package to Amazon S3 \(AWS CLI\)**

1. To use the AWS CLI to create a package, run the following command, replacing *package\-name* with the name of your package and *S3\-bucket\-URL\-to\-manifest\-file* with the URL of the JSON manifest that you copied in [Step 3: Upload the Package and Manifest to an Amazon S3 Bucket](#packages-upload-s3)\. *S3\-bucket\-URL\-of\-package* is the URL of the S3 bucket where the entire package is stored\. When you run the create\-document command in Distributor, you specify the `Package` value for `--document-type`\.

   If you did not add your manifest file to the S3 bucket, the `--content` parameter value is the entire content of the JSON manifest file, in quotations\.

   ```
   aws ssm create-document --name "package-name" --content "S3-bucket-URL-to-manifest-file" --attachments Key="SourceUrl",Values="S3-bucket-URL-of-package" --version-name version-value-from-manifest --document-type Package
   ```

   The following is an example\.

   ```
   aws ssm create-document --name ExamplePackage --content "https://s3.amazonaws.com/mybucket/ExamplePackage/manifest.json" --attachments Key="SourceUrl",Values="https://s3.amazonaws.com/mybucket/ExamplePackage" --version-name 1.0.1 --document-type Package
   ```

1. Verify that your package was added and show the package manifest by running the following command, replacing *package\-name* with the name of your package\. To get a specific version of the document \(not the same as the version of a package\), you can add the `--document-version` parameter\.

   ```
   aws ssm get-document --name "package-name"
   ```

For information about other options you can use with the create\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/create-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-document.html) in the *AWS Systems Manager section of the AWS CLI Command Reference*\. For information about other options you can use with the get\-document command, see [https://docs.aws.amazon.com/cli/latest/reference/ssm/get-document.html](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-document.html)\.