---
uid: PIAdapterForRDBMSDataSourceConfiguration
---

# Data source

To use PI Adapter for RDBMS, you must configure the adapter to collect data from one or more relational databases using the data source configuration. The data source configuration defines properties to communicate with the source relational database through a specified data provider.

## Data providers

The data source configuration allows you to choose between two data providers: SQLServer or ODBC (Open Database Connectivity). The SQLServer data provider allows you to connect to a Microsoft SQL Server without any additional software. The ODBC data provider allows you to connect to any ODBC-compliant relational database.

**Note:** When using the ODBC data provider, you need to install an appropriate ODBC driver for your data source.  For more information on ODBC drivers, refer to the [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using.

## Configure RDBMS data source

Complete the following steps to configure an RDBMS data source. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSource` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an RDBMS data source into the file.

    For sample JSON, see [RDBMS data source examples](#rdbms-data-source-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [RDBMS data source parameters](#rdbms-data-source-parameters).

4. Save the file. For example, as `ConfigureDataSource.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSource.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data source configuration.

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/RDBMS1/DataSource"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component Id other than `RDBMS1`, update the endpoint with your chosen component Id.
    * For a list of other REST operations you can perform, like updating or deleting a data source configuration, see [REST URLs](#rest-urls).
    * You can decide to have a default data selection file generated automatically or you can create the data selection file yourself.

7. Configure data selection.

    For more information, see [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration)

## RDBMS data source schema

The full schema definition for the RDBMS data source configuration is in the `RDBMS_DataSource_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\RDBMS\Schemas`

Linux: `/opt/OSIsoft/Adapters/RDBMS/Schemas`

## RDBMS data source parameters

The following parameters are available for configuring an RDBMS data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **ConnectString** | Required | `string` | Connection string to connect to the data source through the **DataProvider**. This string is encrypted when stored, so secrets such as passwords are safe to use in this string. <br><br> If you have a pre-configured DSN, you may simply specify `DSN={YourDSN}` for this property, along with any other required parameters.<br><br>For more information, refer to the documentation for your relational database. |
| **DataProvider** | Required | `string` | Indicates which Data Provider the adapter should use to connect to the data source. Valid values are `SQLServer` or `ODBC`. <br><br> When connecting to Microsoft SQL Server, use `SQLServer`. When connecting to another relational database through ODBC, use `ODBC`. The `SQLServer` data provider does not require any additional software, but the `ODBC` data provider requires an ODBC driver. For more details, see [SystemRequirements](xref:SystemRequirements).
| **WindowsUser** | Optional | `string` | Optional username to use with Windows Authentication. <br><br>**Note**: You must add your domain to the **WindowsUser**.<br><br>Example: `DomainName\UserName`<br><br>**Note:** This user should be configured to have the minimum permissions needed to read the desired data from your database. It is not recommended to use an admin user or even a user with write permissions. |
| **WindowsPassword** | Optional | `string` | Optional password to be used with Windows Authentication |
| **CommandTimeout** | Optional | `string` | Optional timeout for running queries on the data source.<br><br>Expected format: `HH:MM:SS.###`<br><br>Default Value: 00:00:30 (30 seconds) |
| **UTC** | Optional | `bool` | If `true`, timestamps from the data source are interpreted as UTC time. If `false`, local time relative to the adapter is assumed.<br><br>Allowed value: `true` or `false`<br>Default value: `true` |
| **StreamIdPrefix** | Optional | `string` | Specifies what prefix is used for stream Ids. The naming convention is `{StreamIdPrefix}{StreamId}`. An empty string means no prefix will be added to the stream Ids and names. A `null` value defaults to **ComponentId** followed by a period.<br><br>Example: `RDBMS1.TBD`<br><br>**Note:** If you change the **StreamIdPrefix** of a configured adapter (like when you delete and add a data source), you must restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated.<br><br>Allowed value: any string<br>Default value: `null`
| **DefaultStreamIdPattern** | Optional | `string` | Specifies the default stream Id pattern to use.  An empty or `null` value results in the default value. Possible parameters: `QueryId`, `ValueColumn`, `SelectValue`, `SelectColumn`, and `SourceId`.<br><br>Allowed value: any string<br><br>Default value: `{SourceId}` <br><br>**Note:** `{SourceId}` in the **DefaultStreamIdPattern** evaluates the same as `SourceId` in the stream metadata. For selection items with a non-empty `SelectValue`, `{SourceId}` is equivalent to `{QueryId}.{SelectValue}.{ValueColumn}`. For selection items with an empty `SelectValue`, `{SourceId}` is equivalent to `{QueryId}.{ValueColumn}`. |
| **RequestInterval** | Optional | `string` | Specifies the query interval for history recovery. This will be used to replace the ?ST? and ?ET? parameters in your query. For example, if the **RequestInterval** is set to `01:00:00`, the difference between ?ST? and ?ET? in each query will be 1 hour. For more information, see also [Automatic history recovery](xref:AutomaticHistoryRecovery) and [Queries configuration](xref:PIAdapterForRDBMSQueriesConfiguration). <br><br>Expected format: `HH:MM:SS.###`<br><br>Default Value: `1.00:00:00` (1 day) | 
| **MaxHistoryEventsPerSecond** | Optional | `int` | Specifies the maximum number of events per second that the adapter will collect during history recovery. This can be used to limit the adapter to prevent overwhelming the data source or network. Setting this property to `0` will allow historical data to be collected without throttling. <br><br>Expected Format: `#####` <br><br> Default Value: `0` |
| **DataCollectionMode** | Optional | `string` | Specifies the data collection mode the adapter is in. The following data collection modes are available:<br> **HistoryOnly** - The adapter component does not get started and history recovery on-demand is enabled. For more information, see [On-demand history recovery](xref:OnDemandHistoryRecovery).<br>**CurrentOnly** - The adapter component operates normally and on-demand history recovery is disabled.<br>**CurrentWithBackfill** - The adapter component operates normally, but will automaticall recover intervals during the `Shutdown` device status. History recovery backfills data once device status is `good`. On-demand history recovery is disabled. For more information, see [Automatic history recovery](xref:AutomaticHistoryRecovery). <br>Changing the mode requires a restart of the adapter component.

<!-- About the **RequestInterval**: I will talk to Aurora about the wording in the Automatic history recovery as this seems incorrect (at least for RDBMS) -->

## RDBMS data source examples

The following are examples of valid RDBMS data source configurations:

### RDBMS minimum data source configuration

```json
{
    "ConnectString": "DSN=MyDSN",
    "DataProvider": "ODBC"
}
```

### RDBMS data source configuration with username and password

**Note:** The ConnectString parameter will be encrypted, so it is safe to include a password or other secrets. 

```json
{
    "ConnectString": "Server=ServerName\\SQLEXPRESS; UID=MyUser; PWD=Password",
    "DataProvider": "sqlServer"
}
```

### RDBMS data source configuration with Windows authentication

```json
{
    "ConnectString": "Server=ServerName\\SQLEXPRESS; Database=MyDB; trusted_connection=yes;",
    "DataProvider": "SQLServer",
    "WindowsUser": "MyDomain\\MyUser",
    "WindowsPassword": "MyPassword"
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSource | `GET` | Retrieves the data source configuration. |
| api/v1/configuration/\<ComponentId\>/DataSource | `POST` | Creates the data source configuration. The adapter starts collecting data after the following conditions are met:<br/><br/>&bull; The data source configuration `POST` request is received.<br/>&bull; A data selection configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSource | `PUT` | Configures or updates the data source configuration. Overwrites any active data source configuration. If no configuration is active, the adapter starts collecting data after the following conditions are met:<br/><br/>&bull; The data source configuration `PUT` request is received.<br/>&bull; A data selection configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSource | `DELETE` | Deletes the data source configuration. After the request is received, the adapter stops collecting data. |

**Note:** Replace \<ComponentId\> with the Id of your RDBMS component, for example RDBMS1.
