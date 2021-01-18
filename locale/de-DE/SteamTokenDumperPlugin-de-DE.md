# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` ist seit ASF V4.2.2.2 ein offizielles ASF-**[Plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins)**, welches von uns entwickelt wird. Dies erlaubt es dir, zum Projekt **[SteamDB](https://steamdb.info)** beizutragen, indem du Paket- und App-Tokens sowie Depot-Schlüssel, auf die dein Steam Konto Zugriff hat, teilst. Informationen zu den gesammelten Daten und warum SteamDB diese benötigt, findest du auf der **[Token Dumper](https://steamdb.info/tokendumper)** Website von SteamDB. Die übermittelten Daten enthalten demnach keine potentiell sensiblen Informationen und bergen kein Sicherheits-/Datenschutzrisiko.

---

## Aktivierung des Plugins

Das `SteamTokenDumperPlugin` ist in der Releaseversion von ASF enthalten, jedoch ist das Plugin standardmäßig deaktiviert. Sie können das Plugin aktivieren, indem Sie die globale ASF Konfigurationseigenschaft `SteamTokenDumperPluginEnabled` auf `true` setzen. Im JSON-Syntax:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

Beim Start von ASF teilt das Plugin über den Standard-Protokollierungsmechanismus mit, ob es erfolgreich aktiviert wurde. Sie können das Plugin auch über unseren webbasierten Konfigurationsgenerator aktivieren.

---

## Technische Details

Nach der Aktivierung verwendet das Plugin die Bots, die Sie in ASF betreiben, zur Datenerfassung in Form von Paket- und App-Tokens sowie Depot-Schlüsseln, auf die Ihre Bots Zugriff haben. Das Datenerfassungsmodul umfasst passive und aktive Routinen, die den durch die Datenerfassung verursachten zusätzlichen Aufwand minimieren sollen.

Um den geplanten Anwendungsfall zu erfüllen, wird zusätzlich zu der oben erläuterten Datenerfassungsroutine die Einreichungsroutine initialisiert, die dafür verantwortlich ist, zu bestimmen, welche Daten auf periodischer Basis an SteamDB übermittelt werden müssen. Diese Routine wird innerhalb von `1` Stunde nach dem Start von ASF ausgelöst und wiederholt sich alle `24` Stunden. Das Plugin wird sein Bestes tun, um die Menge der zu sendenden Daten zu minimieren. Daher ist es möglich, dass einige Daten, die das Plugin sammelt, als unbrauchbar zur Übermittlung eingestuft und daher übersprungen werden (z. B. ein App-Update, das das Zugriffstoken nicht ändert).

Das Plugin verwendet eine dauerhafte Cache-Datenbank, die in `config/SteamTokenDumper.cache` gespeichert ist, die einen ähnlichen Zweck wie `config/ASF.db` für ASF erfüllt. Die Datei wird verwendet, um die gesammelten und übermittelten Daten aufzuzeichnen und den Arbeitsaufwand, der bei verschiedenen ASF-Ausführungen anfällt, zu minimieren. Das Entfernen der Datei führt dazu, dass der Prozess von Grund auf neu gestartet wird, was nach Möglichkeit vermieden werden sollte.

---

## Daten

ASF schließt die `steamID` des Mitwirkenden in die Anfrage ein, die als `SteamOwnerID` bestimmt wird, die Sie in ASF festgelegt haben oder, falls Sie das nicht getan haben, die Steam-ID des Bots, der die meisten Lizenzen besitzt. Der angegebene Mitwirkende könnte einige zusätzliche Vorteile von SteamDB für kontinuierliche Hilfe erhalten (z. B. Spenderrang auf der Website), aber das liegt ganz im Ermessen von SteamDB.

In jedem Fall möchten sich die SteamDB-Mitarbeiter im Voraus für Ihre Hilfe bedanken. Die übermittelten Daten ermöglichen den Betrieb von SteamDB, insbesondere die Verfolgung von Informationen über Pakete, Anwendungen und Depots, was ohne Ihre Hilfe nicht mehr möglich wäre.