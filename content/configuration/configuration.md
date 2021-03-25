---
uid: Configuration
---

# Configuration

PI Adapter for RDBMS provides configuration of data source and data selection. The adapter also provides the ability to configure queries to run on the data source.

The examples in the configuration topics use `curl`, a commonly available tool on both Windows and Linux. The adapter can be configured with any programming language or tool that supports making REST calls, or with the EdgeCmd utility. For more information, see the [EdgeCmd utility documentation](https://osisoft.github.io/Edgecmd-Docs/content/edgecmd-utility.html). To validate successful configurations, you can perform data retrieval (GET commands) using a browser, if available on your device.

For more information on PI Adapter configuration tools, see [Configuration tools](xref:ConfigurationTools).

## Quick start

Complete the following steps to establish a data flow from an RDBMS data source device to a data endpoint.

1. Configure one or several RDBMS system components.<br>See [System components configuration](xref:SystemComponentsConfiguration#system-components-configuration).

2. Configure an RDBMS data source for each RDBMS device.<br>See [PI Adapter for RDBMS data source configuration](xref:PIAdapterForRDBMSDataSourceConfiguration#configure-rdbms-data-files-data-source).

3. Optional: Configure RDBMS queries to run on the data source.<br>See [PI Adapter for RDBMS queries configuration](xref:PIAdapterForRDBMSQueriesConfiguration).

4. Configure an RDBMS data selection for each RDBMS data source.<br>See [PI Adapter for RDBMS data selection configuration](xref:PIAdapterForRDBMSDataSelectionConfiguration#configure-rdbms-data-selection).

5. Configure one or several egress endpoints.<br>See [Egress endpoints configuration](xref:EgressEndpointsConfiguration).
