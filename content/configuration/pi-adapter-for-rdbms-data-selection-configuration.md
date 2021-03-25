---
uid: PIAdapterForRDBMSDataSelectionConfiguration
---

# PI Adapter for RDBMS data selection configuration

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

Each data selection item references a query whose results contain the value(s) for that item. For more information on queries, see [PI Adapter for RDBMS queries configuration](xref:PIAdapterForRDBMSQueriesConfiguration).

## Configure RDBMS data selection

Complete the following steps to configure an RDBMS data selection. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSelection` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an RDBMS data selection into the file.

    For sample JSON, see [RDBMS data selection examples](#rdbms-data-selection-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [RDBMS data selection parameters](#rdbms-data-selection-parameters).

4. Save the file. For example, as `ConfigureDataSelection.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSelection.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data selection configuration.

    ```bash
    curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/RDBMS1/DataSelection"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `RDBMS1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data selection configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## RDBMS data selection schema

The full schema definition for the RDBMS data selection configuration is in the `RDBMS_DataSelection_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\RDBMS\Schemas`

Linux: `/opt/OSIsoft/Adapters/RDBMS/Schemas`

## RDBMS data selection parameters

| Parameter        | Required | Type      | Description |
|------------------|----------|-----------|-------------|
| **DataFilterId** | Optional | `string` | The identifier of a data filter defined in the [Data filters configuration](xref:DataFiltersConfiguration). By default, no filter is applied. |
| **ValueColumn** | Required | `string` | The database column to read data values from. |
| **IndexColumn** | Optional | `string` | Column that contains the timestamp. If no column is specified, the timestamp will be the time that the adapter read the value. |
| **IdColumn** | Optional | `string` | Name of the column that contains the IdField. Used to identify which rows of the query results belong to the selection item. |
| **IdField** | Optional | `string` | The unique identifier of the source. Should be in the database column specified in **IdColumn**. Used to identify which rows of the query results belong to the selection item. |
| **Name** | Optional | `string` | The optional friendly name of the data item collected from the data source. If not configured, the default value is the stream ID. |
| **QueryId** | Required | `string` | The identifier of the query configured. |
| **ScheduleId** | Required | `string` | The identifier of a schedule defined in the Schedules configuration. |
| **Selected** | Optional | `boolean` | If `true`, data for this item is collected and sent to one or more configured OMF endpoints.<br><br>Allowed value: `true` or `false`<br>Default value:`true` |
| **StreamId** | Optional | `string` | The custom identifier used to create the stream. If not specified, the RDBMS adapter generates a default value based on the **DefaultStreamIdPattern** in the [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration). |

## RDBMS data selection examples
There are two main ways to configure a selection item. The first and simplest method is used when the columns of the result set are all that is needed to uniquely identify a stream. The second and most common method is used when each row of the result set contains information necessary to uniquely identify a stream.  
The following are examples of valid RDBMS data selection configurations:

### Column as an identifier
This configuration is used when you have a table whose columns identify each stream in the result set. In the simplest of cases, there is one value column and one timestamp column; however, there could be multiple values and multiple timestamps. A separate data selection item needs to be defined for each timestamp value pair.

Example data: 

| Temperature | Pressure | Volume | SampleTime |
|-------------|----------|--------|------------|
| 25 | 100 | 50.6 | 11/11/2020 3:56:00 AM |
| 26.3 | 100.2 | 50.7 | 11/11/2020 3:26:00 AM |

Temperature:
```json
[
  {
    "StreamId": "Tank1.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "ScheduleId": "1min",
    "QueryId": "Tank1",
    "Selected": true
  }
]
```
Pressure:
```json
[
  {
    "StreamId": "Tank1.Pressure",
    "ValueColumn": "Pressure",
    "IndexColumn": "SampleTime",
    "ScheduleId": "1min",
    "QueryId": "Tank1",
    "Selected": true
  }
]
```
Volume:
```json
[
  {
    "StreamId": "Tank1.Volume",
    "ValueColumn": "Volume",
    "IndexColumn": "SampleTime",
    "ScheduleId": "1min",
    "QueryId": "Tank1",
    "Selected": true
  }
]
```

### Identifier in Row
This configuration is used when each row of the result set contains information needed to identify each stream. In the simplest case, there could be one column for the identifier, one column for the timestamp, and one column for the value. However, result sets could contain multiple timestamp columns and multiple value columns. This configuration requires that the user specify the IdColumn and IdField properties.

Example Data:

| Asset | Temperature | Pressure | Volume | SampleTime |
|-------|-------------|----------|--------|------------|
| Tank1 | 25 | 100 | 50.6 | 11/11/2020 3:56:00 AM |
| Tank2 | 26.3 | 100.2 | 50.7 | 11/11/2020 3:56:00 AM |
| Tank3 | 26.4 | 100.1 | 50.5 | 11/11/2020 3:56:00 AM |
| Tank2 | 26.5 | 100 | 50.4 | 11/11/2020 3:46:00 AM |
| Tank3 | 26.6| 99.9 | 50.3 | 11/11/2020 3:46:00 AM |
| Tank1 | 26.7 | 99.8 | 50.2 | 11/11/2020 3:46:00 AM |

Tank1.Temperature:
```json
[
  {
    "StreamId": "Tank1.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "IdColumn": "Asset",
    "IdField": "Tank1",
    "ScheduleId": "1min",
    "QueryId": "Tanks",
    "Selected": true
  }
]
```
Tank2.Temperature:
```json
[
  {
    "StreamId": "Tank2.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "IdColumn": "Asset",
    "IdField": "Tank2",
    "ScheduleId": "1min",
    "QueryId": "Tanks",
    "Selected": true
  }
]
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSelection | `GET` | Retrieves the RDBMS data selection configuration |
| api/v1/configuration/\<ComponentId\>/DataSelection | `PUT` | Configures or updates the RDBMS data selection configuration |
| api/v1/configuration/\<ComponentId\>/DataSelection | `DELETE` | Deletes the RDBMS data selection configuration |
| api/v1/configuration/\<ComponentId\>/DataSelection | `PATCH` | Allows partial updating of configured data selection items. <br>**Note:** The request must be an array containing one or more data selection items. Each data selection item in the array must include its **StreamId**. |
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\> | `PUT` | Updates or creates a new data selection with the specified **StreamId**|
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\>| `DELETE` | Deletes a specific data selection item of the RDBMS data selection configuration |

**Note:** Replace \<ComponentId\> with the Id of your RDBMS component, for example RDBMS1.
