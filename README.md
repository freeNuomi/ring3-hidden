## Ring3 Hidden - File, Directory, Service, and Process Concealment Project

### Project Introduction

This project specializes in implementing common Windows user-mode (Ring3) concealment techniques. Its functionalities encompass hiding files, directories, system services, process information, startup items, and more. Some hiding operations require administrative privileges for optimal results, which can be facilitated by pairing with a bypassUAC tool.

#### Key Features

**File/Directory Hiding**: Transparently hides specific files and directories by hooking system APIs, rendering them invisible in standard system tools such as Explorer. This can also be achieved by setting the hidden and system attributes of targeted files or directories using Windows built-in settings, combined with API hooking for better concealment. Hooked APIs include NtQueryDirectoryFile and NtQueryDirectoryFileEx (which may not always need to be hooked). Note that multiple instances of Explorer.exe might exist within the system, requiring careful injection decisions or injecting into all instances of Explorer.exe.

**Service Hiding**: Utilizes Windows' native security mechanisms to hide services at the user-level. This involves leveraging the Windows Access Control List (ACL) mechanism, which provides a more discreet form of hiding compared to hooking EnumServicesStatusExW. The same mechanism can be used to create inaccessible files.

**Process Hiding**: Implements API Hooking during the process creation and enumeration phases to bypass checks, thus achieving stealthy process invisibility. Hooked API: NtQuerySystemInformation. This hook is effective against tools like Process Hacker.

**Startup Item Concealment**: Uses Native APIs for registry read/write operations to hide startup items. This method achieves excellent concealment, making hidden startup items undetectable even by dedicated utilities such as火绒's Startup Management and Sysinternals Autoruns. 

**Module Concealment**: Relies on PEB chain manipulation for module hiding, which makes modules invisible in most tools but is generally unreliable and easily detected. DLL reflective injection is considered a better alternative.


### Technical Summary


| hide | method |
| -------- | -------- |
| hidden process | hook NtQuerySystemInformation |
| Hide self launch items | NtCreateKeyEx     NtDeleteKeyEx |
| hidden services | Using ACL to hide Windows services |
| hidden file | hook ntQueryDirectoryFile    setfileattributes() |
| hidden module | PEB broken chain|


### Installation & Usage

**Development Environment**: Visual Studio 2022 (v143), Win10 22h2.

**Testing**: Inject the compiled DLL into the target process, or load it into the system via a startup entry.

**Configuration**: Provides simple interfaces or configuration files to specify resources to be hidden.

**Testing Caution**: While Ring3 hiding techniques typically do not compromise system stability, for safety and consistent testing, please conduct code tests in a virtual machine environment and observe if the hidden resources are successfully removed from system views.

### Notes

**Legal Statement**: Please strictly adhere to local laws and regulations. Do not use this project for any illegal or unauthorized purposes.

### Contribution Guide

Contributions from all participants are welcome for project optimization, vulnerability fixes, and new feature additions. Follow GitHub's standard procedures to submit pull requests and discuss related issues in the issue section.

### References

- [ntQueryDirectoryFile function (ntifs.h) - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/ntifs/nf-ntifs-ntquerydirectoryfile)
- [NtQuerySystemInformation function (winternl.h) - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntquerysysteminformation)
