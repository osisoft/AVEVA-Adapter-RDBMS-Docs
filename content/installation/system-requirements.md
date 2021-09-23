---
uid: SystemRequirements
---

# System requirements

PI Adapter for RDBMS is supported on a variety of platforms and processors. Installation kits are available for the following platforms:

| Operating System | Platform | Installation Kit | Processor(s) |
|-------------------|-------------|----------------------------------|-------------|
| Windows 10 Enterprise <br>Windows 10 IoT Enterprise | x64 | `RDBMS_win10-x64.msi`     | Intel/AMD 64-bit processors |
| Debian 9, 10 <br>Ubuntu 18.04, 20.04 | x64 | `RDBMS_linux-x64.deb`     | Intel/AMD 64-bit processors |
| Debian 9, 10 <br>Ubuntu 20.04 | ARM32 | `RDBMS_linux-arm.deb`  | ARM 32-bit processors |
| Debian 9, 10 <br>Ubuntu 20.04 | ARM64 | `RDBMS_linux-arm64.deb`  | ARM 64-bit processors |

Alternatively, you can use tar.gz files with binaries to build your own custom installers or containers for Linux. For more information on installation of the PI Adapter for RDBMS with a Docker container, see [Install PI Adapter for RDBMS using Docker](xref:InstallPIAdapterForRDBMSUsingDocker).

When collecting data through ODBC (Open Database Connectivity), PI Adapter for RDBMS requires that the appropriate ODBC driver for your data source is properly installed and configured.