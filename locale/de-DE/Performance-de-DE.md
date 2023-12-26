# Leistungseffizienz

Das Hauptziel von ASF ist es, so effektiv wie möglich zu sammeln, basierend auf zwei Arten von Daten, mit denen es arbeiten kann – einem kleinen Satz von benutzerdefinierten Daten, die für ASF unmöglich sind selbst zu erraten/kontrollieren, und einem größeren Satz von Daten, der von ASF automatisch überprüft werden kann.

Im Automatikmodus erlaubt ASF Ihnen weder die Spiele auszuwählen, die gesammelt werden sollen – noch den Algorithmus für das Karten sammeln zu ändern. **ASF weiß besser als Sie, was es tun und welche Entscheidungen es treffen sollte, um so schnell wie möglich zu sammeln**. Ihr Ziel ist es, die Konfigurationseigenschaften richtig einzustellen, da ASF sie nicht alleine erraten kann, alles andere ist abgedeckt.

---

Vor einiger Zeit hat Valve den Algorithmus um Karten zu erhalten geändert. Von diesem Zeitpunkt an können wir Steam-Konten nach zwei Kategorien kategorisieren: die **mit eingeschränkten** Kartenfunde und die **ohne**. Der einzige Unterschied zwischen diesen beiden Typen ist die Tatsache, dass Konten mit eingeschränkten Kartenfunde keine Karten aus dem gegebenen Spiel erhalten können, bis sie das gegebene Spiel für mindestens `X` Stunden spielen. Es hat den Anschein, dass ältere Konten, die nie um Rückerstattung gebeten haben, **unbeschränkte Kartenfunde** haben, während neue Konten und diejenigen, die um Rückerstattung gebeten haben, **beschränkte Kartenfunde** haben. Dies ist jedoch nur eine Theorie und sollte nicht als Tatsache betrachtet werden. Deshalb gibt es **keine offensichtliche Antwort** und ASF verlässt sich darauf, dass **Sie**  ihm sagen, welcher Fall für Ihr Konto geeignet ist.

---

ASF beinhaltet derzeit zwei Sammel-Algorithmen:

**Simple** (einfacher) Algorithmus funktioniert am zuverlässigsten bei Konten, die über unbegrenzte Kartenfunde verfügen. Dies ist der primäre Algorithmus, der von ASF verwendet wird. Der Bot findet Spiele für das Sammeln und sammelt sie einzeln, bis alle Karten gesammelt wurden. Dies liegt daran, dass die Karten-Sammel-Rate beim Sammeln von mehr als einem Spiel gegen Null tendiert und völlig ineffektiv ist.

**Complex** (komplex) ist ein neuer Algorithmus, der implementiert wurde, um auch eingeschränkten Konten zu helfen, Ihre Gewinne zu maximieren. ASF wird zunächst den Standardalgorithmus **Simple** für alle Spiele verwenden, die `HoursUntilCardDrops` Stunden Spielzeit überschritten haben; dann, wenn keine Spiele mit >= `HoursUntilCardDrops` Stunden übrig sind, wird es alle Spiele (innerhalb eines Limits von `32`) mit < `HoursUntilCardDrops` Stunden gleichzeitig sammeln, bis eines von ihnen die `HoursUntilCardDrops` Stunden Marke erreicht hat; schließlich wird ASF die Schleife von Anfang an fortsetzen („verwende **Simple** auf diesem Spiel, setze die gleichzeitigen Spiele auf < `HoursUntilCardDrops` fort und so weiter …“). In diesem Fall können wir „Mehrere-Spiele-Sammeln“ verwenden, um die Stunden der Spiele, die wir sammeln müssen, zuerst auf einen angemessenen Wert zu bringen. Bedenken, dass ASF während des Stunden-Sammelns **keine** Karten sammelt. Daher wird es auch nicht prüfen, ob während dieses Zeitraums Karten gesammelt wurden (aus den oben genannten Gründen).

Derzeit wählt ASF den Karten-Sammel-Algorithmus, der ausschließlich auf der Konfigurationsvariable `HoursUntilCardDrops` basiert (die **Sie** festlegen). Wenn `HoursUntilCardDrops` auf `0` eingestellt ist, wird der Algorithmus **Simple** verwendet; andernfalls **Complex** wird stattdessen verwendet – konfiguriert, um die Spielzeit in allen Spielen so zu erhöhen, dass die Anzahl der Stunden vor dem Sammeln der Kartenfunde angegeben wird.

