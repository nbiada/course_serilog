# Serilog - Multi Sink level

## Console e File
```csharp
...
  Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Verbose()
    .WriteTo.Console(restrictedToMinimumLevel: LogEventLevel.Information)
    .WriteTo.File("logfile.txt", restrictedToMinimumLevel: LogEventLevel.Debug)
    .CreateLogger();
...
```
Output in Console
```console
2020-08-30 08:55:02 [Information] New user NICOLA added
2020-08-30 08:55:03 [Warning] Warning of low parts availability
2020-08-30 08:55:03 [Error] Divided by zero 10 / 0
  at ConsoleApplication.Program.Main(String[] args) in c:\dev\testSerilog\Program.cs:line 120
```
Output in File
```console
2020-08-30 08:55:01 [Debug] Application started at 2020-08-30T08:55:01.455654+01
2020-08-30 08:55:02 [Information] New user NICOLA added
2020-08-30 08:55:03 [Warning] Warning of low parts availability
2020-08-30 08:55:03 [Error] Divided by zero 10 / 0
  at ConsoleApplication.Program.Main(String[] args) in c:\dev\testSerilog\Program.cs:line 120
```

