---
uid: TroubleshootPIAdapterForRDBMS
---

# Troubleshoot PI Adapter for RDBMS

PI Adapter for RDBMS provides troubleshooting features that enable you to verify adapter configuration, confirm connectivity, and view message logs. If you are unable to resolve issues with the adapter or need additional guidance, contact OSIsoft Technical Support through the [OSIsoft Customer Portal](https://my.osisoft.com/).

## Check configurations

Incorrect configurations can interrupt data flow and cause errors in values and ranges. Perform the following steps to confirm correct configuration for your adapter.

1. Navigate to [data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration) and verify the following for each configured data source item below:

    * **ConnectString** - All properties in the ConnectString are correct. If properties are incorrect, the adapter cannot connect to the data source.
    * **UTC** - Set to `true` if the timestamps on the data source are UTC timestamps, or `false` if the timestamps on the data source are in the same timezone as the adapter. If this is incorrect, the adapter might not read any data at all, depending on the timestamps in the query.
    * **DataProvider** - The value of this property is `SQLServer` if the data source is a Microsoft SQL Server, otherwise the value is `ODBC`. If using `ODBC`, verify that an appropriate ODBC driver is installed and working properly on the machine your adapter is running on.
    * **WindowsUser and WindowsPassword** - Your data source supports Windows authentication. If so, make sure these values are correct and that the account has read access to the database.

2. Navigate to [data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration) and verify the following for each configured data selection item below:

    * **ValueColumn** - The column exists in the database table and contains the desired data values.
    * **IndexColumn** - The column exists in the database table and contains the desired DateTime formatted timestamps.
    * **IdColumn** - The column exists in the database table and contains the values specified in the IdField property for each of the selection items.
    * **IdField** - The value exists in the IdColumn in the database and is in the row that contains the data for this selection item.
    * **QueryId** - The query exists and is correct for the columns specified.
    * **ScheduleId** - The referenced schedule exists. <br> A non-existent referenced schedule uses a default schedule instead.
    * **DataFilterId** - If configured, the referenced data filter exists.<br> A non-existent or incorrect DataFilterId means that data filtering is not active.

3. Navigate to [query configuration](xref:PIAdapterForRDBMSQueriesConfiguration) and verify that the query is correct and retrieves the desired results.

4. Navigate to [egress endpoints configuration](xref:EgressEndpointsConfiguration). For each configured endpoint, verify that the **Endpoint** and authentication properties are correct.

    * For a PI server or EDS endpoint, verify **UserName** and **Password**.
    * For an OCS endpoint, verify **ClientId** and **ClientSecret**.

## Check connectivity

Perform the following steps to verify active connections to the data source and egress endpoints.

1. Start PI Web API and verify that the PI point values are updating or start OCS and verify that the stream values are updating.
2. If configured, use a health endpoint to determine the status of the adapter.<br>For more information, see [Health and diagnostics](xref:HealthAndDiagnostics).

## Check logs

Perform the following steps to view the adapter and endpoint logs to isolate issues for resolution.

1. Navigate to the logs directory:<br>
    Windows: `%ProgramData%\OSIsoft\Adapters\RDBMS\Logs`<br>
    Linux: `/usr/share/OSIsoft/Adapters/RDBMS/Logs`.
2. Optional: Change the log level of the adapter to receive more information and context.<br>For more information, see [Logging configuration](xref:LoggingConfiguration).
