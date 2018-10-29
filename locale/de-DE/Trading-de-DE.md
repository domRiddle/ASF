# Handel

ASF beinhaltet Unterstützung für Steam nicht-interaktive (Offline-) Handelsangebote. Sowohl das Empfangen (Akzeptieren/Ablehnen) als auch das Senden von Handelsangeboten ist sofort verfügbar und erfordert keine spezielle Konfiguration, außer natürlich ein uneingeschränktes Steam-Konto (dasjenige, das bereits 5€ im Shop ausgegeben hat). Das Handelsmodul ist für eingeschränkte Konten nicht verfügbar.

Hinweis: Jedes Mal, wenn das Wort "Ablehnen" verwendet wird, bedeutet dies entweder ignorieren oder ablehnen, je nach konfigurierter `BotBehaviour` (`RejectInvalidTrades`) Eigenschaft.

* * *

## Logik

ASF akzeptiert immer alle Trades, unabhängig von Gegenständen, die vom Benutzer mit `Master` (oder höherem) Zugriff auf den Bot gesendet werden. Dies ermöglicht nicht nur das einfache Plündern von Steam-Karten, die von der Bot-Instanz gesammelt werden, sondern auch das einfache Verwalten von Steam-Gegenständen, die im Inventar aufbewahrt werden.

ASF lehnt Handelsangebote, unabhängig vom Inhalt, von jedem (Nicht-Master) Benutzer ab, der auf der schwarzen Liste des Handelsmoduls steht. Die schwarze Liste ist in der Standard-Datenbank `BotName.db` gespeichert und kann über `bl`, `bladd` und `blrm` **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** verwaltet werden. Dies sollte als Alternative zu dem von Steam angebotenen Standard-Benutzerblock funktionieren - Verwendung mit Bedacht.

ASF akzeptiert alle `loot`-ähnlichen Handelsangebote, die über Bots gesendet werden, es sei denn, `DontAcceptBotTrades` ist in `TradingPreferences` angegeben. Kurz gesagt, der Standard `TradingPreferences` von `None` bewirkt, dass ASF automatisch Handelsangebote von Benutzern mit `Master` Zugriff auf den Bot (siehe oben) akzeptiert, sowie alle Spenden-Handelsangebote von anderen Bots, die am ASF-Prozess teilnehmen. Wenn du Spenden-Handelsangebote von anderen Bots deaktivieren möchtest, dann ist das das, wofür `DontAcceptBotTrades` in deinen `TradingPreferences` ist.

Wenn du `AcceptDonations` in deinen `TradingPreferences` aktivierst, akzeptiert ASF auch jedes Spenden-Handelsangebot, bei dem das Bot-Konto keine Gegenstände verliert. Diese Eigenschaft betrifft nur Nicht-Bot-Konten, da Bot-Konten von `DontAcceptBotTrades` betroffen sind. `AcceptDonations` ermöglicht es dir, problemlos Spenden von anderen Personen anzunehmen, aber auch Bots, die nicht am ASF-Prozess teilnehmen.

Es ist gut zu erwähnen, dass `AcceptDonations` kein **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** erfordert, da es keine Bestätigung braucht, wenn wir keine Gegenstände verlieren.

