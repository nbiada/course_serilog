# Serilog - Log levels

## Livelli

### Verbose
Debugging di basso livello low-level, informazioni interne

### Debug
Debugging di basso livello, ma superiore al precedente. Informazoni diagnostiche.

### Information
Informazioni di alto livello, non _interne_ al sistema.\
Utile per inserire informazioni di contesto a livello applicativo.

### Warning
Informazioni su _possibili_ problemi che potrebbero avvenire al sistema.\
Utile per garantire filtri su messaggistica molto ampia differenziando da __Information__

### Error
Situazioni non previste che portano ad un blocco del sistema.\
Indicano uno stato in cui il sistema non è più in grado di proseguire nelle operazioni previste.

### Fatal
Identificano eventi catastrofici a livello di infrastruttura generale.\
Tendednzialmente da utilizzare per blocchi a livello più basso della catena ISO/OSI.

&nbsp;

## Utilizzo
Il livello minimo di default è __Information__ e superiori.

```csharp
...
  Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateLogger()
...
Log.Verbose("Application is starting by server {ServerName}", ServerName);

Log.Debug("Application started at {DateStarted}", AppDateTimeStart);

Log.Information("New user {UserName} added", Name);

Log.Warning("Warning of low parts availability");

try {
    int divideByZero = firstNumber / secondNumber
}
catch (Exception ex) {
    Log.Error(ex, "Divided by zero {First} / {Second}", firstNumber, secondNumber);
}
```
```console
2020-08-30 08:55:02 [Information] New user NICOLA added
2020-08-30 08:55:03 [Warning] Warning of low parts availability
2020-08-30 08:55:03 [Error] Divided by zero 10 / 0
  at ConsoleApplication.Program.Main(String[] args) in c:\dev\testSerilog\Program.cs:line 120
```

con cambiamento del livello minimo a __Verbose__
```csharp
...
  Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Verbose()
    .WriteTo.Console()
    .CreateLogger()
...
```

```console
2020-08-30 08:55:00 [Verbose] Application is starting by server SERVER-A
2020-08-30 08:55:01 [Debug] Application started at 2020-08-30T08:55:01.455654+01
2020-08-30 08:55:02 [Information] New user NICOLA added
2020-08-30 08:55:03 [Warning] Warning of low parts availability
2020-08-30 08:55:03 [Error] Divided by zero 10 / 0
  at ConsoleApplication.Program.Main(String[] args) in c:\dev\testSerilog\Program.cs:line 120
```
