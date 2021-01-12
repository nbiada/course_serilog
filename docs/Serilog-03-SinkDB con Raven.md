# Serilog - Sink DB con RavenDB

## RavenDB
### Impostazione
```csharp
public Creator() {
    const string customTemplate = "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level}] {Message}{NewLine}{Exception}";

    var logDocumentStore = new DocumentStore {
        Url = "http://localhost:8080",
        DefaultDatabase = "logs"
    }

    logDocumentStore.Initialize();

    ILogger logger = new LoggerConfiguration()
        .WriteTo.RavenDB(logDocumentStore)
        .CreateLogger();
    
    Log.Logger = logger;
}
```
### Scrittura
```csharp
class User 
{
    public string Name { get; set; }
    public string Age { get; set; }
}
var user = new User
    {
        Name = "Nicola",
        Age = 50
    };

Log.Information("New user -> {NewUser}", @user);
```
Risultato:
| Id | Exception | Level | MessageTemplate | Properties | RenderMessage | Timestamp |
| -- | --------- | ----- | --------------- | ---------- | ------------- | --------- |
| 1 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Nicola", "Age": 50}} | New User -> {\\"Name\\":\\"Nicola\\", \\"Age\\": 50} | 2020-08-30T08:55:00.23376+01:00 |
| 2 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Stefania", "Age": 46}} | New User -> {\\"Name\\":\\"Stefania\\", \\"Age\\": 46} | 2020-08-29T08:55:00.23376+01:00 |
| 3 | _null_ | Information | Hi from {City} | {"City":"Rome"} | Hi from \\"Rome\\" | 2020-08-27T08:55:00.23376+01:00 |

&nbsp;
Json & Properties:
```json
{
    "Timespamp": "2020-08-30T08:55:00.23376+01:00",
    "MessageTemplate": "New user -> {NewUser}",
    "Level": "Information",
    "Exception": null,
    "RenderedMessage": "New User -> {\"Name\":\"Nicola\", \"Age\": 50}",
    "Properties": {
        "Name": "Nicola",
        "Age" : 50
    }
}
```
### Ricerca
Sintassi Query:
```sql
RenderedMessage: "New user*"
```
Risultato:
| Id | Exception | Level | MessageTemplate | Properties | RenderMessage | Timestamp |
| -- | --------- | ----- | --------------- | ---------- | ------------- | --------- |
| 1 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Nicola", "Age": 50}} | New User -> {\\"Name\\":\\"Nicola\\", \\"Age\\": 50} | 2020-08-30T08:55:00.23376+01:00 |
| 2 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Stefania", "Age": 46}} | New User -> {\\"Name\\":\\"Stefania\\", \\"Age\\": 46} | 2020-08-29T08:55:00.23376+01:00 |

&nbsp;  

```sql
RenderedMessage: "New user*" AND Properties.NewUser.Name: "Nicola"
```
Risultato:
| Id | Exception | Level | MessageTemplate | Properties | RenderMessage | Timestamp |
| -- | --------- | ----- | --------------- | ---------- | ------------- | --------- |
| 1 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Nicola", "Age": 50}} | New User -> {\\"Name\\":\\"Nicola\\", \\"Age\\": 50} | 2020-08-30T08:55:00.23376+01:00 |

&nbsp;  

Ricerca per range di valori numerici (da - a)
```sql
Properties.NewUser.Age: [30 60]
```
Risultato:
| Id | Exception | Level | MessageTemplate | Properties | RenderMessage | Timestamp |
| -- | --------- | ----- | --------------- | ---------- | ------------- | --------- |
| 1 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Nicola", "Age": 50}} | New User -> {\\"Name\\":\\"Nicola\\", \\"Age\\": 50} | 2020-08-30T08:55:00.23376+01:00 |
| 2 | _null_ | Information | New user -> {NewUser} | {"NewUser": {"Name":"Stefania", "Age": 46}} | New User -> {\\"Name\\":\\"Stefania\\", \\"Age\\": 46} | 2020-08-29T08:55:00.23376+01:00 |

