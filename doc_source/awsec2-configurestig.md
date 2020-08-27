# AWSEC2\-ConfigureSTIG<a name="awsec2-configurestig"></a>

Security Technical Implementation Guides \(STIGs\) are the configuration standards created by the Defense Information Systems Agency \(DISA\) to secure information systems and software\. To make your systems compliant with STIG standards, you must install, configure, and test a variety of security settings\. 

Amazon EC2 provides an SSM document, AWSEC2\-ConfigureSTIG, to apply STIG to an instance to help you quickly build compliant images for STIG standards\. The STIG SSM document scans for misconfigurations and runs a remediation script\. The STIG SSM document installs InstallRoot on Windows AMIs from the Department of Defense \(DoD\) to install and update the DoD certificates and remove unnecessary certificates to maintain STIG compliance\. There are no additional charges for using the STIG SSM document\. 

You can choose the STIG compliance category to apply\. 

**Compliance levels**
+ **High \(Category I\) **

  The most severe risk and includes any vulnerability that can result in loss of confidentiality, availability, or integrity\.
+ **Medium \(Category II\) **

  Any vulnerability that could result in loss of confidentiality, availability, or integrity\. These risks could be mitigated\.
+ **Low \(Category III\) **

  Any vulnerability that degrades measures to protect against loss of confidentiality, availability, or integrity\.

