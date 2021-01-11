---
uid: PIAdapterForRDBMSQueriesConfiguration
---

# PI Adapter for RDBMS query configuration

Queries configuration holds a list of queries to run on the data source.

## Configure RDBMS queries

**Note:** You cannot modify RDBMS query configurations manually. You must use REST endpoints to add or edit the configuration.

Complete the following steps to configure RDBMS queries:

1. Use any text editor to create a file that contains a RDBMS query in the JSON format
    - For content structure, see [RDBMS queries example](#rdbms-queries-example). 
    - For a table of all available parameters, see [RDBMS query parameters](#rdbms-query-parameters).
2. Save the file. For example, `ConfigureQueries.json`.
3. Use any of the [Configuration tools](xref:ConfigurationTools1-3) capable of making HTTP requests to run either a POST or PUT command to their appropriate endpoint:

**Note:** The following examples use RDBMS1 as the adapter component name. For more information on how to add a component, see [System components configuration](xref:SystemComponentsConfiguration1-3).
  
    `5590` is the default port number. If you selected a different port number, replace it with that value.

    - **POST** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/Queries/`

      Example using `curl`:

      ```bash
      curl -d "@ConfigureQueries.json" -H "Content-Type: application/json" -X POST "http://localhost:5590/api/v1/configuration/RDBMS1/Queries"
      ```

      **Note:** Run this command from the same directory where the file is located.

    - **PUT** endpoint: `http://localhost:5590/api/v1/configuration/<componentId>/Queries/<QueryId>`

      Example using `curl`:

        ```bash
        curl -d "@ConfigureQuery.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/RDBMS1/Queries"
        ```

        **Note:** Run this command from the same directory where the file is located.

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