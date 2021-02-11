---
uid: PIAdapterForRDBMSDataSourceConfiguration
---

# PI Adapter for RDBMS data source configuration

To use the adapter, you must configure the data source to receive data. The data source configuration defines properties to communicate with the source relational database by an ODBC driver. For more information on ODBC drivers, refer to the [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using. Each adapter component may connect to one DSN (Data Source Name).

## Configure RDBMS Data Files data source

**Note:** To modify an RDBMS data source configuration, you must use the REST endpoints to add or edit the configuration.

Complete the following steps to configure an RDBMS data source:

1. Use any text editor to create a file that contains a RDBMS data source in JSON format.

    - For content structure, see [RDBMS data source examples](#rdbms-data-source-examples).
    - For a table of all available parameters, see [RDBMS data source parameters](#rdbms-data-source-parameters).
2. Save the file. For example, `ConfigureDataSource.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run a `PUT` command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/DataSource/`.

      **Note:** The following example uses RDBMS1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration1-3).

    `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/RDBMS1/DataSource"
    ```

    **Note:** Run this command from the same directory where the file is located.

4. Configure data selection. For more information, see [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration).

    **Note:** You can decide to have a default data selection file generated automatically or you can create the data selection file yourself.

## RDBMS data source schema

The full schema definition for the RDBMS data source configuration is in the `RDBMS_DataSource_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\RDBMS\Schemas`

Linux: `/opt/OSIsoft/Adapters/RDBMS/Schemas`

## RDBMS data source parameters

The following parameters are available for configuring an RDBMS data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **ConnectString** | Required | `string` | Connection string to connect to the data source through an ODBC driver.<br><br> You may use the tokens `[username]` and `[password]` as placeholders for authentication properties. The **UserName** and **Password** parameters will be used by the adapter in place of these tokens.<br><br> If you have a pre-configured DSN, you may simply specify `DSN={YourDSN}` for this property, along with any other required parameters.<br><br>For more information, refer to the documentation for your relational database. |
| **UserName** | Optional | `string` | Optional username to be used in **ConnectString**. This value will replace the `[username]` token.<br><br>**Note**: If you are using Windows authentication, you must add your domain to the **UserName**<br><br>Example: YourDomain\\YourUserName.<br><br>**Note:** This user should be configured to have the minimum permissions needed to read the desired data from your database. It is not recommended to use an admin user or even a user with write permissions. |
| **Password** | Optional | `string` | Optional password to be used in **ConnectString**. This value will replace the `[password]` token. |
| **WindowsAuth** | Optional | `bool` | False by default. If true, and if **UserName** and **Password** are specified, the adapter will impersonate the account whose credentials are in the **UserName** and **Password** fields in order to use Windows Authentication. |
| **ConnectTimeout** | Optional | `string` | Optional timeout for connections to the data source.<br><br>Expected format: `HH:MM:SS.###`<br><br>Default Value: 00:00:30 (30 seconds) |
| **CommandTimeout** | Optional | `string` | Optional timeout for running queries on the data source.<br><br>Expected format: `HH:MM:SS.###`<br><br>Default Value: 00:00:30 (30 seconds) |
| **StartTime** | Optional | `string` | Optional time to designate the start of history recovery process.<br><br>Expected format: `yyyy-MM-ddTHH:mm:ss.fffK` |
| **EndTime** | Optional | `string` | Optional time to designate when to stop the history recovery process and shutdown the adapter. If no time is specified, the adapter will continue to collect real time data on the configured schedule.<br<br>Expected format: `yyyy-MM-ddTHH:mm:ss.fffK` |
| **RequestInterval** | Optional | `string` | Maximum period of time for which the adapter will request data at once during history recovery. It is advised to set this property when doing history recovery so the adapter and data source do not get overloaded.<br><br>Expected format: `HH:MM:SS.###`* |
| **UTC** | Optional | `bool` | If `true`, timestamps from the data source will be interpreted as UTC time. If `false`, local time relative to the adapter will be assumed.<br><br>Allowed value: `true` or `false`<br>Default value: `true` |
| **StreamIdPrefix** | Optional | `string` | Specifies what prefix is used for stream IDs. The naming convention is `{StreamIdPrefix}{StreamId}`.An empty string means no prefix will be added to the stream IDs and names. A `null` value defaults to **ComponentID** followed by a period.<br><br>Example: `RDBMS1.TBD`<br><br>**Note:** If you change the **StreamIdPrefix** of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated.
| **DefaultStreamIdPattern** | Optional | `string` | Specifies the default stream ID pattern to use.  An empty or `null` value results in the default value. Possible parameters: `QueryId`, `ValueColumn`, `SourceId`, `IdColumn`, and `DSN`.<br><br>Allowed value: any string<br><br>Default value: `{QueryId}.{ValueColumn}`. |

## RDBMS data source examples

The following are examples of valid RDBMS data source configurations:

### RDBMS minimum data source configuration without history recovery

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "DSN=MyDSN"
  }
]
```

### RDBMS data source configuration with history recovery

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "DSN=MyDSN",
    "StartTime": "2019-11-11T19:01:55Z",
    "EndTime": "2020-11-11T19:01:55Z"
  }
]
```
### RDBMS data source configuration with username and password

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Driver={ODBC Driver 17 for SQL Server}; Server=ServerName\\SQLEXPRESS; UID=[username]; PWD=[password]",
    "UserName": "MyUser",
    "Password": "MyPassword
  }
]
```

### RDBMS data source configuration with Window authentication

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Driver={ODBC Driver 17 for SQL Server}; Server=ServerName\\SQLEXPRESS; trusted_connection=yes",
    "UserName": "MyUser",
    "Password": "MyPassword",
    "WindowsAuth": true
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
