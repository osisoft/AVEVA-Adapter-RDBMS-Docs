---
uid: PIAdapterForRDBMSDataSourceConfiguration
---

# PI Adapter for RDBMS data source configuration

To use the adapter, you must configure the data source to receive data.

Data source configuration defines properties needed to communicate with the source relational database via an ODBC driver. For more information on ODBC drivers, you may refer to [Microsoft's ODBC Programmers Reference](https://docs.microsoft.com/en-us/sql/odbc/reference/odbc-programmer-s-reference?view=sql-server-2017) and the manual for the ODBC driver you are using.
Each adapter component may connect to one DSN.

## Configure RDBMS Data Files data source

**Note:** To modify a RDBMS data source configuration, you must use the REST endpoints to add or edit the configuration.

Complete the following steps to configure a RDBMS data source:

1. Use any text editor to create a file that contains a RDBMS data source in the JSON format.
    - For content structure, see [RDBMS data source examples](#rdbms-data-source-examples).
    - For a table of all available parameters, see [RDBMS data source parameters](#rdbms-data-source-parameters).
2. Save the file. For example, `ConfigureDataSource.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run a PUT command with the contents of that file to the following endpoint: `http://localhost:5590/api/v1/configuration/<adapterId>/DataSource/`.

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

The following parameters are available for configuring a RDBMS data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **DSN** | Required | `string` | The Data Source Name (DSN) that represents an ODBC connection. |
| **Queries** | Required | `array` | List of queries to run on the data source.<br><br>For more information, see [Query parameters](xref:query-parameters). |
| **UserName** | Optional | `string` | Optional username based on the DSN configuration. For Windows authentication, this would be left blank. |
| **Password** | Optional | `string` | Optional password based on the DSN configuration. For Windows authentication, this would be left blank. |
| **StartTime** | Optional | `string` | Optional time to designate the start of history recovery process.<br>Expected format: `yyyy-MM-ddTHH:mm:ss.fffK` |
| **EndTime** | Optional | `string` | Optional time to designate when to stop the history recovery process and shutdown the Adapter. If no time is specified, the adapter will continue to collect real time data on the configured schedule.<br>Expected format: `yyyy-MM-ddTHH:mm:ss.fffK` |
| **CollectionInterval** | Optional | `string` | Maximum period of time for which the adapter will request data at once during history recovery. It is advised to set this property when doing history recovery so the adapter and data source do not get overloaded.<br><br>The expected format is HH:MM:SS.###. * |
| **UTC** | Optional | `boolean` | If true, timestamps from the data source will be interpreted as UTC time. If false, local time relative to the adapter will be assumed.<br><br>Allowed value: true or false<br>Default value: true |
| **StreamIdPrefix** | Optional | `string` | Specifies what prefix is used for stream IDs. The naming convention is `{StreamIdPrefix}{StreamId}`.An empty string means no prefix will be added to the stream IDs and names. A `null` value defaults to **ComponentID** followed by a period.<br><br>Example: `RDBMS1.TBD`<br><br>**Note:** If you change the **StreamIdPrefix** of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated.
| **DefaultStreamIdPattern** | Optional | `string` | Specifies the default stream ID pattern to use.  An empty or `null` value results in the default value. Possible parameters: `QueryId`, `ValueColumn`, `SourceId`, `IdColumn`, and `DSN`.<br><br>Allowed value: any string<br>Default value: `{QueryId}.{ValueColumn}`. |

## Query parameters

The following parameters are available to configure queries to be sent to the ODBC driver. The supported query syntax will depend on the ODBC driver you are using. These queries will be sent to the driver exactly as they are defined here, and the adapter will use what is returned by that query to collect data.
Consult the documentation for your ODBC driver for supported query syntax.

Placeholder values are provided by the adapter to make query configuration more streamlined. Make sure to surround the placeholders with question marks so the adapter knows to read them as placeholders. Those placeholders include:
* ?LST?: Last Scan Time
* ?ET?: End Time 
* ?ST?: Start Time

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| QueryString | Required | `string` | The SQL query to run. |
| Id | Required | `string` | String identifier of the query to be referenced by data selection items. |

## RDBMS data source examples

The following are examples of valid RDBMS data source configurations:

### RDBMS data source configuration without history recovery

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "Id": "MyDSN",
    "UserName": "MyUserName",
    "Password": "MyPassword",
    "Queries": [
      {
        "QueryId": "Tank1",
        "QueryString": "SELECT TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
      },
      {
        "QueryId": "Tanks",
        "QueryString": "SELECT ASSET, TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
      }
    ]
  }
]
```

### RDBMS data source configuration with history recovery

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "Id": "MyDSN",
    "UserName": "MyUserName",
    "Password": "MyPassword",
    "StartTime": "2019-11-11T19:01:55Z",
    "EndTime": "2020-11-11T19:01:55Z"
    "Queries": [
      {
        "QueryId": "Tank1",
        "QueryString": "SELECT TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
      },
      {
        "QueryId": "Tanks",
        "QueryString": "SELECT ASSET, TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
      }
    ]
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSource  | GET | Retrieves the RDBMS data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | POST | Creates the RDBMS data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | PUT | Configures or updates the RDBMS data source configuration |
| api/v1/configuration/_ComponentId_/DataSource | DELETE | Deletes the RDBMS data source configuration |

**Note:** Replace _ComponentId_ with the Id of your RDBMS component, for example RDBMS1.