---

### **Es gibt keine klare Antwort, welcher Algorithmus für Sie besser ist**.

Dies ist einer der Gründe, warum Sie keinen Karten-Sammel-Algorithmus wählen, sondern ASF mitteilen, ob das Konto eingeschränkte Kartenfunde hat oder nicht. Wenn das Konto nicht eingeschränkte Kartenfunde hat, wird auf diesem Konto der Algorithmus „**Simple**“ **besser funktionieren**, da wir keine Zeit damit verschwenden werden, alle Spiele auf `X` Stunden zu bringen – die Funde-Rate der Karten liegt bei mehreren Spielen nahe bei 0 %. Auf der anderen Seite, wenn Ihr Konto eingeschränkt Kartenfunde hat, wird der Algorithmus **Complex** für Sie besser sein, da es keinen Sinn ergibt, allein zu arbeiten, wenn das Spiel `HoursUntilCardDrops` Stunden bisher nicht erreicht hat. Also werden wir zuerst die **Spieldauer** und **dann** die Karten im Solomodus sammeln.

`HoursUntilCardDrops` sollte nicht blindlings gesetzt werden, nur weil Ihnen jemand gesagt hat Sie sollen es tun. Sie sollten Tests durchführen, Ergebnisse vergleichen und basierend auf den Daten, die Sie bekommen, entscheiden, welche Option für Sie besser sein sollte. Sofern Sie minimalen Aufwand dafür betreiben, werden Sie sicherstellen, dass ASF mit größtmöglicher Effizienz für Ihr Konto arbeitet, was wahrscheinlich das ist, was Sie wollen, wenn man bedenkt, dass Sie diese Wiki-Seite gerade lesen. Wenn es eine Lösung gäbe, die für alle funktioniert, hätten Sie keine Wahl – ASF würde selbst entscheiden.

---

### Was ist der beste Weg, um herauszufinden, ob Ihr Konto eingeschränkt ist?

Stellen Sie sicher, dass Sie einige Spiele (vorzugsweise 5+) ohne aufgezeichnete Spielzeit zum Sammeln haben, und führen Sie ASF mit `HoursUntilCardDrops` von `0` aus. Es wäre eine gute Idee, wenn Sie während des Sammelns nichts spielen würden, um genauere Ergebnisse zu erzielen (am besten ASF nachts laufen lassen). Lassen Sie  ASF diese 5 Spiele sammeln und schauen Sie sich danach das Protokoll an, um die Ergebnisse zu sehen.

ASF zeigt deutlich an, wann eine Karte für ein bestimmtes Spiel gesammelt wurde. Sie möchten den schnellsten Kartenfund den ASF erreicht hat. Wenn Ihr Konto beispielsweise uneingeschränkt ist, sollte ein erster Kartenfund nach ungefähr 30 Minuten seit Beginn des Sammelns erfolgen. Wenn Sie **mindestens ein** Spiel entdecken, bei dem die Karte in den ersten 30 Minuten gesammelt wurde, dann ist dies ein Indikator dafür, dass Ihr Konto **nicht** eingeschränkt ist und `HoursUntilCardDrops` von `0` verwendet werden sollte.

Falls Sie andererseits bemerken, dass **jedes** Spiel mindestens `X` Stunden benötigt, bevor die ersten Karten gesammelt werden, dann ist dies ein Indikator für Sie, `HoursUntilCardDrops` anzupassen. Die Mehrheit (wenn nicht gar alle) der eingeschränkten Benutzer benötigen mindestens `3` Stunden Spielzeit, damit die Karten gesammelt werden und dies ist auch der Standardwert für die Einstellung `HoursUntilCardDrops`.

Bedenken Sie, dass Spiele unterschiedliche Fund-Raten haben können. Deshalb sollten Sie testen, ob Ihre Theorie mit **mindestens** 3 Spielen stimmt, vorzugsweise 5+, um sicherzustellen, dass Sie nicht durch einen Zufall auf falsche Ergebnisse stoßen. Ein Kartenfund von einem Spiel in weniger als einer Stunde ist eine Bestätigung, dass Ihr Konto **nicht** eingeschränkt ist und `HoursUntilCardDrops` von `0` verwendet werden kann; aber um zu bestätigen, dass Ihr Konto **eingeschränkt** ist, brauchen Sie mindestens mehrere Spiele, bei denen keine Karten gesammelt wurden, bis Sie eine feste Marke erreicht haben.