**Topics**
+ [Windows STIG settings](#base-os-stig)
+ [Linux STIG settings](#ie-os-stig)

## Windows STIG settings<a name="base-os-stig"></a>

Windows STIG components are designed for standalone servers and apply Local Group Policy\. You can apply low, medium, or high STIG settings\.

### STIG\-Build\-Windows\-Low Version<a name="ib-windows-stig-low"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\.
+ **Windows Server 2019 STIG V1 Release 3:**

  V\-93149, V\-93187, V\-93229, and V\-93231 
+ **Windows Server 2016 STIG V1 Release 11:**

  V\-73307, V\-73649, V\-90355, and V\-90357 
+ **Windows Server 2012R2 STIG V2 Release 18:**

  V\-1076, V\-1112, V\-3472, V\-4445, V\-26359, V\-36678, V\-36733, V\-40172, and V\-40173 
+ **Microsoft \.NET Framework STIG 4\.0 V1 Release 9:**

  V\-30937 and V\-30972
+ **Windows Firewall STIG V1 Release 7:**

  All STIG settings applied\.
+ **Internet Explorer 11 STIG V1 Release 14:**

  All STIG settings applied\.

### STIG\-Build\-Windows\-Medium Version<a name="ib-windows-stig-medium"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\.
+ **Windows Server 2019 STIG V1 Release 3**

  V\-92975, V\-92977, V\-93047, V\-93049, V\-93061, V\-93077, V\-93147, V\-93149, V\-93183, V\-93185, V\-93187, V\-93203, V\-93209, V\-93219, V\-93221, V\-93227, V\-93229, V\-93231, V\-93281, V\-93283, V\-93339, V\-93379, V\-93381, V\-93437, V\-93439, V\-93457, V\-93461, V\-93473, V\-93475, V\-93511, V\-93515, V\-93543, V\-93567, and V\-93571
+ **Windows Server 2016 STIG V1 Release 12**

  V\-73223, V\-73229, V\-73231, V\-73233, V\-73235, V\-73245, V\-73259, V\-73261, V\-73263, V\-73265, V\-73273, V\-73275, V\-73277, V\-73279, V\-73281, V\-73283, V\-73285, V\-73307, V\-73401, V\-73403, V\-73623, V\-73625, V\-73647, V\-73649, V\-73701, V\-73729, V\-73751, V\-73779, V\-73791, V\-78127, V\-90355, and V\-90357 
+ **Windows Server 2012R2 STIG V2 Release 18**

  : V\-1072, V\-1076, V\-1089, V\-1112, V\-1114, V\-1115, V\-1145, V\-2907, V\-3289, V\-3383, V\-3472, V\-3487, V\-4445, V\-6840, V\-14225, V\-15505, V\-26359, V\-26469, V\-26481, V\-26484, V\-26487, V\-26494, V\-36658, V\-36661, V\-36662, V\-36666, V\-36670, V\-36671, V\-36672, V\-36678, V\-36733, V\-36734, V\-36735, V\-36736, V\-40172, V\-40173, V\-42420, V\-57637, V\-57641, V\-57645, V\-57653, V\-57655, V\-57719, and V\-75915
+ **Microsoft \.NET Framework STIG 4\.0 V1 Release 9**

  V\-7055, V\-7061, V\-7063, V\-7067, V\-7069, V\-7070, V\-18395, V\-30926, V\-30935, V\-30937, V\-30968, V\-30972, V\-30986, V\-31026, and V\-32025
+ **Windows Firewall STIG V1 Release 7**

  All STIG settings applied\.
+ **Internet Explorer 11 STIG V1 Release 14**

  All STIG settings applied\.

### STIG\-Build\-Windows\-High Version<a name="ib-windows-stig-high"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For a complete list, see the [STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=windows)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\.
+ **Windows Server 2019 STIG V1 Release 3**

  V\-92975, V\-92977, V\-93047, V\-93049, V\-93051, V\-93057, V\-93061, V\-93077, V\-93147, V\-93149, V\-93183, V\-93185, V\-93187, V\-93203, V\-93205, V\-93209, V\-93217, V\-93219, V\-93221, V\-93227, V\-93229, V\-93231, V\-93281, V\-93283, V\-93339, V\-93369, V\-93379, V\-93381, V\-93437, V\-93439, V\-93457, V\-93461, V\-93473, V\-93475, V\-93511, V\-93515, V\-93543, V\-93567, and V\-93571
+ **Windows Server 2016 STIG V1 Release 12**

  V\-73217, V\-73221, V\-73223, V\-73225, V\-73229, V\-73231, V\-73233, V\-73235, V\-73241, V\-73245, V\-73259, V\-73261, V\-73263, V\-73265, V\-73273, V\-73275, V\-73277, V\-73279, V\-73281, V\-73283, V\-73285, V\-73307, V\-73401, V\-73403, V\-73623, V\-73625, V\-73647, V\-73649, V\-73701, V\-73729, V\-73735, V\-73747, V\-73751, V\-73779, V\-73791, V\-78127, V\-90355, and V\-90357
+ **Windows Server 2012R2 STIG V2 Release 18**

  V\-1072, V\-1074, V\-1076, V\-1089, V\-1102, V\-1112, V\-1114, V\-1115, V\-1127, V\-1145, V\-2907, V\-3289, V\-3338, V\-3340, V\-3383, V\-3472, V\-3487, V\-4445, V\-6840, V\-7002, V\-14225, V\-15505, V\-26359, V\-26469, V\-26479, V\-26481, V\-26484, V\-26487, V\-26494, V\-36451, V\-36658, V\-36659, V\-36661, V\-36662, V\-36666, V\-36670, V\-36671, V\-36672, V\-36678, V\-36733, V\-36734, V\-36735, V\-36736, V\-40172, V\-40173, V\-42420, V\-57637, V\-57641, V\-57645, V\-57653, V\-57655, V\-57719, and V\-75915
+ **Microsoft \.NET Framework STIG 4\.0 V1 Release 9**

  V\-7055, V\-7061, V\-7063, V\-7067, V\-7069, V\-7070, V\-18395, V\-30926, V\-30935, V\-30937, V\-30968, V\-30972, V\-30986, V\-31026, and V\-32025
+ **Windows Firewall STIG V1 Release 7**

  All STIG settings applied\.
+ **Internet Explorer 11 STIG V1 Release 14**

  All STIG settings applied\.

## Linux STIG settings<a name="ie-os-stig"></a>

The following sections contain information about Linux STIG components\. You can apply low, medium, or high STIG settings\. 

### STIG\-Build\-Linux\-Low Version<a name="ib-linux-stig-low"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For complete list, see the [ STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\. 

**RHEL 7 STIG V2 Release 7**

V\-72003, V\-72059, V\-72061, V\-72063, V\-72069, V\-72071, V\-72275, V\-72281, V\-81009, V\-81011, and V\-81013

### STIG\-Build\-Linux\-Medium Version<a name="ib-linux-stig-medium"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For complete list, see the [ STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\.

**RHEL 7 STIG V2 Release 7**

V\-71863, V\-71897, V\-71927, V\-71931, V\-71933, V\-71947, V\-71957, V\-71965, V\-71971, V\-71973, V\-71975, V\-71983, V\-71999, V\-72001, V\-72003, V\-72007, V\-72009, V\-72017, V\-72019, V\-72021, V\-72023, V\-72025, V\-72027, V\-72029, V\-72031, V\-72033, V\-72035, V\-72037, V\-72039, V\-72041, V\-72049, V\-72059, V\-72061, V\-72063, V\-72069, V\-72071, V\-72073, V\-72075, V\-72081, V\-72083, V\-72085, V\-72087, V\-72089, V\-72091, V\-72093, V\-72209, V\-72211, V\-72219, V\-72221, V\-72225, V\-72253, V\-72255, V\-72257, V\-72265, V\-72269, V\-72273, V\-72275, V\-72281, V\-72315, V\-72417, V\-72427, V\-72433, V\-73161, V\-73163, V\-73171, V\-81009, V\-81011, V\-81013, V\-81015, V\-81017, and V\-92255

### STIG\-Build\-Linux\-High Version<a name="ib-linux-stig-high"></a>

The following STIG settings have *not* been applied due to organization\-specific policies and/or technical limitations\. All other applicable STIGs have been applied\. For complete list, see the [ STIGs Document Library](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=operating-systems%2Cunix-linux)\. For instructions on how to view the complete list, see [How to View SRGs and STIGs ](https://dl.dod.cyber.mil/wp-content/uploads/stigs/doc/HOW_TO_VIEW_SRGs_and_STIGs.doc)\.

**RHEL 7 STIG V2 Release 7**

V\-71855, V\-71863, V\-71897, V\-71927, V\-71931, V\-71933, V\-71937, V\-71939, V\-71947, V\-71957, V\-71965, V\-71971, V\-71973, V\-71975, V\-71983, V\-71989, V\-71991, V\-71997, V\-71999, V\-72001, V\-72003, V\-72007, V\-72009, V\-72017, V\-72019, V\-72021, V\-72023, V\-72025, V\-72027, V\-72029, V\-72031, V\-72033, V\-72035, V\-72037, V\-72039, V\-72041, V\-72049, V\-72059, V\-72061, V\-72063, V\-72067, V\-72069, V\-72071, V\-72073, V\-72075, V\-72081, V\-72083, V\-72085, V\-72087, V\-72089, V\-72091, V\-72093, V\-72209, V\-72211, V\-72213, V\-72219, V\-72221, V\-72225, V\-72253, V\-72255, V\-72257, V\-72265, V\-72269, V\-72273, V\-72275, V\-72281, V\-72315, V\-72417, V\-72427, V\-72433, V\-73161, V\-73163, V\-73171, V\-81009, V\-81011, V\-81013, V\-81015, V\-81017, V\-92255, and V\-94843