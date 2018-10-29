# Lokalisierung

ASF wird von dem Dienst Crowdin unterstützt, wodurch es jedem ermöglicht wird ASF in alle weltweit gesprochenen Sprachen zu übersetzen. Für eine detailliertere Erklärung wie Crowdin funktioniert lies dir bitte die **[Einführung in Crowdin](https://support.crowdin.com/crowdin-intro)** durch.

Wenn du daran interessiert bist, was zur Zeit vor sich geht, kannst du die **[aktuellen ASF Crowdin Aktivitäten](https://crowdin.com/project/archisteamfarm/activity_stream)** ansehen.

* * *

## Umfang

Unsere Plattform unterstützt zusätzlich zur Übersetzung des Hauptprogramms ASF auch die Übersetzung des gesamten Inhalts, den wir zusammen mit ASF anbieten. Dies beinhaltet insbesondere unseren ASF-WebConfigGenerator, ASF-ui sowie auch das Wiki selbst. All das kann man bequem durch das Crowdin-Interface übersetzen.

* * *

## Anmeldung

Wenn du bei ASF helfen willst, entweder durch Übersetzen selbst, oder das Rezensieren oder Genehmigen von Übersetzungen, bitte melde dich auf der **[Crowdin Projektseite](https://crowdin.com/project/archisteamfarm)** an. Die Registrierung ist einfach und absolut gratis! Nach dem Einloggen kannst du Sprachen auswählen, denen du gern zugewiesen werden möchtest und anschließend zu den ASF strings gehen und dem Rest der Community beim Übersetzen von ASF in alle Sprachen der Welt helfen!

* * *

### Übersetzen

Wenn bei der Sprache deiner Wahl noch Strings fehlen, kannst du sie dir direkt nehmen und an der Übersetzung arbeiten. Wir versuchten unser Bestes im Bezug auf Flexibilität der Übersetzungen, weshalb viele Strings extra Variablen beinhalten, die ASF während der Laufzeit zur Verfügung stellen wird. Diese sind in Form von Nummern in geschwungenen Klammern eingeschlossen wie zum Beispiel `{0}`. Das erlaubt dir das Standard-ASF-Format aufzubrechen und den String an eine Position zu verschieben, der deiner Sprache und Übersetzung entspricht, anstatt an strengen Kontext und Format gebunden zu sein. Das ist besonders wichtig in Sprachen wie Hebräisch, die von rechts nach links gelesen werden.

Zum Beispiel könntest du folgenden String haben:

> Es verbleiben {0} Spiele zum Sammeln.

Aber basierend auf deiner Sprache könnte folgender Satz mehr Sinn machen:

> Die Zahl der verbleibenden Spiele zum Sammeln ist {0}.

Oder:

> {0} ist die Zahl der verbleibenden Spiele zum Sammeln.

Diese Flexibilität biten wir besonders für dich, damit du den ASF-Satz leicht umschreiben kannst um besser zu deiner Sprache zu passen und die Anzahl der von ASF zur Verfügung gestellten Informationen in eine Position zu rücken, die deiner Übersetzung entspricht (anstatt jeden Teil einzeln zu übersetzen). Das Verbessert die allgemeine Übersetzungsqualität.

* * *

### Rezensionen

Wenn dein String bereits von jemand anderem übersetzt wurde, kannst du dafür stimmen. Stimmen macht es möglich die beste Variante einer Übersetzung auszusuchen, anstatt an einem ursprünglichen Vorschlag festzuhalten - Das Verbessert die Qualität der Übersetzungen weiter. Du kannst für bereits verfügbare Vorschläge stimmen oder deine eigenen Vorschlag der Übersetzung einbringen, der durch den selben Prozess gehen wird. Schlussendlich wird ein endgültiger Wert gewählt, basierend auf dem Vorschlag mit den meisten Stimmen oder als Wahl eines Korrekturlesers, der für diese Sprache ausgewählt wurde, der die gegebene Übersetzung persönlich genehmigt (unter Anderem basierend auf deinen Stimmen).

**Du brauchst keine Genehmigung um deine Übersetzungen in ASF zu sehen**. Genehmigung heißt einfach, dass jemand, dem wir vertrauen, deinen Beitrag angesehen und für gut befunden hat. Es ist in Ordnung keine genehmigten Übersetzungen, die von der Community gemacht wurden zu haben, wo du für die Beste Art stimmst. So lange es übersetzt ist, ist es in Ordnung! Wenn du denkst, dass die aktuelle Übersetzung schlecht ist, kannst du gerne jederzeit für eine bessere stimmen oder selbst eine vorschlagen!

* * *

### Korrekturlesen

Es ist eine gute Idee eine konsistente Übersetzung zu haben, selbst, wenn es möglicherweise die Freiheiten der oben erklärten Community-Rezensierung oder des Stimmprozesses einschränken könnte. Das ist hauptsächlich weil falsche Übersetzungen, die nicht notwendigerweise schlecht sind, so viele Stimmen bekommen könnten, dass es nicht möglich ist eine bessere Übersetzung vorzuschlagen, selbst, wenn diese jemand hat.

Wenn du eine Vergangenheit von Beiträgen auf Crowdin oder jeglicher anderen Übersetzungsplattform hast, die wir verifizieren und vertrauensvoll nennen können, würden wir uns freuen, dir Korrekturleser-Zugriff zu der Sprache zu geben, zu der du beiträgst, damit es dir möglich ist gegebene Übersetzungen zu genehmigen oder konsistenter zu gestalten. Korrekturlesen ist keine einfache Aufgabe, besonders, weil ASF von Zeit zu Zeit sehr "technisch" und schwer zu übersetzen sein kann, aber wir verstehen, dass es für eine perfekte Übersetzung oft nötig ist. Wenn du uns helfen kannst eine Sprache Korrektur zu lesen, bitten wir dich: **[Gib uns Bescheid](https://crowdin.com/messages/create/13177432)**, bedenke aber, dass du deine Anfrage mit vergangenen Beiträgen, die wir verifizieren können (etwa bei ASF oder anderen Projekten auf Crowdin zu helfen), stützen musst. Wir können auch fortgeschrittenen Benutzern das initiale Korrekturlesen erlauben, wenn wir sie persönlich kennen und sie mit dem Rest der Community kooperieren können um ASF in der gegebenen Sprache bestmöglich zu übersetzen.

Die generellen Regeln gelten auch für das Korrekturlesen - nimm dir Zeit, hör auf die Benutzer, arbeite als Projektmanager, löse Probleme und stell sicher, dass du die Dinge besser machst und nicht schlimmer.

* * *

### Probleme

Wenn du ein Problem mit einer bestimmten Übersetzung hast, weil du zum Beispiel nicht weißt, wie sie zu übersetzen ist, eine genehmigte Übersetzung falsch ist, du mehr Kontext benötigst oder Ähnliches, dann Poste bitte einen Kommentar unter dem betroffenen String und markiere ihn mit [X] als ein Problem.

**Bitte vermeide das [x] wenn du keine technische Erklärung oder Aktion eines Admins benötigst**. Es steht dir frei Kommentare für Diskussionen zur Übersetzung eines Strings zu verwenden, aber der "Issue-Tag" sollte nur verwendet werden, wenn eine weitergehende technische Erklärung oder Lösung durch einen Admin benötigt wird und wird typischerweise jemanden betreffen, der deine Sprache nicht spricht, weshalb du Problem-Kommentare möglichst auf Englisch schreiben solltest (damit wir verstehen, was das Problem ist).

Es gibt 4 Typen von Problemen, die wir unterstützen:

* generelle Frage - Für alles, was keinen der anderen Typen betrifft. Dieser Typ **sollte vermieden** werden, weil es dann höchstwahrscheinlich **kein** Übersetzungsproblem ist. Trotzdem ist diese Möglichkeit für alle anderen Fälle offen.
* Die aktuelle Übersetzung ist falsch - Dies sollte **nur** verwendet werden, wenn die Übersetzung von einem Korrekturleser genehmigt wurde und du glaubst, dass sie falsch ist, weil sie etwa einen Tippfehler beinhaltet oder du einen guten Vorschlag hast sie zu verbessern. Dieser Typ sollte nie bei Übersetzungen verwendet werden, die durch Community-Stimmen gewählt wurden, in welchem Fall du den Benutzer der die Übersetzung geschrieben hat kontaktieren und ihn um eine Korrektur bitten oder einfach für eine bessere Übersetzung stimmen solltest.
* Fehlender Kontext - Dies ist, was du verwenden solltest, wenn du nicht sicher bist, welchen Teil von ASF du übersetzt und was der Kontext des gegebenen Strings oder sein Sinn ist. Dieser Typ sollte nur für die Entwicklung von ASF verwendet werden, da er bedeutet, dass du technische Hilfe benötigst, weil du nicht sicher bist, wie der String zu übersetzen ist.
* Fehler im Quell-String - Dies sollte nur verwendet werden, wenn du denkst, dass das Original (Englisch) falsch ist. Sehr selten, aber meine eigene Muttersprache ist auch nicht Englisch und deshalb solltest du dich frei fühlen diesen zu verwenden, wenn du eine generelle Idee hast, wie man es verbessern könnte.

* * *

### Übersetzungsfortschritt

Jede Sprache hat zwei Zustände des Fortschritts - Übersetzung und Korrekturlesen.

Eine Sprache gilt als **übersetzt** wenn ihr Übersetzungsfortschritt 100% erreicht. Zu diesem Zeitpunkt hat jeder übersetzbare String der von ASF verwendet wird eine entsprechende Bedeutung. Allerdings heißt das nicht, dass der kein Raum für Verbesserungen ist - das weitere Stimmen ist durchgehend aktiviert und du kannst immer noch Vorschläge für bessere Übersetzungen einbringen und für existierende Stimmen. Bitte merke, dass bereits fertig übersetzte Sprachen immer noch unter 100% fallen können, wenn wir während der Entwicklung existierende Strings ändern oder neue hinzufügen. Du kannst entsprechende Crowdin-Benachrichtigungen aktivieren, wenn du in einem solchen Fall eine E-Mail erhalten willst.

Gewählte Sprachen können entsprechende Korrekturleser haben, die Übersetzungen validieren und endgültige Versionen auswählen. Dies ist der letzte Schritt nach der Übersetzung und erlaubt weitere Verbesserungen.

ASF wird die Sprache **so bald wie möglich** beinhalten, was bedeutet, dass sie nicht genehmigt oder zu 100% übersetzt werden muss. Die tatsächlich verwendeten Strings sind immer die am beliebtesten im Bezug auf Stimmen, außer der gewählte Korrekturleser hat anders entschieden (sehr selten). Deshalb kannst du deine Bemühungen direkt in der nächsten Version von ASF sehen, sobald die Übersetzung auf Crowdin ist. Typischerweise fügen wir Übersetzungsupdates in dem Moment hinzu, in dem wir die neue ASF-Version herausgeben werden.

* * *

## Fehlende Sprachen

Standardmäßig hat das ASF-Projekt nur Übersetzungen für die 30 meistgesprochenen Sprachen weltweit zur Verfügung. Wenn du eine weitere (oder einen lokalen Dialekt zu einer existierenden) hinzufügen willst, **[lass es uns bitte wissen](https://crowdin.com/messages/create/13177432)** und wir werden sie möglichst schnell hinzufügen. Wir wollen nicht mehrere hundert verschiedene Sprachen, wenn sie keiner übersetzt. Deshalb haben wir sie auf diese Anzahl reduziert. Bitte zögere nicht uns zu kontaktieren, wenn du eine nicht gelistete Sprache übersetzen willst. Für uns ist es sehr einfach eine weitere hinzuzufügen.

Für eine komplette Liste der verfügbaren Sprachen, in die ASF übersetzt werden kann, **[klicke hier](https://support.crowdin.com/api/language-codes)**.

* * *

## Pluralisierung

Jede Sprache hat ihre eigenen Regeln in Bezug auf die Pluralisierung. Diese Regeln findest du unter **[CLDR](https://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html)**, welche ihre Anzahl und genauen Sprachbedingungen angibt.

Wir tun unser Bestes, um dir eine flexible Lokalisierung anzubieten, und so lange wie möglich, wird dies auch Regeln für Pluralisierung beinhalten. Als Beispiel werden wir heute folgende Zeichenkette ins Polnische übersetzen:

> Released {PLURAL:n|{n} month|{n} months} ago

`PLURAL` Schlüsselwort wird hier besonders behandelt, da es dir erlaubt, alle Pluralformen einzubeziehen, die deine Sprache unterstützt. Wenn du einen Blick auf CLDR wirfst, wirst du sehen, dass es im Englischen nur 2 kardinale Formen gibt - "eine" und "andere". Und wie du oben sehen kannst, haben wir beide definiert - `{n} month` und `{n} months`.

Unsere polnische Sprache umfasst jedoch 4 von ihnen - "eine", "wenige", "viele" und "andere". Das bedeutet, dass wir alle für die vollständige Umsetzung definieren sollten. Unsere Übersetzungsprogramme sind bereits intelligent genug, um eine geeignete Pluralform basierend auf Sprachregeln auszuwählen, so dass du nur alle diese in der Übersetzung definieren musst:

> Wydany {PLURAL:n|{n} miesiąc|{n} miesiące|{n} miesięcy|{n} miesiąca} temu

Auf diese Weise haben wir alle 4 Pluralformen für unsere polnische Sprache definiert, und da unsere Lokalisierungsbibliothek bereits die genauen Regeln kennt, wird sie das richtige Formular für die angegebene `{n}` Nummer korrekt verwenden.

Es ist nicht zwingend erforderlich, alle von deiner Sprache verwendeten Pluralformen zu definieren. Wenn sie fehlt, verwendet unsere Lokalisierungsbibliothek die zuletzt definierte Form an ihrer Stelle. Es ist eine gute Idee, alle von deiner Sprache verwendeten Pluralformen zu definieren, aber in einigen Fällen können die verbleibenden Pluralformen die gleichen sein wie die letzten, in diesem Fall ist es nicht notwendig, sie zu wiederholen. In unserem obigen Beispiel war es zwingend erforderlich, da die "andere" Form im Polnischen für Monate "miesiąca" ist und nicht "miesięcy" wie in "vielen".

* * *

## Wiki

Unsere Crowdin-Plattform ermöglicht es dir sogar selbst das Wiki zu lokalisieren. Dies ist ein sehr mächtiges Programm, da es dir ermöglicht, eine komplette ASF-Dokumentation in deiner Muttersprache zu erstellen und so das allerletzte Problem bei der ASF-Lokalisierung effektiv zu lösen. Zusammen mit der Übersetzung des Programms und alle seiner Teile macht dies die Lokalisierung komplett.

Wiki ist in dieser Hinsicht etwas Besonderes, da es eine Online-Hilfe ist, bei der man sich nicht zu sehr an den ursprünglichen Satz halten muss. Das bedeutet, dass du mit deiner Sprache so natürlich wie möglich umgehen und eine originelle Bedeutung und Hilfe liefern solltest - nicht unbedingt mit der ursprünglichen Zeichenkette, den verwendeten Wörtern und der tatsächlichen Interpunktion. Scheu dich nicht die Zeichenkette in etwas viel natürlicheres für deine Sprache umzuschreiben, solange du die allgemeine Richtung und die im Satz enthaltene Hilfe einhältst.

* * *

### Globale Links

Unsere Crowdin-Plattform ermöglicht es dir auch, den Originaltext so anzupassen, dass er auf neue (lokalisierte) Standorte verweist.

ASF enthält Links auf fast jeder Seite zur leichteren Navigation sowie eine Seitenleiste auf der rechten Seite. Die fantastische Tatsache ist, dass du all das bearbeiten kannst, indem du Links "fixierst", um auf richtige lokalisierte Seiten für deine Sprache zu verweisen. Es erfordert ein wenig Vorsicht, wenn du das tust, aber es ist möglich.

Zum Beispiel enthält die **[Startseite](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home-de-DE)** von ASF folgenden Text:

> Wenn du ein neuer Benutzer bist, empfehlen wir dir mit dem Leitfaden zur **[Installation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-de-DE)** anzufangen.

Der eigentlich so geschrieben wird:

```markdown
Wenn du ein neuer Benutzer bist, empfehlen wir dir mit dem Leitfaden zur **[Installation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-de-DE)** anzufangen.
```

Auf Crowdin solltest du zuerst zu deinen Editor-Einstellungen gehen und sicherstellen, dass HTML-Tags für dich auf "Show" gesetzt sind. Dies ist sehr wichtig, wenn du dich dazu entscheidest das Wiki zu übersetzen.

* * *

![Crowdin](https://i.imgur.com/YqAxiZ4.png)

* * *

Nun, während der Übersetzung auf Crowdin, je nach Formatierung, siehst du ASF-Links im Text entweder als:

* Zu übersetzende Zeichenkette zusammen mit HTML-Tags (Mehrheit der Zeichenketten, wobei nur ein Teil des Satzes ein Link ist)
* Einzelne zu übersetzende Zeichenkette, mit Link in `Hidden texts` -> `Link addresses` (selten, wo die gesamte Zeichenkette ein Link ist, am häufigsten in der Seitenleiste)

In unserem obigen Beispiel ist dies der erste Fall (da nur "Installation" ein Link ist), so dass wir es in Crowdin folgendermaßen sehen werden:

* * *

![Crowdin 2](https://i.imgur.com/Li5RzS3.png)

* * *

Regardless of case, firstly you should copy the source string and translate it as usual, leaving entire HTML (if present) in-tact. Dies wäre ein Beispiel für eine Übersetzung in die polnische Sprache:

* * *

![Crowdin 3](https://i.imgur.com/NpKwfka.png)

* * *

Now, if the link is a generic link that points outside of the wiki (e.g. to latest ASF release), you can leave it as it is since you don't want to edit it. You can save it and move forward.

However, if the link **does** point further inside the wiki, like the one above, you can actually correct it to point to new (localized) location. You do this by carefully appending `-locale` to target URL in `<a>` tag, like below:

* * *

![Crowdin 4](https://i.imgur.com/TL8uwmb.png)

* * *

Be extremely careful about this, and ensure that your URL indeed exists, since if you make a mistake, that link will stop functioning. If you succeeded, you now have a fully functional translation with link pointing to translated (in our case `Setting-up-pl-PL`) page.

Doing the steps above will properly translate our HTML back to markdown:

```markdown
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.
```

And finally into wiki text:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.

When no HTML is present (second case), this is even easier since you can just go to `Hidden texts` -> `Link addresses`.

* * *

![Crowdin 5](https://i.imgur.com/ZmgavCM.png)

* * *

From there you can easily correct the link to point to new location, without even bothering with HTML at all:

* * *

![Crowdin 6](https://i.imgur.com/maG7kSm.png)

* * *

### Lokale Links

Im gesamten Wiki findest du auch lokale Links, die auf einen bestimmten Abschnitt des Dokuments verweisen. Diese Links beginnen mit dem `#` Zeichen.

Dies sind nun Sonderfälle, da diese Links auf den Namen der Abschnitte des aktuellen Dokuments basieren. While for URLs we have general convention of adding `-locale` to the URL, and it works everywhere, section names will be translated by you and other people, so you need to ensure that they point to proper location.

For example you can find `#introduction` link in our **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#introduction)** section:

* * *

![Crowdin 7](https://i.imgur.com/EEHSPtK.png)

* * *

Da wir das Wort "Introduction" in "Wprowadzenie" für unsere polnische Sprache übersetzen werden, müssen wir diesen Link korrigieren, da er sonst nicht mehr funktioniert.

* * *

![Crowdin 8](https://i.imgur.com/JMJegO7.png)

* * *

Auf diese Weise funktioniert unser lokaler Link weiterhin, da er nun auf den Namen des Bereichs zeigt, den wir verwenden. Du kannst Links innerhalb von HTML-Tags auf die gleiche Weise korrigieren.

* * *

### Codeblöcke

Seie äußerst sorgfältig, wenn du Sätze mit `<code></code>` Blöcken übersetzt. Der Codeblock zeigt feste ASF-Codenamen oder Begriffe an die nicht übersetzt werden sollten. Zum Beispiel:

> This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit `RateLimited` status before you're done with your entire batch.

Wie du sehen kannst, befindet sich das Wort `RateLimited` hier in einem Codeblock und zeigt den internen ASF-Code-Status an, der nicht übersetzt werden sollte. Likewise, you shouldn't translate other code blocks, such as names of config properties (e.g. `TradingPreferences`), enum members (e.g. `Stable` and `Experimental` options of `UpdateChannel`) and likewise.

However, just because those words should not be translated, doesn't mean that you can't add appropriate translation next to them, for example in brackets.

> Ta funkcja jest wyjątkowo użyteczna w przypadku aktywacji dużej ilości kluczy i gwarancji napotkania statusu `RateLimited` (zbyt częstej aktywacji) przed ukończeniem całej partii.

As you can see above, we've added "zbyt częstej aktywacji", literally "too often activation" next to `RateLimited` in order to translate that status in a friendly way, while at the same time keeping original ASF meaning that the user might see during usage of the program. In the same way you can translate/explain other, similar cases of various words and sentences.

Wenn du glaubst, dass etwas Unangemessenes in einem Codeblock enthalten ist oder es einen Text gibt der sich nicht in einem Codeblock befindet, sich aber in einem Codeblock befinden sollte, kannst du gerne hier auf Crowdin fragen, indem du ein entsprechendes **[Problem](#Probleme)** erstellst.

* * *

Vielen Dank, dass du uns bei der Übersetzung von ASF in alle weltweit gesprochenen Sprachen unterstützt!