Es ist wichtig zu beachten, dass in der Vergangenheit `HoursUntilCardDrops` nur `0` oder `2` war und deshalb hatte ASF eine einzige `CardDropsRestricted` Variable, die es erlaubte, zwischen diesen beiden Werten zu wechseln. Mit den jüngsten Änderungen stellten wir fest, dass jetzt nicht nur die Mehrheit der Benutzer `3` Stunden anstelle der vorherigen `2` benötigt, sondern auch, dass `HoursUntilCardDrops` jetzt dynamisch ist und jeden Wert pro Konto treffen kann.

Am Ende liegt die Entscheidung natürlich bei Ihnen.

Und um es noch schlimmer zu machen – ich habe Fälle erlebt in denen Leute von einem eingeschränkten in einen uneingeschränkten Zustand und umgekehrt gewechselt haben – entweder wegen eines Steam-Bugs (oh ja, es gibt viele davon) oder wegen einiger Änderungen an der Logik seitens Valve. Selbst wenn Sie also bestätigt haben, dass Ihr Konto eingeschränkt ist (oder nicht), glauben Sie nicht, dass es so bleiben wird – um von uneingeschränkt zu eingeschränkt zu wechseln genügt es eine Rückerstattung zu verlangen. Wenn Sie das Gefühl haben, dass ein zuvor eingestellter Wert nicht mehr angemessen ist, können Sie jederzeit einen erneuten Test durchführen und ihn entsprechend aktualisieren.

---

Standardmäßig geht ASF davon aus, dass `HoursUntilCardDrops` `3` ist, da der negative Effekt der Einstellung auf `3`, wenn er kleiner sein sollte, kleiner ist als umgekehrt. Dies liegt daran, dass wir im schlimmsten Fall `3` Stunden Sammeln pro `32` Spiele verschwenden, verglichen mit dem Verschwenden von `3` Stunden Sammeln pro Spiel, wenn `HoursUntilCardDrops` standardmäßig auf `0` eingestellt wurde. Sie sollten diese Variable jedoch immer noch so einstellen, dass sie mit Ihrem Konto übereinstimmt, um maximale Effizienz zu erreichen, da dies nur eine blinde Vermutung ist, die auf potenziellen Nachteilen und der Mehrheit der Benutzer basiert (daher versuchen wir, standardmäßig das „kleineres Übel“ zu wählen).

Im Moment reichen zwei der oben genannten Algorithmen für alle derzeit möglichen Kontoszenarien aus, um so effektiv wie möglich zu wirtschaften, daher ist es nicht geplant, weitere hinzuzufügen.

Es ist gut zu wissen, dass ASF ebenfalls einen manuellen Sammelmodus besitzt, welcher mittels des Befehls `play` aktiviert wird. Sie können mehr darüber lesen in **[Befehle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)**.

---

## Steam Pannen

Der Algorithmus zum Sammeln von Karten funktioniert nicht immer so, wie er sollte, und es ist möglich, dass verschiedene Steam-Pannen auftreten, z. B. Karten, die auf eingeschränkten Konten gesammelt werden; Karten, die beim Schließen/Wechseln des Spiels gesammelt werden; Karten, die beim Spielen überhaupt nicht gesammelt werden, und ähnliche Szenarien.

Dieser Abschnitt ist hauptsächlich für Leute, die sich fragen, warum ASF **X** nicht macht, wie die Schnellumschaltung von Spielen, um Karten schneller zu sammeln.

Was ist eine **Steam-Panne** – eine spezifische Aktion, die **undefiniertes** Verhalten auslösen, das **nicht beabsichtigt, undokumentiert und als logischer Fehler betrachtet wird**. Es ist **unzuverlässig per Definition**, was bedeutet, dass es nicht zuverlässig mit einer sauberen Testumgebung reproduziert werden kann, und daher ohne den Einsatz von Hacks programmiert ist, die erraten sollen, wann eine Störung auftritt und wie man damit kämpfen / sie missbrauchen kann. Normalerweise ist es temporär, bis die Entwickler den Logikfehler beheben, obwohl einige Fehlfunktionen für einen sehr langen Zeitraum unbemerkt bleiben können.

Ein anschauliches Beispiel für das, was als **Steam-Panne** gilt, ist nicht die ungewöhnliche Situation, eine Karte zu sammeln, wenn das Spiel geschlossen wird, was bis zu einem gewissen Grad mit der Spiel-Sprung-Funktion des *Idle Master* missbraucht werden kann.

