---
uid: PIAdapterForRDBMSSupportedFeatures
---

# Supported features

## Data types

Because relational databases may have custom types, the ODBC driver you choose needs to support type mapping to the following .NET types:

- bool
- short
- int
- uint
- float
- double
- string

For more details, see the ODBC driver-specific documentation.

## History recovery

PI Adapter for RDBMS supports [History Recovery](xref:HistoryRecovery). The adapter creates history recovery intervals only on shutdown. Every item is historical by design and there is no automatic history recovery interval on a `bad` device status.

Data source configuration properties **MaxHistoryEventsPerSecond**, **RequestInterval**, and **DataCollectionMode** impact history recovery depending on how they are configured. For more information, see [Data source - RDBMS data source parameters](xref:PIAdapterForRDBMSDataSourceConfiguration#rdbms-data-source-parameters).
