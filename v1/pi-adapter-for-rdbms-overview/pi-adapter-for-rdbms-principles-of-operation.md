---
uid: PIAdapterForRDBMSPrinciplesOfOperation
---

# PI Adapter for RDBMS principles of operation

The RDBMS adapter's operations focus on data collection and stream creation

## Adapter configuration

For the RDBMS adapter to start data collection, configure the following:

- Data source: Provide the data source from which the adapter should collect data.
- Data selection: Select RDBMS items to which the adapter should read data from.
- Queries: List of SQL queries to execute against the data source.
- Logging: Set up the logging attributes to manage the adapter logging behavior.

For more details, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration) and [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration).

## Connection

PI Adapter for RDBMS can retrieve data from SQL Server via the .NET Data Provider for SQL Server and other relational databases via the .NET Data Provider for ODBC. When collecting data via ODBC, an appropriate ODBC driver for the data source should be installed and configured before the configuring the adapter. When collecting data from SQL Server, it is not necessary to install an ODBC driver. 

For more information on ODBC drivers, you may refer to [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using.

## Data collection

Data will be collected for each DataSelection Item at an interval defined in the schedule referenced by the DataSelection Item's `ScheduleId` property. When the schedule is ran, the adapter will execute all queries that are referenced by a DataSelection Item in the schedule. The adapter will parse the results of that query and send data to the corresponding streams. 

## Stream creation

The RDBMS adapter creates a stream with two properties for each selected RDBMS item. The properties are described in the following table.

| Property name | Data type | Description |
|---------------|-----------|-------------|
| `Timestamp`   | String    | The time read from the result set. If no time is read, this will be the local time the adapter read the value. |
| `Value`       | The type on the data source converted to an OMF type | The value read from the result set. |

Certain metadata are sent with each stream created. The following metadata are common for every adapter type:

- **ComponentId**: Specifies the data source, for example, _RDBMS1_
- **ComponentType**: Specifies the type of adapter, for example, _RDBMS_

Metadata specific to the RDBMS adapter:

- **ScheduleId**: Specifies the schedule used to collect data for the stream. For RDBMS, this correlates to the ScheduleId property of the selection item. For example, `schedule1`.
- **SourceId**: Specifies the information used to uniquely identify the stream on the source system. For RDBMS it is `{QueryId}.{IdField}.{ValueColumn}`. For example, `TankQuery.Tank1.Temperature`. If there is no IdField specified for a given selection item, IdField is left out of SourceId. For example, `Query1.Temperature`.

Each stream created for the selected measurement has a unique identifier (stream ID). If you specify a custom stream ID for the measurement in the data selection configuration, the adapter uses that stream ID to create the stream. Otherwise, the adapter constructs the stream ID using the following format:

```code
<AdapterComponentID>.<QueryId>.<ValueColumn>
```

**Note:** Naming convention is affected by `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration. For more information, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration).
