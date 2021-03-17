---
uid: PIAdapterForRDBMSQueriesConfiguration
---

# PI Adapter for RDBMS queries configuration

Queries configuration holds a list of queries to run on the data source.

## Configure RDBMS queries

Complete the following steps to configure RDBMS queries. Use the `PUT` method in conjunction with the `http://localhost:5590/api/v1/configuration/<ComponentId>/Queries` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for RDBMS queries into the file.

    For sample JSON, see [RDBMS queries example](#rdbms-queries-example).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [RDBMS query parameters](#rdbms-query-parameters).

4. Save the file. For example, as `ConfigureQueries.json`.

5. Open a command line session. Change directory to the location of `ConfigureQueries.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the queries configuration.

    ```bash
    curl -d "@ConfigureQueries.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/<ComponentId>/Queries"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `RDBMS1`, update the endpoint with your chosen component ID.
    <br/>
    <br/>

## RDBMS query schema

The full schema definition for the RDBMS query configuration is in the `RDBMS_Queries_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\RDBMS\Schemas`

Linux: `/opt/OSIsoft/Adapters/RDBMS/Schemas`

## RDBMS query parameters

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


## RDBMS queries example

```json
[
    {
        "QueryId": "Tank1",
        "QueryString": "SELECT TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
    },
    {
        "QueryId": "Tanks",
        "QueryString": "SELECT ASSET, TEMPERATURE, PRESSURE, VOLUME, SAMPLETIME FROM TANK1 WHERE SAMPLETIME > ?LST? ORDER BY SAMPLETIME ASC"
    }
]
```
