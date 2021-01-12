# Serilog - Context & Filtering

## Context
```csharp
var contextLog = Log.Logger.ForContext<Program>();

contextLog.Information("Hey {Name}!", FirstName);
contextLog.Information("Starting from Server {ServerName}!", TheServerName);
```
```json
{
    ...
    "Properties": {
        "Name": "Nicola",
        "ServerName": "SERVER-A",
        "SourceContext": "ConsoleApplication.Program"
    }
}
```

## Filtering
```csharp
...
  Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Verbose()
    .WriteTo.Console()
    .WriteTo.RavenDB(logDocumentStore)
    .Filter.ByExcluding(x => x.Properties.ContainsKey("ServerName"))
    .Filter.ByIncludingOnly(x => x.MessageTemplate.Text.Contains("Starting from"))
    .Filter.ByExcluding(Matching.WithProperty("ServerName"))
    .Filter.ByExcluding(Matching.FromSource<Program>())
    .Filter.ByExcluding(Matching.FromSource("ConsoleApplication"))
    .CreateLogger();
...
```

## Correlazioni
```csharp
private void SendConfirmationEmail(int BookId) 
{
    var emailFrom = "automatic@mycompany.com";

    var corrLog = Log.Logger.ForContext("Booking", $"BOOK-{BookId}");

    corrLog.Information("Sent email");
    ...
    corrLog.Error(ex, "Error sending email");
}

private void BookIt(int BookId) 
{
    var bookingHotel = "Hotel Mario";

    var corrLog = Log.Logger.ForContext("Booking", $"BOOK-{BookId}");

    corrLog.Information("Booked for {Hotel}", bookingHotel);
}
```
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Sent email",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Sent email",
    "Properties": {
        "Booking": "BOOK-123"
    }
}
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Booked for {Hotel}",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Booked for Hotel Mario",
    "Properties": {
        "Booking": "BOOK-123"
    }
}

```

### Ricerca in RavenDB
```sql
Properties.Booking: BOOK-123
```


## Proprietà fisse
```csharp
...
  Log.Logger = new LoggerConfiguration()
    .Enrich.WithProperty("ApplicationName", "BookingSystem")
    .WriteTo.Console()
    .WriteTo.RavenDB(logDocumentStore)
    .CreateLogger();
...
```
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Sent email",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Sent email",
    "Properties": {
        "Booking": "BOOK-123",
        "ApplicationName": "BookingSystem"
    }
}
```

### Custom Enricher
```csharp
...
  Log.Logger = new LoggerConfiguration()
    .Enrich.With(new ApplicationDetailsEnricher())
    .WriteTo.Console()
    .WriteTo.RavenDB(logDocumentStore)
    .CreateLogger();
...

public class ApplicationDetailsEnricher: ILogEventEnricher
{
    public void Enrich(LogEvent logEvent, ILogEventPropertyFactory propertyFactory)
    {
        var applicationAssembly = Assembly.GetEntryAssembly();

        var name = applicationAssembly.GetName().Name;
        var version = applicationAssembly.GetName().Version;

        logEvent.AddPropertyIfAbsent(
            propertyFactory.CreateProperty("ApplicationName", name)
        );
        logEvent.AddPropertyIfAbsent(
            propertyFactory.CreateProperty("ApplicationVersion", version)
        );
    }
}
```
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "Sent email",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "Sent email",
    "Properties": {
        "Booking": "BOOK-123",
        "ApplicationName": "BookingSystem",
        "ApplicationVersion": "1.0.0.0"
    }
}
```
