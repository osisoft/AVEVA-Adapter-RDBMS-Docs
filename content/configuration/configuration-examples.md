---
uid: PIAdapterForRDBMSConfigurationExamples
---

# Configuration examples

The following JSON samples provide examples for all configurations available for PI Adapter for RDBMS.

## System components configuration with two RDBMS adapter instances

```json
[
  {
    "ComponentId": "RDBMS1",
    "ComponentType": "RDBMS"
  },
  {
    "ComponentId": "RDBMS2",
    "ComponentType": "RDBMS"
  },
  {
    "ComponentId": "OmfEgress",
    "ComponentType": "OmfEgress"
  }
]

```

## RDBMS adapter configuration

```json
{
  "RDBMS1": {
      "Logging": {
          "logLevel": "Information",
          "logFileSizeLimitBytes": 34636833,
          "logFileCountLimit": 31
      },
      "DataSource": [
        {
          "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
          "ConnectString": "Server=ServerName\\SQLEXPRESS; UID=[username]; PWD=[password]",
          "DataProvider": "sqlServer"
        }
      ],
      "DataSelection": [
        {
          "StreamId": "Tank1.Temperature",
          "ValueColumn": "Temperature",
          "IndexColumn": "SampleTime",
          "SelectColumn": "Asset",
          "SelectValue": "Tank1",
          "ScheduleId": "1min",
          "QueryId": "Tanks",
          "Selected": true
        },
        {
          "StreamId": "Tank2.Temperature",
          "ValueColumn": "Temperature",
          "IndexColumn": "SampleTime",
          "SelectColumn": "Asset",
          "SelectValue": "Tank2",
          "ScheduleId": "1min",
          "QueryId": "Tanks",
          "Selected": true
        }
      ]
  },
  "System": {
      "Logging": {
          "logLevel": "Information",
          "logFileSizeLimitBytes": 34636833,
          "logFileCountLimit": 31
      },
      "HealthEndpoints": [
      ],
  "Diagnostics": {
          "enableDiagnostics": true
  },
      "Components": [
          {
              "componentId": "Egress",
              "componentType": "OmfEgress"
          },
          {
              "componentId": "RDBMS1",
              "componentType": "RDBMS"
          }
      ],
  "Buffering": {
          "BufferLocation": "C:/ProgramData/OSIsoft/Adapters/RDBMS/Buffers",
          "MaxBufferSizeMB": -1,
          "EnableBuffering": true
      }
   },
  "OmfEgress": {
      "Logging": {
          "logLevel": "Information",
          "logFileSizeLimitBytes": 34636833,
          "logFileCountLimit": 31
      },
      "DataEndpoints": [
          {
              "id": "WebAPI EndPoint",
              "endpoint": "https://PIWEBAPIServer/piwebapi/omf",
              "userName": "USERNAME",
              "password": "PASSWORD"
          },
          {
              "id": "OCS Endpoint",
              "endpoint": "https://OCSEndpoint/omf",
              "clientId": "CLIENTID",
              "clientSecret": "CLIENTSECRET"
          }
      ]
  }
}
```

## Data source configuration

The following are examples of valid RDBMS data source configurations.

### RDBMS minimum data source configuration for ODBC

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "DSN=MyDSN",
    "DataProvider": "ODBC"
  }
]
```

### RDBMS minimum data source configuration for SQL Server

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Server=ServerName\\SQLEXPRESS; UID=[username]; PWD=[password]",
    "DataProvider": "sqlServer"
  }
]
```

### RDBMS data source configuration with Windows Authentication for SQL Server
```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Server=ServerName\\SQLEXPRESS; Trusted_Connection=yes",
    "WindowsUser": "domain\\MyUser",
    "WindowsPassword": "MyPassword",
    "DataProvider": "sqlServer"
  }
]
```

## Data selection configuration

There are two main ways to configure a selection item: by column or by row.

### Column as an identifier

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

Tank1.Temperature:

```json
[
  {
    "StreamId": "Tank1.Temperature",
    "ValueColumn": "Temperature",
    "IndexColumn": "SampleTime",
    "SelectColumn": "Asset",
    "SelectValue": "Tank1",
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
    "SelectColumn": "Asset",
    "SelectValue": "Tank2",
    "ScheduleId": "1min",
    "QueryId": "Tanks",
    "Selected": true
  }
]
```
