---
uid: PIAdapterForRDBMSPrinciplesOfOperation
---

# Principles of operation

The RDBMS adapter's operations focus on data collection and stream creation.

## Adapter configuration

For the RDBMS adapter to start data collection, configure the following items:

- Data source: Provide the data source from which the adapter should collect data
- Data selection: Select RDBMS items for the adapter to read
- Queries: List SQL queries to execute against the data source
- Logging: Set up the logging attributes to manage the adapter logging behavior

For more details, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration) and [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration).

## Connection

PI Adapter for RDBMS can retrieve data from:

* SQL Server, using the .NET Data Provider for SQL Server
* Other relational databases, using the .NET Data Provider for ODBC (Open Database Connectivity)

When collecting data through ODBC, you must install and configure an appropriate ODBC driver for the data source before configuring the adapter. 

When collecting data from a SQL Server, additional driver installation is not necessary.

For more information on ODBC drivers, refer to [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-ver15) and the manual for the ODBC driver you are using.

The following ODBC Drivers have been tested and will work with this adapter. This is not a comprehensive list of ODBC drivers that can work with this adapter. Most recent ODBC drivers should work with this adapter but are not guaranteed. 

- [Microsoft ODBC Driver for SQL Server Version 17.8.1.1](https://docs.microsoft.com/en-us/sql/connect/odbc/microsoft-odbc-driver-for-sql-server?view=sql-server-ver15)
- [MariaDB Connector/ODBC 3.1 Series](https://downloads.mariadb.org/connector-odbc/)
- [MySQL Connector/ODBC Version 8.0.26](https://dev.mysql.com/downloads/connector/odbc/)
- [Oracle Instant Client ODBC Version 19.00.00.00](https://www.oracle.com/database/technologies/releasenote-odbc-ic.html)

## Data collection

The adapter collects data for each DataSelection item at an interval defined using the `ScheduleId` property, which you can set in the schedule configuration. 

When the schedule runs, the adapter executes all queries that are referenced by a DataSelection item in the schedule. The adapter parses the results of that query and sends data to the corresponding streams.

## Stream creation

The RDBMS adapter creates a stream with two properties for each selected RDBMS item. The properties are described in the following table.

| Property name | Data type | Description |
|---------------|-----------|-------------|
| `Timestamp`   | String    | The time read from the result set. If no time is available, the adapter uses the local time instead. |
| `Value`       | The type of the data source converted to an OMF type | The value read from the result set. |

Certain metadata are sent with each stream created. The following metadata are common for every adapter type:

- **ComponentId**: Specifies the data source, for example, _RDBMS1_
- **ComponentType**: Specifies the type of adapter, for example, _RDBMS_

Metadata specific to the RDBMS adapter includes:

- **ScheduleId**: Specifies the schedule used to collect data for the stream. For RDBMS, this value correlates to the ScheduleId property of the selection item. For example, `schedule1`.
- **SourceId**: Specifies the information used to uniquely identify the stream on the source system. For RDBMS it is `{QueryId}.{SelectValue}.{ValueColumn}`. For example, `TankQuery.Tank1.Temperature`. If there is no SelectValue specified for a given selection item, SelectValue is omitted from the SourceId. For example, `Query1.Temperature`.

Each stream created for the selected measurement has a unique identifier (stream Id). If you specify a custom stream Id for the measurement in the data selection configuration, the adapter uses that stream Id to create the stream. Otherwise, the adapter constructs the stream Id using the following format:

```code
<AdapterComponentId>.<QueryId>.<ValueColumn>
```

**Note:** Naming convention is affected by `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration. For more information, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration).
