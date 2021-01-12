# Serilog - Concetti Base

## Referenza NuGet
Per prima cosa è necessario aggiungere una referenza a `Serilog` package.

## Codice 
Il codice sotto riportato è la minima configurazione possibile per inviare alla console i messaggi di log strutturato.

```csharp
// Log informations
ILogger logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateLogger();

Log.Logger = logger;
Log.Information("This is an information");
```

### Scrittura su file
Per consentire una scrittua su file:
```csharp
public Creator() {
    ILogger logger = new LoggerConfiguration()
        .WriteTo.File("logfile.txt")
        // .WriteTo.Console()
        .CreateLogger();
    
    Log.Logger = logger;
}

private void TheMethod() {
    Log.Information("This is an information");
}
```

Per consentire un _custom template_ del log:
```csharp
public Creator() {
    const string customTemplate = "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level}] {Message}{NewLine}{Exception}";

    ILogger logger = new LoggerConfiguration()
        .WriteTo.File("logfile.txt", outputTemplate: customTemplate)
        .CreateLogger();
    
    Log.Logger = logger;
}

private void TheMethod() {
    Log.Information("This is an information");
}
```
Il limite in byte della dimensione di un file di log è di __1 Gb__.
Se impostiamo un limite di dimensione e viene raggiunto il file non riceverà ulteriori righe.
```csharp
    ...
    ILogger logger = new LoggerConfiguration()
        .WriteTo.File("logfile.txt", 
            outputTemplate: customTemplate,
            fileSizeLimitByte: 300)
        .CreateLogger();
```
Se imposto il parametro `fileSizeLimitByte` a `null` non verrà più controllata la dimensione massima e continuerà a loggare fino al limite del disco.

### Rolling file
Impostando la modalità `RollingFile` Serilog creerà un nuovo file ogni giorno.\
Il file che veiene creato avrà in automatico la data appesa al nome.\
In questo caso il file si chiamerà __logfile-20200829.txt__
```csharp
    ...
    ILogger logger = new LoggerConfiguration()
        .WriteTo.RollingFile("logfile.txt", 
            outputTemplate: customTemplate,
            fileSizeLimitByte: 300)
        .CreateLogger();
```

### Limite file
Aggiungere un limite al numero di file da creare
```csharp
    ...
    ILogger logger = new LoggerConfiguration()
        .WriteTo.RollingFile("logfile.txt",
            retainedFileCountLimit: 2, 
            outputTemplate: customTemplate)
        .CreateLogger();
```
