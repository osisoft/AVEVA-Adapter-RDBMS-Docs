---
uid: PIAdapterForRDBMSPrinciplesOfOperation
---

# PI Adapter for RDBMS principles of operation

The RDBMS adapter's operations focus on data collection and stream creation

## Adapter configuration

For the RDBMS adapter to start data collection, configure the following:

- Data source: Provide the data source from which the adapter should collect data.
- Data selection: Select RDBMS items to which the adapter should read data from.
- Queries: List of SQL queries to send to the ODBC driver.
- Logging: Set up the logging attributes to manage the adapter logging behavior.

For more details, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration) and [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration).

## Connection

The RDBMS adapter can retrieve data from a relational database by connecting to a compliant ODBC driver with a pre-configured DSN. The adapter assumes an appropriate ODBC driver is installed on the machine hosting the adapter. 

For more information on ODBC drivers, you may refer to [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using.

## Data collection

The adapter sends queries defined in the Queries configuration to the ODBC driver to run on the data source. These queries return a result set that the adapter parses through to get data values.

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

- **TBD**

Each stream created for the selected measurement has a unique identifier (stream ID). If you specify a custom stream ID for the measurement in the data selection configuration, the adapter uses that stream ID to create the stream. Otherwise, the adapter constructs the stream ID using the following format:

```code
<AdapterComponentID>.<QueryId>.<ValueColumn>
```

**Note:** Naming convention is affected by `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration. For more information, see [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration).