- **Undefiniertes Verhalten** – Sie können nicht sagen, ob 0 oder 1 Karten gesammelt werden, wenn Sie die Panne auslösen.
- **Nicht beabsichtigt** – basierend auf früheren Erfahrungen und Verhaltensweisen des Steam-Netzwerkes, die nicht zu demselben Verhalten beim Senden einer einzelnen Anfrage führen.
- **Undokumentiert** – es ist klar dokumentiert auf der Steam-Website, wie Karten erhalten werden, und **an jedem einzelnen Ort** es wird klar gesagt, dass es durch **Spielen** erhalten wird, NICHT durch das Schließen von Spielen, das Erhalten von Erfolgen, das Wechseln oder das gleichzeitige Starten von 32 Spielen.
- **Betrachtet als logischer Fehler** – das Schließen von Spielen oder das Vertauschen von Spielen sollte kein Ergebnis haben, wenn Karten fallen gelassen werden, die eindeutig als durch **gewonnene Spielzeit** erhalten werden.
- **Unzuverlässig per Definition, kann nicht zuverlässig reproduziert werden** – es funktioniert nicht für alle, und selbst wenn es einmal für Sie funktioniert hat, funktioniert es vielleicht nicht mehr zum zweiten Mal.

Jetzt, nachdem wir erkannt haben, was eine Steam-Panne ist, und die Tatsache, dass Karten, die fallen gelassen werden, wenn das Spiel geschlossen wird eine **ist**, können wir zum zweiten Punkt übergehen – **ASF missbraucht das Steam-Netzwerk in keiner Weise per Definition, und es tut sein Bestes, um Steam-AGB, seine Protokolle und das, was allgemein akzeptiert wird** zu erfüllen. Das Spamming des Steam-Netzwerkes mit ständigen Anfragen zum Öffnen/Schließen des Spiels kann als **[DoS-Angriff](https://de.wikipedia.org/wiki/Denial-of-service_attack)** betrachtet werden und **verletzt direkt [Steam-Online-Verhalten](https://store.steampowered.com/online_conduct/?l=english)**.

> Als Steam-Abonnent verpflichten Sie sich, die folgenden Verhaltensrichtlinien zu befolgen.
> 
> […] Sie folgende Handlungen [nicht] durchführen […]:
> 
> Störung, Schädigung oder Manipulation von Steam

Es spielt keine Rolle, ob Sie in der Lage sind, Steam-Pannen mit anderen Programmen (wie IM) auszulösen, und es spielt auch keine Rolle, ob Sie mit uns einverstanden sind und ein solches Verhalten als DoS-Angriff betrachten oder nicht – es liegt an Valve, dies zu beurteilen, aber wenn wir es als Ausnutzen/Missbrauch nicht beabsichtigten Verhaltens durch übermäßige Steam-Netzwerkanfragen betrachten, dann können Sie ziemlich sicher sein, dass Valve eine ähnliche Ansicht dazu hat.

ASF wird **nie** Steam-Exploits, Missbräuche, Hacks oder jede mögliche andere Aktivität ausnutzen, die wir als **illegal oder unerwünscht** (entsprechend Steam-AGB, Steam-Online-Verhalten oder einer anderen vertrauenswürdigen Quelle) ansehen, die anzeigen könnte, dass ASF-Aktivität durch Steam-Netzwerk unerwünscht ist, wie im Abschnitt **[Beitragen](https://github.com/JustArchiNET/ArchiSteamFarm/blob/main/.github/CONTRIBUTING.md)** angegeben.

Wenn Sie um jeden Preis Ihr Steam-Konto riskieren wollen, um ein paar Cent-Karten schneller als sonst zu sammeln, dann wird ASF leider nie so etwas im Automatikmodus anbieten; obwohl Sie immer noch den `play` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** haben, das als Werkzeug benutzt werden kann, um in Bezug zur Steam-Netzwerkinteraktion zu tun, was immer Sie wollen. Wir empfehlen nicht, die Vorteile von Steam-Pannen für den eigenen Gewinn auszunutzen – nicht nur mit ASF, sondern generell auch mit jedem anderen Programm. Am Ende ist es jedoch Ihr Konto und Ihre Wahl, was Sie damit anstellen wollen – denken Sie einfach daran, dass wir Sie gewarnt haben.