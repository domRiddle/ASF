# Übersetzung

ASF wird von einem Service namens Crowdin unterstützt, was es jedem ermöglicht ASF in jegliche Sprachen zu übersetzen. Für eine detailliertere Erklärung wie Crowdin funktioniert lies dir bitte die **[Einführung in Crowdin](https://support.crowdin.com/crowdin-intro/)** durch.

Wenn du daran interessiert bist, was zur Zeit vor sich geht, kannst du die **[aktuellen ASF Aktivitäten](https://crowdin.com/project/archisteamfarm/activity_stream)** ansehen.

* * *

## Umfang

Unsere Plattform unterstützt zusätzlich zur Übersetzung des Hauptprogramms ASF auch die Übersetzung des gesamten Inhalts, den wir zusammen mit ASF anbieten. Dies beinhaltet insbesondere unseren Generator für Konfigurationsdateien, das graphische IPC User-Interface und auch das Wiki selbst. All das kann man bequem durch das Crowdin-Interface übersetzen.

* * *

## Anmeldung

Wenn du bei ASF helfen willst, entweder durch Übersetzen selbst, oder das Rezensieren oder Genehmigen von Übersetzungen, bitte melde dich auf der **[Crowdin Projektseite](https://crowdin.com/project/archisteamfarm)** an. Die Registrierung ist einfach und absolut gratis! Nach dem Einloggen kannst du Sprachen auswählen, denen du gern zugewiesen werden möchtest und anschließend zu den ASF strings gehen und dem Rest der Community beim Übersetzen von ASF in alle Sprachen der Welt helfen!

* * *

### Übersetzung

Wenn bei der Sprache deiner Wahl noch Strings fehlen, kannst du sie dir direkt nehmen und an der Übersetzung arbeiten. Wir versuchten unser Bestes im Bezug auf Flexibilität der Übersetzungen, weshalb viele Strings extra Variablen beinhalten, die ASF während der Laufzeit zur Verfügung stellen wird. Diese sind in Form von Nummern in geschwungenen Klammern eingeschlossen wie zum Beispiel `{0}`. Das erlaubt dir das Standard-ASF-Format aufzubrechen und den String an eine Position zu verschieben, der deiner Sprache und Übersetzung entspricht, anstatt an strengen Kontext und Format gebunden zu sein. Das ist besonders wichtig in Sprachen wie Hebräisch, die von rechts nach links gelesen werden.

Zum Beispiel könntest du folgenden String haben:

> Es verbleiben {0} Spiele zum Sammeln.

Aber basierend auf deiner Sprache könnte folgender Satz mehr Sinn machen:

> Die Zahl der verbleibenden Spiele zum Sammeln ist {0}.

Diese Flexibilität biten wir besonders für dich, damit du den ASF-Satz leicht umschreiben kannst um besser zu deiner Sprache zu passen und die Anzahl der von ASF zur Verfügung gestellten Informationen in eine Position zu rücken, die deiner Übersetzung entspricht (anstatt jeden Teil einzeln zu übersetzen). Das Verbessert die allgemeine Übersetzungsqualität.

* * *

### Rezensierung

Wenn dein String bereits von jemand anderem übersetzt wurde, kannst du dafür stimmen. Stimmen macht es möglich die beste Variante einer Übersetzung auszusuchen, anstatt an einem ursprünglichen Vorschlag festzuhalten - Das Verbessert die Qualität der Übersetzungen weiter. Du kannst für bereits verfügbare Vorschläge stimmen oder deine eigenen Vorschlag der Übersetzung einbringen, der durch den selben Prozess gehen wird. Schlussendlich wird ein endgültiger Wert gewählt, basierend auf dem Vorschlag mit den meisten Stimmen oder als Wahl eines Korrekturlesers, der für diese Sprache ausgewählt wurde, der die gegebene Übersetzung persönlich genehmigt (unter Anderem basierend auf deinen Stimmen).

**Du brauchst keine Genehmigung um deine Übersetzungen in ASF zu sehen**. Genehmigung heißt einfach, dass jemand, dem wir vertrauen, deinen Beitrag angesehen und für gut befunden hat. Es ist in Ordnung keine genehmigten Übersetzungen, die von der Community gemacht wurden zu haben, wo du für die Beste Art stimmst. So lange es übersetzt ist, ist es in Ordnung! Wenn du denkst, dass die aktuelle Übersetzung schlecht ist, kannst du gerne jederzeit für eine bessere stimmen oder selbst eine vorschlagen! 

* * *

### Korrekturlesen

Es ist eine gute Idee eine konsistente Übersetzung zu haben, selbst, wenn es möglicherweise die Freiheiten der oben erklärten Community-Rezensierung oder des Stimmprozesses einschränken könnte. Das ist hauptsächlich weil falsche Übersetzungen, die nicht notwendigerweise schlecht sind, so viele Stimmen bekommen könnten, dass es nicht möglich ist eine bessere Übersetzung vorzuschlagen, selbst, wenn diese jemand hat.

Wenn du eine Vergangenheit von Beiträgen auf Crowdin oder jeglicher anderen Übersetzungsplattform hast, die wir verifizieren und vertrauensvoll nennen können, würden wir uns freuen, dir Korrekturleser-Zugriff zu der Sprache zu geben, zu der du beiträgst, damit es dir möglich ist gegebene Übersetzungen zu genehmigen oder konsistenter zu gestalten. Korrekturlesen ist keine einfache Aufgabe, besonders, weil ASF von Zeit zu Zeit sehr "technisch" und schwer zu übersetzen sein kann, aber wir verstehen, dass es für eine perfekte Übersetzung oft nötig ist. Wenn du uns helfen kannst eine Sprache Korrektur zu lesen, bitten wir dich: **[Gib uns Bescheid](https://crowdin.com/messages/create/13177432)**, bedenke aber, dass du deine Anfrage mit vergangenen Beiträgen, die wir verifizieren können (etwa bei ASF oder anderen Projekten auf Crowdin zu helfen), stützen musst. Wir können auch fortgeschrittenen Benutzern das initiale Korrekturlesen erlauben, wenn wir sie persönlich kennen und sie mit dem Rest der Community kooperieren können um ASF in der gegebenen Sprache bestmöglich zu übersetzen.

Die generellen Regeln gelten auch für das Korrekturlesen - nimm dir Zeit, hör auf die Benutzer, arbeite als Projektmanager, löse Probleme und stell sicher, dass du die Dinge besser machst und nicht schlimmer.

* * *

### Issues

Wenn du ein Problem mit einer bestimmten Übersetzung hast, weil du zum Beispiel nicht weißt, wie sie zu übersetzen ist, eine genehmigte Übersetzung falsch ist, du mehr Kontext benötigst oder Ähnliches, dann Poste bitte einen Kommentar unter dem betroffenen String und markiere ihn mit [X] als ein Problem.

**Bitte vermeide das [x] wenn du keine technische Erklärung oder Aktion eines Admins benötigst**. Es steht dir frei Kommentare für Diskussionen zur Übersetzung eines Strings zu verwenden, aber der "Issue-Tag" sollte nur verwendet werden, wenn eine weitergehende technische Erklärung oder Lösung durch einen Admin benötigt wird und wird typischerweise jemanden betreffen, der deine Sprache nicht spricht, weshalb du Problem-Kommentare möglichst auf Englisch schreiben solltest (damit wir verstehen, was das Problem ist).

Es gibt 4 Typen von Problemen, die wir befürworten:

- generelle Frage - Für alles, was keinen der anderen Typen betrifft. Dieser Typ **sollte vermieden** werden, weil es dann höchstwahrscheinlich **kein** Übersetzungsproblem ist. Trotzdem ist diese Möglichkeit für alle anderen Fälle offen.
- Die aktuelle Übersetzung ist falsch - Dies sollte **nur** verwendet werden, wenn die Übersetzung von einem Korrekturleser genehmigt wurde und du glaubst, dass sie falsch ist, weil sie etwa einen Tippfehler beinhaltet oder du einen guten Vorschlag hast sie zu verbessern. Dieser Typ sollte nie bei Übersetzungen verwendet werden, die durch Community-Stimmen gewählt wurden, in welchem Fall du den Benutzer der die Übersetzung geschrieben hat kontaktieren und ihn um eine Korrektur bitten oder einfach für eine bessere Übersetzung stimmen solltest.
- Fehlender Kontext - Dies ist, was du verwenden solltest, wenn du nicht sicher bist, welchen Teil von ASF du übersetzt und was der Kontext des gegebenen Strings oder sein Sinn ist. Dieser Typ sollte nur für die Entwicklung von ASF verwendet werden, da er bedeutet, dass du technische Hilfe benötigst, weil du nicht sicher bist, wie der String zu übersetzen ist.
- Fehler im Quell-String - Dies sollte nur verwendet werden, wenn du denkst, dass das Original (Englisch) falsch ist. Sehr selten, aber meine eigene Muttersprache ist auch nicht Englisch und deshalb solltest du dich frei fühlen diesen zu verwenden, wenn du eine generelle Idee hast, wie man es verbessern könnte.

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

## Wiki

Our crowdin platform also allows you to localize even the wiki itself. This is a very powerful tool, since you can also adapt the original text in order to make it point to new (localized) locations.

As you should know already, everybody can change wiki language by adding `-locale` string to any visited page. For example, instead of visiting **<FAQ>**, you can visit **<FAQ-ru-RU>** that contains FAQ translated into Russian (if available).

Now, ASF includes links on almost every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit those too, "fixing" links to point to proper localized pages for your language. It requires a bit of hacking around, but it's possible.

For example, ASF **[home page](https://github.com/JustArchi/ArchiSteamFarm/wiki)** includes a text such as:

> If you're a new user, we recommend starting with **[setting up](Setting-up)** guide.

Which is originally written as:

```markdown
If you're a new user, we recommend starting with **[setting up](Setting-up)** guide.
```

On the crowdin you'll see instead:

> If you're a new user, we recommend starting with <0>setting up</0> guide.

Firstly, you translate it as usual, leaving everything in-tact. This would be example for Polish language:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z <0>przewodnika po konfiguracji</0>.

Now, if the link is a generic link that points outside of the wiki (e.g. to latest ASF release), you can leave it as it is since you don't want to edit it. You can save it and move forward.

However, if the link **does** point further inside the wiki, like the one above, you can actually **remove** `<0>` tags and replace them with pure HTML appropriate for new location.

We have a `Setting-up` link, so our Polish version is `Setting-up-pl-PL`. We can now hover over `<0>` tag to see how it's defined, usually it'll be either `<strong><a></a></strong>` or `<a></a>` alone. Then, with knowledge of what `<0>` is in fact replaced with, we can rewrite it to point into new location:

```html
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z <strong><a href="Setting-up-pl-PL">przewodnika po konfiguracji</a></strong>.
```

This will properly transform HTML back to markdown:

```markdown
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](Setting-up-pl-PL)**.
```

And finally into wiki text:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](Setting-up-pl-PL)**.

In similar way you can translate (and point to new locations) our sidebar! 

* * *

Thank you for helping us to translate ASF!