Du kannst die ASF-Handelsmöglichkeiten auch weiter anpassen, indem du `TradingPreferences` entsprechend anpasst. Eine der Hauptfunktionen von `TradingPreferences` ist die Option `SteamTradeMatcher`, die ASF veranlasst, eine eingebaute Logik für die Annahme von Handelsangeboten zu verwenden, die dir hilft, fehlende Abzeichen zu vervollständigen, was besonders nützlich in Zusammenarbeit mit dem öffentlichen Inserieren von **[SteamTradeMatcher](https://www.steamtradematcher.com)** ist, aber auch ohne diese arbeiten kann. Es wird im Folgenden näher beschrieben.

* * *

## SteamTradeMatcher

Wenn `SteamTradeMatcher` aktiv ist, wird ASF einen ziemlich komplexen Algorithmus verwenden, um zu überprüfen, ob das Handelsangebot die STM-Regeln erfüllt und zumindest neutral gegenüber steht. Die eigentliche Logik ist die folgende:

- Lehne das Handelsangebot ab, wenn wir etwas anderes verlieren als die in unseren `MatchableTypes` angegebenen Objekttypen.
- Lehne das Handelsangebot ab, wenn wir nicht mindestens die gleiche Anzahl an Gegenständen pro Spiel und pro Typ erhalten.
- Lehne das Handelsangebot ab, wenn der Benutzer nach speziellen Steam Sommer/Winter-Verkaufskarten fragt und eine Handelssperre hat.
- Lehne das Handelsangebot ab, wenn die Dauer der Handelssperre `MaxTradeHoldDuration` globale Konfigurationseigenschaft überschreitet.
- Lehne das Handelsangebot ab, wenn wir nicht `MatchEverything` gesetzt haben und es für uns schlimmer als neutral ist.
- Akzeptiere das Handelsangebot, wenn wir ihn nicht durch einen der oben genannten Punkte abgelehnt haben.

Es ist nett zu erwähnen, dass ASF auch Überzahlungen unterstützt - die Logik wird richtig funktionieren, wenn der Benutzer dem Handelsangebot etwas mehr hinzufügt, solange alle oben genannten Bedingungen erfüllt sind.

Die ersten 4 Ablehnungsprädikate sollten für jeden offensichtlich sein. Das letzte beinhaltet die Logik der tatsächlichen Duplikate, die den aktuellen Zustand unseres Inventars überprüft und entscheidet, was der Status des Handelsangebotes ist.

- Das Handelsangebot ist **gut**, wenn unser Fortschritt in Richtung Fertigstellung voranschreitet. A A (vorher) <-> A B (nachher)
- Das Handelsangebot ist **neutral**, wenn unser Fortschritt bei der Fertigstellung intakt bleibt. A B (vorher) <-> A C (nachher)
- Das Handelsangebot ist **schlecht**, wenn unser Fortschritt in Richtung Fertigstellung zurückgeht. A C (vorher) <-> A A (nachher)

STM arbeitet nur mit guten Handelsangeboten, was bedeutet, dass Benutzer, die STM für den Duplikatabgleich verwenden, uns immer nur gute Trades vorschlagen sollten. ASF ist jedoch liberal und akzeptiert auch neutrale Handelsangebote, denn in diesen Handelsangeboten verlieren wir nicht wirklich etwas, so dass es keinen wirklichen Grund gibt, sie abzulehnen. Dies ist besonders nützlich für deine Freunde, da sie deine überschüssigen Karten ohne STM austauschen können, solange du keinen festen Fortschritt verlierst.

Standardmäßig lehnt ASF schlechte Handelsangebote ab - das ist fast immer das, was du als Benutzer willst. Du kannst jedoch optional `MatchEverything` in deinen `TradingPreferences` aktivieren, um ASF dazu zu bringen, alle Duplikat-Handelsangebote zu akzeptieren, einschließlich **schlechte Handelsangebote**. Dies ist nur dann nützlich, wenn du einen 1:1-Handels-Bot unter deinem Account betreiben möchtest, da du verstehst, dass **ASF dir nicht mehr helfen wird, Fortschritte bei der Vervollständigung von Abzeichen zu erzielen, und dich anfällig für den Verlust des gesamten fertigen Sets für N Duplikate derselben Karte macht**. Wenn du nicht absichtlich einen Handels-Bot ausführen möchtest, der **nie** ein Set vervollständigt, willst du diese Option nicht aktivieren.

Unabhängig von den von dir gewählten `TradingPreferences` bedeutet ein von ASF abgelehnter Handelsangebot nicht, dass du es nicht selbst akzeptieren kannst. Wenn du den Standardwert von `BotBehaviour` beibehalten hast, der `Keine` ist, ignoriert ASF diese Handelsangebote einfach - so kannst du selbst entscheiden, ob du an ihnen interessiert bist oder nicht. Gleiches gilt für Handelsangebote mit Gegenständen außerhalb von `MatchableTypes`, sowie für alles andere - das Modul soll dir helfen, STM-Handelsangebote zu automatisieren, nicht zu entscheiden, was ein gutes Handelsangebot ist und was nicht. Die einzige Ausnahme von dieser Regel ist, wenn es um Benutzer geht, die du vom Handelsmodul aus mit dem Befehl `bladd` auf die schwarze Liste gesetzt hast - Handelsangebote von diesen Benutzern werden unabhängig von den Einstellungen von `BotBehaviour` sofort abgelehnt.

Es wird dringend empfohlen, **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)** zu verwenden, wenn du diese Option aktivierst, da diese Funktion ihr ganzes Potenzial verliert, wenn du dich entscheidest, jedes Handelsangebot manuell zu bestätigen. `SteamTradeMatcher` funktioniert auch ohne die Möglichkeit, Handelsangebote zu bestätigen, aber es könnte einen Rückstand an Bestätigungen erzeugen, wenn du sie nicht rechtzeitig akzeptierst.