# Serilog - Pacchetti aggiuntivi

## NuGet
`Serilog.Extras.AppSettings`

settaggi in file esterno, non può essere mischiato con i settaggi da codice

`Serilog.Extras.Web`

aggiunge diversi Enricher (HttpRequestId, HttpRequestNumber, etc.)
aggiunge log di Url e metodi, post data, unhandled exceptions

`Serilog.Extras.Timing`

informazioni relative alla durata delle operazioni\
aggiungendo specifici tag per inizio e fine
```console
[Information] Start ...
[Information] End ...
[Warning] Time exceeded or too long ...
```
proprietà specifiche (TimedOperationId, TimedOperationDescription, TimedOperationElapsed)

