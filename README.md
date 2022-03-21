# 1. Three deploy ways
Visual Studio enables three ways to deploy the Visual C++ libraries together with your application: central deployment, local deployment, and static linking. 

## 1.1 Central deployment
In central deployment, library DLL files are installed in the Windows\System32 directory, or for 32-bit library files on x64 systems, the Windows\SysWow64 directory. Microsoft automatically updates its libraries that are centrally deployed. For Visual C++ libraries that are locally deployed or statically linked, you must provide the updates.

## 1.2 local deployment
In local deployment, library files are installed in your application folder together with the executable file. Different versions of Visual C++ redistributable libraries can be installed in the same folder because the file name of each version includes its version number. For example, version 12 of the C++ runtime library is msvcp120.dll, and version 14 is msvcp140.dll.

A library may be spread across multiple additional DLLs, known as `dot libraries`. For example, some functionality in the standard library released in Visual Studio 2017 version 15.6 was added into msvcp140_1.dll, to preserve the ABI compatibility of msvcp140.dll. If you use Visual Studio 2017 version 15.6 (toolset 14.13) or later, you may need to locally deploy these dot libraries as well as the main library. These separate dot libraries will eventually be added to the base library, when the ABI changes.

## 1.3 static linking
In addition to dynamically linked libraries, Visual Studio supplies most of its libraries as static libraries. You can statically link a static library to your application, that is, link the library object code directly into the application. This creates a single binary without a DLL dependency, so that you don't have to deploy the Visual C++ library files separately. 

---

# 2. Visual C++ redistributable libraries version
Visual Studio versions since Visual Studio 2015 share the same redistributable files. For example, any apps built by the Visual Studio 2015, 2017, 2019, or 2022 toolsets can use the latest Microsoft Visual C++ Redistributable. However, the version of the Microsoft Visual C++ redistributable installed on the machine must be the same or higher than the version of the Visual C++ toolset used to create your application. 

> **Windows XP Support**: Microsoft ended support for Windows XP on April 8, 2014. Current versions of the Visual C++ Redistributable for Visual Studio 2015-2022 only support Windows Vista, 7, 8.1, 10, and 11. The last version of the Visual C++ Redistributable that works on Windows XP shipped in Visual Studio 2019 version 16.7 (file versions starting with 14.27). 

---

# 3. Universal CRT
From Visual Studio .NET through Visual Studio 2013, each major release of the C++ compiler and tools has included a new, standalone version of the Microsoft C Runtime (CRT) library. These standalone versions of the CRT were independent from, and to various degrees, incompatible with each other. For example, the CRT library used by Visual Studio 2012 was version 11, named msvcr110.dll, and the CRT used by Visual Studio 2013 was version 12, named msvcr120.dll. Beginning in Visual Studio 2015, it's no longer the case. Visual Studio 2015 and later versions of Visual Studio all use one Universal CRT.

The Universal CRT (UCRT) is a Microsoft Windows operating system component. It's included as part of the operating system in Windows 10 or later, and Windows Server 2016 or later. The UCRT is available by using Windows Update on older operating systems that are still in extended support. Local deployment of the Universal CRT is supported, with some restrictions.

