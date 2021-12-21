---
uid: PIAdapterForRDBMSQueriesConfiguration
---

# Queries

Queries configuration contains a list of queries to run on the data source.

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
    * If you use a component Id other than `RDBMS1`, update the endpoint with your chosen component Id.
    <br/>
    <br/>

## RDBMS query schema

The full schema definition for the RDBMS query configuration is in the `RDBMS_Queries_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\RDBMS\Schemas`

Linux: `/opt/OSIsoft/Adapters/RDBMS/Schemas`

## RDBMS query parameters

The following parameters are available to configure queries to be sent to the Open Database Connectivity (ODBC) driver. The supported query syntax depends on the ODBC driver you are using.
These queries are sent to the driver exactly as defined, and the adapter uses the driver response to collect data.

Refer to the documentation for your ODBC driver for supported query syntax.

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| QueryString<sup> | Required | `string` | The SQL query to run. |
| Id | Required | `string` | String identifier of the query to be referenced by data selection items. |

<sup> Within `QueryString`, you can use the placeholder values listed below to streamline query configuration. Delimit the placeholders with question marks. Placeholders include:


* `?LST?`: Last Scan Time. This is a timestamp representing the time the adapter ran the last scan.
* `?ET?`: End Time. If the adapter is in a history recovery mode, this will be the `?ST?` value plus the RequestInterval specified in the data source configuration. For example, if **RequestInterval** is set to 1 hour, there will be an hour difference between `?ST?` and `?ET?`. If the adapter is not in history recovery mode, this will be replaced with the current time.
* `?ST?`: Start Time. If the adapter is in history recovery mode, this placeholder will be replaced by the StartTime specified in the history recovery configuration on the first scan. After the first scan during history recovery, this placeholder will be replaced by the timestamp of the last event collected.

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

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/Queries      | GET       | Gets all configured queries |
| api/v1/configuration/_ComponentId_/Queries      | DELETE    | Deletes all configured queries |
| api/v1/configuration/_ComponentId_/Queries      | POST      | Adds an array of queries or a single query. Fails if any query already exists |
| api/v1/configuration/_ComponentId_/Queries      | PUT       | Replaces all queries |
| api/v1/configuration/_ComponentId_/Queries/*id* | GET       | Gets configured query by *id* |
| api/v1/configuration/_ComponentId_/Queries/*id*| DELETE     | Deletes configured query by *id* |
| api/v1/configuration/_ComponentId_/Queries/*id* | PUT       | Replaces query by *id*. Fails if query does not exist |
| api/v1/configuration/_ComponentId_/Queries/*id* | PATCH     | Allows partial updating of configured query by *id* |

**Note:** Replace *ComponentId* with the Id of your adapter component.
