# VMware vSphere As Built Report

# Getting Started
Below are the instructions on how to install, configure and generate a VMware vSphere As Built report.

## Pre-requisites
The following PowerShell modules are required for generating a VMware vSphere As Built report.

Each of these modules can be easily downloaded and installed via the PowerShell Gallery 

- [PScribo Module](https://www.powershellgallery.com/packages/PScribo/)
- [VMware PowerCLI Module](https://www.powershellgallery.com/packages/VMware.PowerCLI/)

### Module Installation

Open a Windows PowerShell terminal window and install each of the required modules as follows;
```powershell
install-module PScribo

install-module VMware.PowerCLI

install-module AsBuiltReport
```
## Configuration
The vSphere As Built Report utilises a JSON file to allow configuration of report information, features and section detail. All report settings are configured via the JSON file.

The following provides information of how to configure each schema within the report's JSON file.

### Report
The **Report** sub-schema provides configuration of the vSphere report information

| Schema | Sub-Schema | Description |
| ------ | ---------- | ----------- |
| Report | Name | The name of the As Built Report
| Report | Version | The report version
| Report | Status | The report release status

### Options
The **Options** sub-schema allows certain options within the report to be toggled on or off

| Schema | Sub-Schema | Setting | Description |
| ------ | ---------- | ------- | ----------- |
| Options | ShowLicenses | true / false | Toggle to mask/unmask  vSphere license keys within the As Built report.<br><br> **Masked License Key**<br>\*\*\*\*\*-\*\*\*\*\*-\*\*\*\*\*-56YDM-AS12K<br><br> **Unmasked License Key**<br>AKLU4-PFG8M-W2D8J-56YDM-AS12K

### InfoLevel
The **InfoLevel** sub-schema allows configuration of each section of the report at a granular level. The following sections can be set

| Schema | Sub-Schema | Default Setting |
| ------ | ---------- | --------------- |
| InfoLevel | vCenter | 3
| InfoLevel | ResourcePool | 3
| InfoLevel | Cluster | 3
| InfoLevel | VMhost | 3
| InfoLevel | Network | 3
| InfoLevel | vSAN | 3
| InfoLevel | Datastore | 3
| InfoLevel | DSCluster | 3
| InfoLevel | VM | 3
| InfoLevel | VUM | 3
| InfoLevel | NSX\* | 0
| InfoLevel | SRM\*\* | 0

\* *Requires PowerShell module [PowerNSX](https://github.com/vmware/powernsx) to be installed*

\*\* *Placeholder for future release* 

There are 6 levels (0-5) of detail granularity for each section as follows;

| Setting | InfoLevel | Description |
| ------- | ---- | ----------- |
| 0 | Disabled | does not collect or display any information
| 1 | Summary** | provides summarised information for a collection of objects
| 2 | Informative | provides condensed, detailed information for a collection of objects
| 3 | Detailed | provides detailed information for individual objects
| 4 | Adv Detailed | provides detailed information for individual objects, as well as information for associated objects (Hosts, Clusters, Datastores, VMs etc)
| 5 | Comprehensive | provides comprehensive information for individual objects, such as advanced configuration settings

\*\* *Placeholder for future release*

### Healthcheck
The **Healthcheck** sub-schema is used to toggle health checks on or off.

#### vCenter
The **vCenter** sub-schema is used to configure health checks for vCenter Server.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| vCenter | Mail | true / false | Highlights mail settings which are not configured | ![Critical](https://placehold.it/15/FFB38F/000000?text=+) Not Configured 
| vCenter | Licensing | true / false | Highlights product evaluation licenses | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Product evaluation license in use

#### Cluster
The **Cluster** sub-schema is used to configure health checks for vSphere Clusters.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| Cluster | HAEnabled | true / false | Highlights vSphere Clusters which do not have vSphere HA enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA disabled
| Cluster | HAAdmissionControl | true / false | Highlights vSphere Clusters which do not have vSphere HA Admission Control enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA Admission Control disabled
| Cluster | HostFailureResponse | true / false | Highlights vSphere Clusters which have vSphere HA Failure Response set to disabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA Host Failure Response disabled
| Cluster | HostMonitoring | true / false | Highlights vSphere Clusters which do not have vSphere HA Host Monitoring enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA Host Monitoring disabled
| Cluster | DatastoreOnPDL | true / false | Highlights vSphere Clusters which do not have Datastore on PDL enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA Datastore on PDL disabled
| Cluster | DatastoreOnAPD | true / false | Highlights vSphere Clusters which do not have Datastore on APD enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere HA Datastore on APD disabled
| Cluster | APDTimeOut | true / false | Highlights vSphere Clusters which do not have APDTimeOut enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) APDTimeOut disabled
| Cluster | vmMonitoing | true / false | Highlights vSphere Clusters which do not have VM Monitoting enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) VM Monitoring disabled
| Cluster | DRSEnabled | true / false | Highlights vSphere Clusters which do not have vSphere DRS enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere DRS disabled
| Cluster | DRSAutomationLevelFullyAuto | true / false | Checks the vSphere DRS Automation Level is set to 'Fully Automated' | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere DRS Automation Level not set to 'Fully Automated'
| Cluster | PredictiveDRS | true / false | Highlights vSphere Clusters which do not have Predictive DRS enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Predictive DRS disabled
| Cluster | DRSVMHostRules | true / false | Highlights DRS VMHost rules which are disabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) DRS VMHost rule disabled
| Cluster | DRSRules | true / false | Highlights DRS rules which are disabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) DRS rule disabled
| Cluster | VsanEnabled | true / false | Highlights vSphere Clusters which do not have Virtual SAN enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Virtual SAN disabled
| Cluster | EVCEnabled | true / false | Highlights vSphere Clusters which do not have Enhanced vMotion Compatibility (EVC) enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) vSphere EVC disabled
| Cluster | VUMCompliance | true / false | Highlights vSphere Clusters which do not comply with VMware Update Manager baselines | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Unknown<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+)  Not Compliant

