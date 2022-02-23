---
uid: SystemRequirements
---

# System requirements

[!include[product](../_includes/inline/product-name.md)] is supported on a variety of platforms and processors. Install kits are available for the following platforms:

| Operating System | Platform | Installation Kit | Processor(s) |
|-------------------|-------------|----------------------------------|-------------|
| Windows 10 Enterprise <br>Windows 10 IoT Enterprise | x64 | <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.msi</code>     | Intel/AMD 64-bit processors |
| Debian 9, 10<br>Ubuntu 18.04, 20.04 | x64 | <code>[!include[installer](../_includes/inline/installer-name.md)]-x64_.deb</code>     | Intel/AMD 64-bit processors |
| Debian 9, 10<br>Ubuntu 20.04 | ARM32 | <code>[!include[installer](../_includes/inline/installer-name.md)]-arm_.deb</code>  | ARM 32-bit processors |
| Debian 10<br>Ubuntu 20.04 | ARM64 | <code>[!include[installer](../_includes/inline/installer-name.md)]-arm64_.deb</code>  | ARM 64-bit processors |

Alternatively, you can use tar.gz files with binaries to build your own custom installers or containers for Linux. For more information on installing the adapter with Docker containers, see <xref:InstallUsingDocker>.

## Data connectivity

When collecting data through Open Database Connectivity (ODBC), PI Adapter for RDBMS requires that the appropriate ODBC driver for your data source is properly installed and configured. For more information on ODBC drivers, refer to [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-ver15) and the manual for the ODBC driver you are using.

When collecting data from a SQL Server, additional driver installation is not necessary.

The following ODBC drivers have been tested and work with this adapter. This is not a comprehensive list of ODBC drivers that work with this adapter. Most recent ODBC drivers should work, but it is not guaranteed.

- [Microsoft ODBC Driver for SQL Server Version 17.8.1.1](https://docs.microsoft.com/en-us/sql/connect/odbc/microsoft-odbc-driver-for-sql-server?view=sql-server-ver15)
- [MariaDB Connector/ODBC 3.1 Series](https://downloads.mariadb.org/connector-odbc/)
- [MySQL Connector/ODBC Version 8.0.26](https://dev.mysql.com/downloads/connector/odbc/)
- [Oracle Instant Client ODBC Version 19.00.00.00](https://www.oracle.com/database/technologies/releasenote-odbc-ic.html)

The following versions of SQL Server have been tested and work with this adapter. Again, this is not a comprehensive list. Most recent SQL Server versions should work, but it is not guaranteed.

- [Microsoft SQL Server 2019](https://www.microsoft.com/en-us/sql-server/sql-server-2019)
- [Microsoft SQL Server 2017](https://www.microsoft.com/en-us/sql-server/sql-server-2017)

**Note:** PI Adapter for RDBMS supports 64-bit DSN's only, 32-bit DSN's are not supported.

## PI Web API compatibility

This version of [!include[product](../_includes/inline/product-name.md)] is compatible with PI Web API 2021 and later. 
