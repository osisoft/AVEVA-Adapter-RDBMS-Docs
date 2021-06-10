---
uid: PIAdapterForRDBMSDataSourceDiscovery
---

# Data source discovery

A discovery against the data source of an RDBMS adapter allows you to specify the optional **query** parameter. The query discovers the contents of the data source and narrows down the scope of the discovery. You can add the discovered items to the data selection. For more information, see also [Queries](xref:Queries). <!-- I am unsure how exactly this topic is helpful for configuring the discovery, but remember from our call that you mentioned the ?LST? placeholder that we describe in the Queries topic -->

## RDBMS query string

The string of the **query** parameter must contain string items in the following form: <br>`<QueryId>/<IndexColumn>/<IdColumn>` <!-- Is TimeStamp an actual parameter that is specified in the query string? --><br><br>

| String item      | Required | Description |
|------------------|----------|-------------|
| **QueryId**     | Optional | The identifier of the query configured.
| **IndexColumn** | Optional | Column that contains the timestamp. If no column is specified, the timestamp will be the time that the adapter read the value. <!-- Does the IndexColumn replace the Timestamp you mentioned today? -->
| **IdColumn**    | Optional | Name of the column that contains the IdField. Used to identify which rows of the query results belong to the selection item.

### Query rules

The following rules apply for specifying the query string:

- Multiple queries are separated by a semicolon (`;`).
- Partial queries are terminated by a multi-level wildcard (`#`).
- A query cannot be terminated by a trailing slash (`/`).
- A query cannot start with a leading slash (`/`) or `$`.
- Topics are case sensitive.

**Note:** The data source might contain tens of thousands of metrics. Use the `#` judiciously and narrow down the query string to something specific or break down the query into different discoveries.

#### Wildcards

Wildcards are allowed in the query with the following specifications:

- A single-level wildcard replaces one topic level and is indicated by `+`.
- A multi-level wildcard covers many topic levels and is indicated by `#`.
- Wildcards can be combined.
- `#` must not be used more than once and can only be used at the end of the topic.
- No query, an empty string, or `null` as the query parameter is equivalent to `#`.

## Discovery query example

The query parameter must be specified in the following form:
`<QueryId>/<IndexColumn>/<IdColumn>`.

### RDBMS data source discovery initiation

```json
{
	"id" : "40",
	"query" : "Tanks/SampleTime/Asset"
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
