---
uid: PIAdapterForRDBMSDataSourceConfiguration
---

# PI Adapter for RDBMS data source configuration

To use the adapter, you must configure the adapter to collect data from one or more relational databases using the data source configuration. The data source configuration defines properties to communicate with the source relational database through a specified data provider.

## Data providers

The data source configuration allows you to choose between two data providers: SQLServer or ODBC. The SQLServer data provider allows you to connect to a Microsoft SQL Server without any additional software. The ODBC data provider allows you to connect to any ODBC compliant relational database. When using the ODBC data provider, you need to install an appropriate ODBC driver for your data source.  For more information on ODBC drivers, refer to the [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using. 

## Configure RDBMS Data Files data source

Complete the following steps to configure an RDBMS data source. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSource` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an RDBMS data source into the file.

    For sample JSON, see [RDBMS data source examples](#RDBMS-data-source-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [RDBMS data source parameters](#RDBMS-data-source-parameters).

4. Save the file. For example, as `ConfigureDataSource.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSource.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data source configuration.

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/RDBMS1/DataSource"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `RDBMS1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data source configuration, see [REST URLs](#rest-urls).
    * You can decide to have a default data selection file generated automatically or you can create the data selection file yourself.
    <br/>
    <br/>

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
| **DataProvider** | Required | `string` | Indicates which Data Provider the adapter should use to connect to the data source. Valid values are `SQLServer` or `ODBC`. <br><br> When connecting to Microsoft SQL Server, use `SQLServer`. When connecting to another relational database through ODBC, use `ODBC`. The `SQLServer` data provider does not require any additional software, but the `ODBC` data provider requires an ODBC driver. See installation requirements for more details. 
| **WindowsUser** | Optional | `string` | Optional username to use with Windows Authentication. <br><br>**Note**: You must add your domain to the **WindowsUser**<br><br>Example: YourDomain\\YourUserName.<br><br>**Note:** This user should be configured to have the minimum permissions needed to read the desired data from your database. It is not recommended to use an admin user or even a user with write permissions. |
| **WindowsPassword** | Optional | `string` | Optional password to be used with Windows Authentication|
| **CommandTimeout** | Optional | `string` | Optional timeout for running queries on the data source.<br><br>Expected format: `HH:MM:SS.###`<br><br>Default Value: 00:00:30 (30 seconds) |
| **UTC** | Optional | `bool` | If `true`, timestamps from the data source are interpreted as UTC time. If `false`, local time relative to the adapter is assumed.<br><br>Allowed value: `true` or `false`<br>Default value: `true` |
| **StreamIdPrefix** | Optional | `string` | Specifies what prefix is used for stream IDs. The naming convention is `{StreamIdPrefix}{StreamId}`.An empty string means no prefix will be added to the stream IDs and names. A `null` value defaults to **ComponentID** followed by a period.<br><br>Example: `RDBMS1.TBD`<br><br>**Note:** If you change the **StreamIdPrefix** of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated.<br><br>Allowed value: any string<br>Default value: `null`
| **DefaultStreamIdPattern** | Optional | `string` | Specifies the default stream ID pattern to use.  An empty or `null` value results in the default value. Possible parameters: `QueryId`, `ValueColumn`, `IdField`, `IdColumn`, and `SourceId`.<br><br>Allowed value: any string<br><br>Default value: `{SourceId}` <br><br>**Note:** `{SourceId}` in the **DefaultStreamIdPattern** evaluates the same as `SourceId` in the stream metadata. For selection items with a non-empty `IdField`, `{SourceId}` is equivalent to `{QueryId}.{IdField}.{ValueColumn}` For selection items with an empty `IdField`, `{SourceId}` is equivalent to `{QueryId}.{ValueColumn}` |

## RDBMS data source examples

The following are examples of valid RDBMS data source configurations:

### RDBMS minimum data source configuration

```json
[
  {
    "ConnectString": "DSN=MyDSN",
    "DataProvider": "ODBC"
  }
]
```

### RDBMS data source configuration with username and password

**Note:** The ConnectString parameter will be encrypted, so it is safe to include a password or other secrets. 

```json
[
  {
    "ConnectString": "Server=ServerName\\SQLEXPRESS; UID=MyUser; PWD=Password",
  }
]
```

### RDBMS data source configuration with Windows authentication

```json
[
  {
    "ConnectString": "Server=ServerName\\SQLEXPRESS; Database=MyDB; trusted_connection=yes;",
    "DataProvider": "SQLServer",
    "WindowsUser": "MyDomain\\MyUser",
    "WindowsPassword": "MyPassword"
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSource  | `GET` | Retrieves the RDBMS data source configuration |
| api/v1/configuration/\<ComponentId\>/DataSource  | `POST` | Creates the RDBMS data source configuration |
| api/v1/configuration/\<ComponentId\>/DataSource  | `PUT` | Configures or updates the RDBMS data source configuration |
| api/v1/configuration/\<ComponentId\>/DataSource | `DELETE` | Deletes the RDBMS data source configuration |

**Note:** Replace \<ComponentId\> with the Id of your RDBMS component, for example RDBMS1.