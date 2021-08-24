---
uid: PIAdapterForRDBMSDataSourceDiscovery
---

# Data source discovery

A discovery against the data source of an RDBMS adapter requires you to specify the **query** parameter. In the discover query, you must specify the `QueryId` of a query in the queries configuration facet. For more information, see [Queries](xref:PIAdapterForRDBMSQueriesConfiguration). This is the query that is executed against the data source. The adapter parses the result set for data selection items. To allow the adapter to create more accurate data selection items, you may also provide other key=value pairs. You can add the discovered items to the data selection.

## RDBMS query string

The string of the **query** parameter must contain string items in the following form: <br>`QueryId=Tanks;IndexColumn=SAMPLETIME;SelectColumn=ASSET;InitialTime=6/11/2021 4:29:12 PM` <br><br>

| String item      | Required | Description |
|------------------|----------|-------------|
| **QueryId**     | Required | The identifier of the query to reference in the `Queries` configuration facet. The results of this query determine the discovery results. 
| **IndexColumn** | Optional | Column containing the timestamp. The adapter will use this column to populate the `IndexColumn` property of newly discovered data selection items.<br><br> Default: empty string.
| **SelectColumn**    | Optional | Column containing the SelectValue. The adapter will use this column to populate the `SelectColumn` property of the newly discovered selection item. The adapter will also use this column to distinguish selection items from unique rows in the result set. Default: empty string.
| **InitialTime** | Optional | The timestamp that is used to substitute the `?LST?` parameter (if used) in your query during discovery. Default is one hour earlier than current time.

### Query rules

The following rules apply for specifying the query string:

- The query is made up of key=value pairs.
- Pairs are separated with a semicolon (`;`).
- Keys and values are separated with an equals (`=`).
- The discovery query must reference a defined query from the `Queries` configuration facet.
- Columns are case-insensitive.

**Note:** The data source might contain tens of thousands of metrics. Ensure that the query will only return data for the selection items you will be interested in.

## Discovery query example

The query parameter must be specified in the following form:
`QueryId=Tanks;IndexColumn=SAMPLETIME;SelectColumn=ASSET;InitialTime=11/11/2020 3:46:00 AM`.

### Example result set from `Tanks` query

| Asset | Temperature | Pressure | SampleTime |
|-------|-------------|----------|------------|
| Tank1 | 25 | 100 | 11/11/2020 3:56:00 AM |
| Tank2 | 26.3 | 100.2 | 11/11/2020 3:56:00 AM |
| Tank2 | 26.5 | 100 | 11/11/2020 3:46:00 AM |
| Tank1 | 26.7 | 99.8 | 11/11/2020 3:46:00 AM |

### RDBMS data source discovery initiation

```json
{
  "id" : "40",
  "query" : "QueryId=Tanks;IndexColumn=SampleTime;SelectColumn=Asset;InitialTime=11/11/2020 3:46:00 AM"
}
```

### RDBMS data source discovery results

```json
[
  {
    "id": "40",
    "query": "Tanks/SampleTime/Asset",
    "startTime": "2020-12-14T14:19:01.4383791-08:00",
    "endTime": "2020-12-14T14:19:31.8549164-08:00",
    "progress": 30,
    "itemsFound": 700,
    "newItems": 200,
    "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/rdbmsComponentId/Discoveries/40/result",
    "autoSelect": false,
    "status": "Complete",
    "errors": null
  }
]
```

### RDBMS discovered selection items

```json
[
  {
    "StreamId": "Tank1.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "SelectColumn": "Asset",
    "SelectValue": "Tank1",
    "ScheduleId": "1",
    "QueryId": "Tanks",
    "Selected": false
  },
  {
    "StreamId": "Tank1.Pressure",
    "ValueColumn": "Pressure",
    "IndexColumn": "SampleTime",
    "SelectColumn": "Asset",
    "SelectValue": "Tank1",
    "ScheduleId": "1",
    "QueryId": "Tanks",
    "Selected": false
  },
  {
    "StreamId": "Tank2.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "SelectColumn": "Asset",
    "SelectValue": "Tank2",
    "ScheduleId": "1",
    "QueryId": "Tanks",
    "Selected": false
  },
  {
    "StreamId": "Tank2.Pressure",
    "ValueColumn": "Pressure",
    "IndexColumn": "SampleTime",
    "SelectColumn": "Asset",
    "SelectValue": "Tank2",
    "ScheduleId": "1",
    "QueryId": "Tanks",
    "Selected": false
  }
]
```
