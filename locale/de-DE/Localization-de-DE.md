# Übersetzung

ASF wird von einem Service namens Crowdin unterstützt, was es jedem ermöglicht ASF in jegliche Sprachen zu übersetzen. Für eine detailliertere Erklärung wie Crowdin funktioniert lies dir bitte die **[Einführung in Crowdin](https://support.crowdin.com/crowdin-intro)** durch.

Wenn du daran interessiert bist, was zur Zeit vor sich geht, kannst du die **[aktuellen ASF Crowdin Aktivitäten](https://crowdin.com/project/archisteamfarm/activity_stream)** ansehen.

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

Oder:

> {0} ist die Zahl der verbleibenden Spiele zum Sammeln.

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

### Probleme

Wenn du ein Problem mit einer bestimmten Übersetzung hast, weil du zum Beispiel nicht weißt, wie sie zu übersetzen ist, eine genehmigte Übersetzung falsch ist, du mehr Kontext benötigst oder Ähnliches, dann Poste bitte einen Kommentar unter dem betroffenen String und markiere ihn mit [X] als ein Problem.

**Bitte vermeide das [x] wenn du keine technische Erklärung oder Aktion eines Admins benötigst**. You're free to use comments for discussion related to translation of given string, but issue should be used only when you need further technical explanation or admin correction, and it will typically involve somebody who do not even speak the language you're translating to, so please stick with English when writing issue comment (so we can understand what the issue is).

Es gibt 4 Typen von Problemen, die wir befürworten:

* generelle Frage - Für alles, was keinen der anderen Typen betrifft. Dieser Typ **sollte vermieden** werden, weil es dann höchstwahrscheinlich **kein** Übersetzungsproblem ist. Trotzdem ist diese Möglichkeit für alle anderen Fälle offen.
* Die aktuelle Übersetzung ist falsch - Dies sollte **nur** verwendet werden, wenn die Übersetzung von einem Korrekturleser genehmigt wurde und du glaubst, dass sie falsch ist, weil sie etwa einen Tippfehler beinhaltet oder du einen guten Vorschlag hast sie zu verbessern. Dieser Typ sollte nie bei Übersetzungen verwendet werden, die durch Community-Stimmen gewählt wurden, in welchem Fall du den Benutzer der die Übersetzung geschrieben hat kontaktieren und ihn um eine Korrektur bitten oder einfach für eine bessere Übersetzung stimmen solltest.
* Fehlender Kontext - Dies ist, was du verwenden solltest, wenn du nicht sicher bist, welchen Teil von ASF du übersetzt und was der Kontext des gegebenen Strings oder sein Sinn ist. Dieser Typ sollte nur für die Entwicklung von ASF verwendet werden, da er bedeutet, dass du technische Hilfe benötigst, weil du nicht sicher bist, wie der String zu übersetzen ist.
* Fehler im Quell-String - Dies sollte nur verwendet werden, wenn du denkst, dass das Original (Englisch) falsch ist. Sehr selten, aber meine eigene Muttersprache ist auch nicht Englisch und deshalb solltest du dich frei fühlen diesen zu verwenden, wenn du eine generelle Idee hast, wie man es verbessern könnte.

* * *

### Übersetzungsfortschritt

Jede Sprache hat zwei Zustände des Fortschritts - Übersetzung und Korrekturlesen.

Eine Sprache gilt als **übersetzt** wenn ihr Übersetzungsfortschritt 100% erreicht. Zu diesem Zeitpunkt hat jeder übersetzbare String der von ASF verwendet wird eine entsprechende Bedeutung. Allerdings heißt das nicht, dass der kein Raum für Verbesserungen ist - das weitere Stimmen ist durchgehend aktiviert und du kannst immer noch Vorschläge für bessere Übersetzungen einbringen und für existierende Stimmen. Bitte merke, dass bereits fertig übersetzte Sprachen immer noch unter 100% fallen können, wenn wir während der Entwicklung existierende Strings ändern oder neue hinzufügen. Du kannst entsprechende Crowdin-Benachrichtigungen aktivieren, wenn du in einem solchen Fall eine E-Mail erhalten willst.

Gewählte Sprachen können entsprechende Korrekturleser haben, die Übersetzungen validieren und endgültige Versionen auswählen. Dies ist der letzte Schritt nach der Übersetzung und erlaubt weitere Verbesserungen.

ASF wird die Sprache **so bald wie möglich** beinhalten, was bedeutet, dass sie nicht genehmigt oder zu 100% übersetzt werden muss. The actual strings that will be used are always the most popular ones in terms of the votes, unless chosen proofreader decided otherwise (rarely). Deshalb kannst du deine Bemühungen direkt in der nächsten Version von ASF sehen, sobald die Übersetzung auf Crowdin ist. Typischerweise fügen wir Übersetzungsupdates in dem Moment hinzu, in dem wir die neue ASF-Version herausgeben werden.

* * *

## Fehlende Sprachen

Standardmäßig hat das ASF-Projekt nur Übersetzungen für die 30 meistgesprochenen Sprachen weltweit zur Verfügung. Wenn du eine weitere (oder einen lokalen Dialekt zu einer existierenden) hinzufügen willst, **[lass es uns bitte wissen](https://crowdin.com/messages/create/13177432)** und wir werden sie möglichst schnell hinzufügen. Wir wollen nicht mehrere hundert verschiedene Sprachen, wenn sie keiner übersetzt. Deshalb haben wir sie auf diese Anzahl reduziert. Bitte zögere nicht uns zu kontaktieren, wenn du eine nicht gelistete Sprache übersetzen willst. Für uns ist es sehr einfach eine weitere hinzuzufügen.

Für eine komplette Liste der verfügbaren Sprachen, in die ASF übersetzt werden kann, **[klicke hier](https://support.crowdin.com/api/language-codes)**.

* * *

## Wiki

Our crowdin platform also allows you to localize even the wiki itself. This is a very powerful tool, since it allows you to create a whole ASF documentation in your native language, effectively solving the very last issue when it comes to ASF localization. Together with translation of the program and all its parts, this makes localization complete.

Wiki is a bit special in this regard, since it's online help where you don't need to stick with original sentence too much. This means that you want to be as natural with your language as possible, and deliver original meaning and help - not necessarily stick with original string, used words and actual punctuation. Don't be afraid of rewriting the string into something far more natural for your language, as long as you keep the general direction and help included in the sentence.

* * *

### Global links

Our crowdin platform also allows you to adapt the original text in order to make it point to new (localized) locations.

ASF includes links on almost every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit all of that, "fixing" links to point to proper localized pages for your language. It requires to be a bit careful doing that, but it's possible.

For example, ASF **[home page](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)** includes a text such as:

> Wenn du ein neuer Benutzer bist, empfehlen wir dir mit dem Leitfaden zur **[Installation](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)** anzufangen.

Which is originally written as:

```markdown
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up)** guide.
```

On the crowdin, first thing you should do is going to your editor settings and ensuring that HTML tags are set to "Show" for you. This is very important if you decide to localize the wiki.

* * *

![Crowdin](https://i.imgur.com/YqAxiZ4.png)

* * *

Now, during translating on the crowdin, depending on formatting, you'll see ASF links in the text either as:

* String to translate together with HTML tags (majority of strings, where only part of the sentence is a link)
* Alone string to translate, with link included in `Hidden texts` -> `Link addresses` (rare, where entire string is a link, most common in sidebar)

In our example above, it's the first case (since only "setting up" is a link), so in crowdin we'll see it as:

* * *

![Crowdin 2](https://i.imgur.com/Li5RzS3.png)

* * *

Regardless of case, firstly you click ALT+C (or copy source button) and translate it as usual, leaving entire HTML (if present) in-tact. This would be example of translation for Polish language:

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

### Local links

Across the wiki you will also find local links that point to particular section of the document. Those links start with `#` character.

Now those are special cases, since those links are based on names of the sections of current document. While for URLs we have general convention of adding `-locale` to the URL, and it works everywhere, section names will be translated by you and other people, so you need to ensure that they point to proper location.

For example you can find `#introduction` link in our **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration#introduction)** section:

* * *

![Crowdin 7](https://i.imgur.com/EEHSPtK.png)

* * *

Since we're going to translate "Introduction" word into "Wprowadzenie" for our Polish language, we'll need to correct this link since it'll stop functioning the moment we do this.

* * *

![Crowdin 8](https://i.imgur.com/JMJegO7.png)

* * *

This way our local link will keep working, since it'll now point to name of the section that we're using. You can correct links inside HTML tags in exactly the same way.

* * *

### Code blocks

Be extremely careful when you translate sentences with `<code></code>` blocks inside. Code block indicates fixed ASF code names or terms that should not be translated. Zum Beispiel:

    This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit <code>RateLimited</code> status before you're done with your entire batch.
    

As you can see, `RateLimited` word here is inside a code block and indicates internal ASF code status - this should not be translated. Likewise, you shouldn't translate other code blocks, such as names of config properties (e.g. `TradingPreferences`), enum members (e.g. `Stable` and `Experimental` options of `UpdateChannel`) and likewise.

If you believe that something inappropriate is included in a code block, or that there is a text that is not in a code block but should be inside it, feel free to ask on our crowdin by creating appropriate **[issue](#issues)**.

* * *

Thank you for helping us translating ASF into all languages spoken worldwide!