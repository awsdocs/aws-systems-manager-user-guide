# Working with File and Windows Registry Inventory<a name="sysman-inventory-file-and-registry"></a>

Systems Manager Inventory enables you to search and inventory files on Windows and Linux operating systems\. You can also search and inventory the Windows Registry\.

**Files**: You can collect metadata information about files, including file names, the time files were created, the time files were last modified and accessed, and file sizes, to name a few\. To start collecting file inventory, you specify a file path where you want to perform the inventory, one or more patterns that define the types of files you want to inventory, and if the path should be traversed recursively\. Systems Manager inventories all file metadata for files in the specified path that match the pattern\. File inventory uses the following parameter input\.

```
{
"Path": string,
"Pattern": array[string],
"Recursive": true,
"DirScanLimit" : number // Optional
}
```
+ **Path**: The directory path where you want to inventory files\. For Windows, you can use environment variables like %PROGRAMFILES% as long as the variable maps to a single directory path\. For example, if you use %PATH% that maps to multiple directory paths, Inventory throws an error\. 
+ **Pattern**: An array of patterns to identify files\.
+ **Recursive**: A Boolean value indicating whether Inventory should recursively traverse the directories\.
+ **DirScanLimit**: An optional value specifying how many directories to scan\. Use this parameter to minimize performance impact on your instances\. By default, Inventory scans a maximum of 5,000 directories\.

**Note**  
Inventory collects metadata for a maximum of 500 files across all specified paths\.

Here are some examples of how to specify the parameters when performing an inventory of files\.
+ On Linux, collect metadata of \.sh files in the `/home/ec2-user` directory, excluding all subdirectories\.

  ```
  [{"Path":"/home/ec2-user","Pattern":["*.sh", "*.sh"],"Recursive":false}]
  ```
+ On Windows, collect metadata of all "\.exe" files in the Program Files folder, including subdirectories recursively\.

  ```
  [{"Path":"C:\Program Files","Pattern":["*.exe"],"Recursive":true}]
  ```
+ On Windows, collect metadata of specific log patterns\.

  ```
  [{"Path":"C:\ProgramData\Amazon","Pattern":["*amazon*.log"],"Recursive":true}]
  ```
+ Limit the directory count when performing recursive collection\.

  ```
  [{"Path":"C:\Users","Pattern":["*.ps1"],"Recursive":true, "DirScanLimit": 1000}]
  ```

**Windows Registry**: You can collect Windows Registry keys and values\. You can choose a key path and collect all keys and values recursively\. You can also collect a specific registry key and its value for a specific path\. Inventory collects the key path, name, type, and the value\.

```
{
"Path": string, 
"Recursive": boolean,
"ValueNames": array[string] // optional
}
```
+ **Path**: The path to the Registry key\.
+ **Recursive**: A Boolean value indicating whether Inventory should recursively traverse Registry paths\.
+ **ValueNames**: An array of value names for performing inventory of Registry keys\. If you use this parameter, Systems Manager will inventory only the specified value names for the specified path\.

**Note**  
Inventory collects a maximum of 250 Registry key values for all specified paths\.

Here are some examples of how to specify the parameters when performing an inventory of the Windows Registry\.
+ Collect all keys and values recursively for a specific path\.

  ```
  [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon","Recursive": true}]
  ```
+ Collect all keys and values for a specific path \(recursive search disabled\)\.

  ```
  [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Intel\PSIS\PSIS_DECODER", "Recursive": false}]
  ```
+ Collect a specific key by using the `ValueNames` option\.

  ```
  {"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\MachineImage","ValueNames":["AMIName"]}
  ```