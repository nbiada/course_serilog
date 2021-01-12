# Serilog - Formattazione avanzata

## Formattazione

### Standard
```csharp
...
Log.Information("Added user {UserName}, Age {UserAge}", name, age);
```
Risultato:
```console
Added user "Nicola", Age 50
```

### Literal (l)
```csharp
...
Log.Information("Added user {UserName:l}, Age {UserAge}", name, age);
```
Risultato:
```console
Added user Nicola, Age 50
```
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Added user {UserName:l}, Age {UserAge}",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Added user Nicola, Age 50",
    "Properties": {
        "UserName": "Nicola",
        "UserAge" : 50
    }
}
```

### Numeric
```csharp
...
Log.Information("Total cost is {TotalCost:0.00}", totalCost);
```
```console
2020-08-30 08:55:00 [Information] Total cost is 1234.56
```
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Total cost is {TotalCost:0.00}",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Total cost is 1234.56",
    "Properties": {
        "totalCost": 1234.568979
    }
}
```
