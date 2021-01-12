# Serilog - Dati Strutturati

## Dati strutturati

### Scalar Datatypes
```csharp
...
Log.Information("Added user {UserName}, Age {UserAge}", name, age);
```
Risultato:
```console
Added user "Nicola", Age 50
```

### Class Datatypes
```csharp
...
class User 
{
    public string Name { get; set; }
    public string Age { get; set; }
}
Log.Information("Added user {@User}", user);
```

### Enumeratori
```csharp
...
IEnumerable favColors = new List<string>
    {
        "red","orange","black"
    };

Log.Information("My fav colors: {colors}", favColors);
```
Risultato:
```console
My fav colors: ["red", "orange", "black"]
```

### Dictionary
```csharp
...
var countries = new Dictionary<string, int>
    {
        {"Italy", 60},
        {"Germany", 200},
        {"Switzerland", 14}
    };

Log.Information("Country and population: {ContriesPopulation}", countries);
```
Risultato:
```console
Country and population: : [("Italy", 60), ("Germany", 200), ("Switzerland", 14)]
```

### Classi
```csharp
...
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

Log.Information("New user -> {NewUser}", user);
```
Risultato:
```console
New user -> "Namespace+User"
```

Aggiungendo un overriding del metodo ToString:

```csharp
...
class User 
{
    public string Name { get; set; }
    public string Age { get; set; }

    public override string ToString() {
        return string.Format("Name: {0} - Age: {1}", Name, Age);
    }
}
var user = new User
    {
        Name = "Nicola",
        Age = 50
    };

Log.Information("New user -> {NewUser}", user);
```
Risultato:
```console
New user -> "Name: Nicola - Age: 50"
```

### Oggetti destrutturati
```csharp
...
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
```console
New user -> User { Name: "Nicola", Age: 50 }
```
