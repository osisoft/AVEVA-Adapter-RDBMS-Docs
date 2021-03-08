---
uid: PIAdapterForRDBMSConfigurationExamples
---

# PI Adapter for RDBMS configuration examples

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
              "Endpoint": "http://localhost:/api/v1/tenants/default/namespaces/default/omf"
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