## 3.1 UCRT central deployment
The Universal CRT is a Recommended update for all supported Microsoft Windows operating systems, so by default, most machines install it as part of the regular update process. The initial release of the Universal CRT was [KB2999226](https://support.microsoft.com/kb/2999226). A later update with various bug fixes was made in [KB3118401](https://support.microsoft.com/kb/3118401), and there have been more updates with further bug fixes and new features. For more recent updates, search [support.microsoft.com](https://support.microsoft.com/) for "Universal C Runtime" or "Universal CRT".

Not all Microsoft Windows computers regularly install updates by use of Windows Update, and some may not install all Recommended updates. To support the use of applications built by using the Visual Studio 2015 and later C++ toolsets on those machines, there are Universal CRT Redistributable files available for offline distribution. Those redistributable files may be downloaded from one of the KB links above. The Universal CRT Redistributable requires that the machine has been updated to the current service pack. So, for example, the redistributable for Windows 7 will only install onto Windows 7 SP1, not Windows 7 RTM.

The Universal CRT is a fundamental dependency of the C++ libraries. The Visual C++ Redistributable (VCRedist) installs the initial version of the Universal CRT (version 10.0.10240) on machines that don't already have one installed. This version is sufficient to satisfy the C++ library dependencies. If your application depends on a more recent version of the Universal CRT, you must use Windows Update to bring your machine fully up to date. Or, install that version of the UCRT explicitly. It's best to install the Universal C Runtime by using Windows Update or an MSU before you install the VCRedist. This installation order can help avoid multiple required reboots.

Not all operating systems are eligible for the most recent Universal C Runtime via Windows Update. On Windows 10 or Windows 11, the centrally deployed version matches the version of the operating system. To update the Universal C Runtime further, you must update the operating system. For Windows Vista through Windows 8.1, the latest available Universal C Runtime is based on the Windows 10 Anniversary Update (version 10.0.14393).

## 3.2 UCRT local deployment
The UCRT DLLs for local deployment are included as part of the Windows SDK, in the `Windows Kits\10\Redist\ucrt\DLLs` subdirectory, by computer architecture (such as `C:\Program Files (x86)\Windows Kits\10\Redist\ucrt\DLLs\x86`). The DLLs required include `ucrtbase.dll` and a set of APISet forwarder DLLs named `api-ms-win-*.dll`. The set of DLLs required on each operating system varies. It's highly recommended that you include all of the DLLs when you deploy locally.

There are two restrictions on local deployment to be aware of:

- On Windows 10 and Windows 11, the Universal CRT in the system directory is always used, even if an application includes an application-local copy of the Universal CRT. It's true even when the local copy is newer, because the Universal CRT is a core operating system component on Windows 10 and later.

- Versions of Windows before Windows 8: The Universal CRT can't be packaged locally with a plugin, if it's located anywhere but the main app executable directory. The APISet forwarder DLLs are unable to resolve the ucrtbase.dll successfully in this case. Some recommended alternative solutions include:

    + Statically link the Universal CRT,
    + Centrally deploy the Universal CRT, or
    + Place the Universal CRT files in the same directory as the app.

> **Windows XP:** When the Visual C++ Redistributable is installed on Windows XP, it directly installs the Universal CRT and all of its dependencies into the system directory. It doesn't install or depend on any Windows Update. 

---

# 4. Download Visual C++ Redistributable Package
## 4.1 Visual Studio 2015, 2017, 2019, and 2022
|Architecture|Offical Link|Note|
|---|---|---|
|ARM64|https://aka.ms/vs/17/release/vc_redist.arm64.exe|Permalink for latest supported ARM64 version|
|X86|https://aka.ms/vs/17/release/vc_redist.x86.exe|Permalink for latest supported x86 version|
|X64|https://aka.ms/vs/17/release/vc_redist.x64.exe|Permalink for latest supported x64 version. The X64 redistributable package contains both ARM64 and X64 binaries.|

## 4.2 Visual Studio 2013 (VC++ 12.0)
|Architecture|Offical Link|Version|
|---|---|---|
|X86|https://aka.ms/highdpimfc2013x86enu|12.0.40664.0|
|X64|https://aka.ms/highdpimfc2013x64enu|12.0.40664.0|

## 4.3 Visual Studio 2012 (VC++ 11.0) Update 4
|Architecture|Offical Link|Version|
|---|---|---|
|X86|https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe|11.0.61030.0|
|X64|https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe|11.0.61030.0|

## 4.4. Visual Studio 2010 (VC++ 10.0) SP1
> Visual Studio 2010 Service Pack 1 reached end of extended support on July 14, 2020

|Architecture|Offical Link|Version|
|---|---|---|
|X86|https://download.microsoft.com/download/C/6/D/C6D0FD4E-9E53-4897-9B91-836EBA2AACD3/vcredist_x86.exe|10.0.40219.1|
|X64|https://download.microsoft.com/download/A/8/0/A80747C3-41BD-45DF-B505-E9710D2744E0/vcredist_x64.exe|10.0.40219.1|

## 4.5 Visual Studio 2008 (VC++ 9.0) SP1
> Visual Studio 2008 Service Pack 1 reached end of extended support on April 10, 2018

|Architecture|Offical Link|Version|
|---|---|---|
|X86|https://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x86.exe|9.0.30729.5677|
|X64|https://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x64.exe|9.0.30729.5677|

## 4.6 Visual Studio 2005 (VC++ 8.0) SP1
> Visual Studio 2005 reached end of extended support on April 12, 2016

|Architecture|Offical Link|Note|
|---|---|---|
|X86|https://www.microsoft.com/en-us/download/details.aspx?id=26347||
|X64|https://www.microsoft.com/en-us/download/details.aspx?id=26347||
|IA64|https://www.microsoft.com/en-us/download/details.aspx?id=26347||

