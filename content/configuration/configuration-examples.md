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
          "ConnectString": "Driver={ODBC Driver 17 for SQL Server}; Server=ServerName\\SQLEXPRESS; UID=[username]; PWD=[password]",
          "UserName": "MyUser",
          "Password": "MyPassword"
        }
      ],
      "DataSelection": [
        {
          "StreamId": "Tank1.Temperature",
          "ValueColumn": "Temperature",
          "IndexColumn": "SampleTime",
          "IdColumn": "Asset",
          "IdField": "Tank1",
          "ScheduleId": "1min",
          "QueryId": "Tanks",
          "Selected": true
        },
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
          },
          {
              "Id": "EDS",
              "Endpoint": "http://localhost:/api/v1/tenants/default/namespaces/default/omf",
              "UserName": "eds",
              "Password": "eds"
          }
      ]
  }
}
```

## Data source configuration

The following are examples of valid RDBMS data source configurations.

### RDBMS minimum data source configuration

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "DSN=MyDSN"
  }
]
```

### RDBMS data source configuration with username and password

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Driver={ODBC Driver 17 for SQL Server}; Server=ServerName\\SQLEXPRESS; UID=[username]; PWD=[password]",
    "UserName": "MyUser",
    "Password": "MyPassword"
  }
]
```

### RDBMS data source configuration with Windows authentication

```json
[
  {
    "DefaultStreamIdPattern": "{QueryId}.{ValueColumn}",
    "ConnectString": "Driver={ODBC Driver 17 for SQL Server}; Server=ServerName\\SQLEXPRESS; trusted_connection=yes",
    "UserName": "MyUser",
    "Password": "MyPassword",
    "WindowsAuth": true
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
