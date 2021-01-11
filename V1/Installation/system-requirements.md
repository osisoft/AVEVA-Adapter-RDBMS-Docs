---
uid: SystemRequirements
---

# System requirements

PI Adapter for RDBMS is supported on a variety of platforms and processors. Installation kits are available for the following platforms:

| Operating System | Platform | Installation Kit | Processor(s) |
|-------------------|-------------|----------------------------------|-------------|
| Windows 10 Enterprise <br>Windows 10 IoT Enterprise | x64 | `RDBMS_win10-x64.msi`     | Intel/AMD 64-bit processors |
| Debian 9, 10 <br>Ubuntu 18.04, 20.04 | x64 | `RDBMS_linux-x64.deb`     | Intel/AMD 64-bit processors |
| Debian 9, 10 <br>Ubuntu 18.04, 20.04 | ARM32 | `RDBMS_linux-arm.deb`  | Arm 32-bit processors |
| Debian 9, 10 <br>Ubuntu 18.04, 20.04 | ARM64 | `RDBMS_linux-arm64.deb`  | Arm 64-bit processors |

Alternatively, you can use tar.gz files with binaries to build your own custom installers or containers for Linux. For more information on installation of the PI Adapter for RDBMS with a Docker container, see [Install PI Adapter for RDBMS using Docker](xref:InstallPIAdapterForRDBMSUsingDocker).

### Native runtime libraries
##### Windows 
For Windows 10 installations, the latest Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017 and 2019 is required and installed by the PI Adapter for RDBMS installation kit.
As a best practice, OSIsoft recommends to install the [latest supported Microsoft Visual C++ downloads](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) to receive the latest available updates.

##### Linux
For Linux based operating systems, the GNU C++ Standard library is required. PI Adapter for RDBMS depends on `libstdc++.so.6.0.22` or newer. Newer versions of `libstdc++.so` must contain `GLIBCXX_3.4.22`.
In Debian, Ubuntu, and many other distributions, this library is distributed in the libstdc++6 package. This package is part of the base system for Debian 9 and 10, Ubuntu 18.04 and 20.04, as well as many other distributions, so there is often no action required for this dependency.