#### VMHost
The **VMHost** sub-schema is used to configure health checks for VMHosts.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| VMhost | ConnectionState | true / false | Highlights VMHosts connection state | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Maintenance<br>  ![Critical](https://placehold.it/15/FFB38F/000000?text=+)  Disconnected
| VMhost | HyperThreading | true / false | Highlights VMHosts which have HyperThreading disabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) HyperThreading disabled<br> 
| VMhost | ScratchLocation | true / false | Highlights VMHosts which are configured with the default scratch location | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Scratch location is /tmp/scratch
| VMhost | IPv6Enabled | true / false | Highlights VMHosts which do not have IPv6 enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) IPv6 disabled
| VMhost | UpTimeDays | true / false | Highlights VMHosts with uptime days greater than 9 months | ![Warning](https://placehold.it/15/FFE860/000000?text=+) 9 - 12 months<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+)  >12 months
| VMhost | Licensing | true / false | Highlights VMHosts which are using production evaluation licenses | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Product evaluation license in use
| VMhost | Services | true / false | Highlights status of important VMHost services | ![Warning](https://placehold.it/15/FFE860/000000?text=+) TSM / TSM-SSH service enabled
| VMhost | TimeConfig | true / false | Highlights if the NTP service has stopped on a VMHost | ![Critical](https://placehold.it/15/FFB38F/000000?text=+)  NTP service stopped
| VMhost | LockdownMode | true / false | Highlights VMHosts which do not have Lockdown mode enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Lockdown Mode disabled<br>
| VMhost | VUMCompliance | true / false | Highlights VMHosts which are not compliant with VMware Update Manager software packages | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Unknown<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+)  Incompatible

#### vSAN
The **vSAN** sub-schema is used to configure health checks for vSAN.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| vSAN | CapacityUtilization | true / false | Highlights vSAN datastores with storage capacity utilization over 75% | ![Warning](https://placehold.it/15/FFE860/000000?text=+) 75 - 90% utilized<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+) >90% utilized

#### Datastore
The **Datastore** sub-schema is used to configure health checks for Datastores.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| Datastore | CapacityUtilization | true / false | Highlights datastores with storage capacity utilization over 75% | ![Warning](https://placehold.it/15/FFE860/000000?text=+) 75 - 90% utilized<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+) >90% utilized

#### DSCluster
The **DSCluster** sub-schema is used to configure health checks for Datastore Clusters.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| DSCluster | CapacityUtilization | true / false | Highlights datastore clusters with storage capacity utilization over 75% | ![Warning](https://placehold.it/15/FFE860/000000?text=+) 75 - 90% utilized<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+) >90% utilized
| DSCluster | SDRSAutomationLevelFullyAuto | true / false | Checks the Datastore Cluster SDRS Automation Level is set to 'Fully Automated' | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Storage DRS Automation Level not set to 'Fully Automated'

#### VM
The **VM** sub-schema is used to configure health checks for virtual machines.

| Schema | Sub-Schema | Setting | Description | Highlight |
| ------ | ---------- | ------- | ----------- | --------- |
| VM | PoweredOn | true / false | Enables/Disables checking if the VM is powered on | ![Warning](https://placehold.it/15/FFE860/000000?text=+) VM is powered off
| VM | CpuHotAddEnabled | true / false | Highlights virtual machines which have CPU Hot Add enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) CPU Hot Add enabled
| VM | CpuHotRemoveEnabled | true / false | Highlights virtual machines which have CPU Hot Remove enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) CPU Hot Remove enabled
| VM | MemoryHotAddEnabled | true / false | Highlights VMs which have Memory Hot Add enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Memory Hot Add enabled
| VM | ChangeBlockTrackingEnabled | true / false | Highlights VMs which do not have Change Block Tracking enabled | ![Warning](https://placehold.it/15/FFE860/000000?text=+) Change Block Tracking disabled
| VM | SpbmPolicyCompliance | true / false | Highlights VMs which do not comply with storage based policies | ![Warning](https://placehold.it/15/FFE860/000000?text=+) VM storage based policy compliance is unknown<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+) VM does not comply with storage based policies
| VM | VMToolsOK | true / false | Highlights Virtual Machines which do not have VM Tools installed, are out of date or are not running | ![Warning](https://placehold.it/15/FFE860/000000?text=+) VM Tools not installed, out of date or not running
| VM | VMSnapshots | true / false | Highlights Virtual Machines which have snapshots older than 7 days | ![Warning](https://placehold.it/15/FFE860/000000?text=+) VM Snapshot age >= 7 days<br> ![Critical](https://placehold.it/15/FFB38F/000000?text=+) VM Snapshot age >= 14 days


## Examples 
- Generate HTML & Word reports with Timestamp
Generate a vSphere As Built Report for vCenter Server 'vcenter-01.corp.local' using specified credentials. Export report to HTML & DOC formats. Use default report style. Append timestamp to report filename. Save reports to 'C:\Users\Tim\Documents'
```powershell
New-AsBuiltReport -Target 'vcenter-01.corp.local' -Username 'administrator@vsphere.local' -Password 'VMware1!' -Report VMware.vSphere -Format Html,Word -OutputPath 'C:\Users\Tim\Documents' -Timestamp
```
- Generate HTML & Text reports with Health Checks
Generate a vSphere As Built Report for vCenter Server 'vcenter-01.corp.local' using stored credentials. Export report to HTML & Text formats. Use default report style. Highlight environment issues within the report. Save reports to 'C:\Users\Tim\Documents'
```powershell
New-AsBuiltReport -Target 'vcenter-01.corp.local' -Credentials $Creds -Report VMware.vSphere -Format Html,Text -OutputPath 'C:\Users\Tim\Documents' -EnableHealthCheck
```
- Generate report with multiple vCenter Servers using Custom Style
Generate a single vSphere As Built Report for vCenter Servers 'vcenter-01.corp.local' and 'vcenter-02.corp.local' using specified credentials. Report exports to WORD format by default. Apply custom style to the report. Reports are saved to the user profile folder by default.
```powershell
New-AsBuiltReport -Target vcenter-01.corp.local,vcenter-02.corp.local -Username 'administrator@vsphere.local' -Password 'VMware1!' -Report Vmware.vSphere -StylePath C:\Scripts\Styles\MyCustomStyle.ps1
```
- Generate HTML & Word reports, attach and send reports via e-mail
Generate a vSphere As Built Report for vCenter Server 'vcenter-01.corp.local' using specified credentials. Export report to HTML & DOC formats. Use default report style. Reports are saved to the user profile folder by default. Attach and send reports via e-mail.
```powershell
New-AsBuiltReport -Target vcenter-01.corp.local -Username 'administrator@vsphere.local' -Password 'VMware1!' -Report Vmware.vSphere -Format Html,Word -OutputPath C:\Users\Tim\Documents -SendEmail
```
# Release Notes
## [0.3.1] - 2019-03-08
### Changed
- Modified for PS module
- Updated default VMware style sheet to include page orientation

## [0.3.0] - 2019-02-01
### Added
- Added Cluster VM Overrides section

### Changed
- Improvements to code structure & readability
- Improvements to output formatting
- Improvements to vSphere HA/DRS Cluster reporting and health checks
- Improvements to VM reporting and health checks
- Corrected sorting of numerous table entries
- Corrected VMHost & VM uptime calculations
- Corrected display of 3rd party Multipath Policy plugins
- Corrected vSAN type & disk count
- Updated Get-Uptime & Get-License functions

## [0.2.2] - 2018-09-19
### Added
- Added new VM health checks for CPU Hot Add/Remove, Memory Hot Add & Change Block Tracking
- Improvements to VM reporting for Guest OS, CPU Hot Add/Remove, Memory Hot Add & Change Block Tracking
- Minor updates to section paragraph text

## 0.2.1
### What's New
- Added SDRS VM Overrides to Datastore Cluster section
- SCSI LUN section rewritten to improve script performance
- Fixed issues with current working directory paths
- Changes to InfoLevel settings and definitions
- Script formatting improvements to some sections to align with PowerShell best practice guidelines
- vCenter Server SSL Certificate section removed temporarily 

## 0.2.0
### What's New
- Requires PScribo module 0.7.24
- Added regions/endregions to all sections of script
- Formatting improvements
- Added Resource Pool summary information
- Added vSAN summary information
- Added vCenter Server mail settings health check
- Datastore Clusters now has it's own dedicated section
- Added DSCluster health checks
- Added VM Power State health check
- Renamed Storage section to Datastores
- Renamed Storage health checks section to Datastore
- Added support for NSX-V reporting

### Known Issues
- Verbose script errors when connecting to vCenter with a Read-Only user account

- In HTML documents, word-wrap of table cell contents is not working, causing the following issues;
  - Cell contents may overflow table columns
  - Tables may overflow page margin
  - [PScribo Issue #83](https://github.com/iainbrighton/PScribo/issues/83)

- In Word documents, some tables are not sized proportionately. To prevent cell overflow issues in HTML documents, most tables are auto-sized, this causes some tables to be out of proportion.
    
    - [PScribo Issue #83](https://github.com/iainbrighton/PScribo/issues/83)