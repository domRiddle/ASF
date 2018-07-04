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

Or:

> {0} is the number of games to idle.

The flexibility is provided specially for you, so you can slightly reword ASF sentence to fit your language better and move ASF-provided number or other information in a place that fits your translation (instead of translating each part independently). This improves overall translation quality.

* * *

### Rezensierung

If your string was already translated by somebody else, you can vote for it. Voting makes it possible to choose the best variant of the translation, instead of sticking with initial suggestion - this enhances overall translation quality even further. You can vote on already available suggestions, or suggest your own translation, which will go through the same process. Eventually, final string will be chosen either based on most voted suggestion, or as a choice of proofreader selected for that language who personally approves given translation (based on your votes as well).

**You do not need approval to see your translated strings in ASF**. Approval simply means that somebody trusted reviewed the content, as in - picked the final version of the translation. It's totally fine to have not-approved community-driven translations, where you vote for the best one. As long as it's translated, everything is fine! And if you think that current translation is bad, you can always vote for the better one, or suggest one yourself!

* * *

### Korrekturlesen

It's a good idea to have a consistent translation, even if it could potentially take freedom from community review/voting process explained above. This is mainly because incorrect translations that are not necessarily bad might get so many upvotes that it's no longer possible to suggest any better translation, even if somebody has such.

If you have past history of contributions on Crowdin or any other localization platform/service that we can verify and assume trustworthy, we're happy to give you a proof-reader access to given language you're contributing to, so you'll be able to approve given translation and make it consistent. Proof-reading is not an easy task, especially because ASF can be very "technical" from time to time and really difficult to translate, but we understand that it's often needed for a perfect translation. Therefore if you can help by proof-reading given language, **[let us know](https://crowdin.com/messages/create/13177432)**, but keep in mind that you'll need to back up your request with past localization contributions that we can verify (e.g. working with ASF localization on Crowdin, or with any other project). We might also allow more advanced users to pick up initial proof-reading, if we know them personally and they're capable of cooperating with the rest of the community in order to localize ASF in that language best.

General rules apply for proof-reading - do not rush, listen to your users, work as a project manager, resolve issues, ensure that you're making things better and not worse.

* * *

### Probleme

If you have a problem with particular translation, e.g. you do not know how to translate it, approved translation is incorrect, you need more specific context, or likewise, please post a comment under specific string, and mark it with [X] Issue.

**Please avoid using issue mark if you do not need technical/development explanation or admin action**. You're free to use comments for discussion related to translation of given string, but issue should be used only when you need further technical explanation or admin correction, and it will typically involve somebody who do not even speak the language you're translating, so please stick with English when writing issue comment (so we can understand what the issue is).

There are currently 4 supported type of issues:

* generelle Frage - Für alles, was keinen der anderen Typen betrifft. Dieser Typ **sollte vermieden** werden, weil es dann höchstwahrscheinlich **kein** Übersetzungsproblem ist. Trotzdem ist diese Möglichkeit für alle anderen Fälle offen.
* Die aktuelle Übersetzung ist falsch - Dies sollte **nur** verwendet werden, wenn die Übersetzung von einem Korrekturleser genehmigt wurde und du glaubst, dass sie falsch ist, weil sie etwa einen Tippfehler beinhaltet oder du einen guten Vorschlag hast sie zu verbessern. Dieser Typ sollte nie bei Übersetzungen verwendet werden, die durch Community-Stimmen gewählt wurden, in welchem Fall du den Benutzer der die Übersetzung geschrieben hat kontaktieren und ihn um eine Korrektur bitten oder einfach für eine bessere Übersetzung stimmen solltest.
* Fehlender Kontext - Dies ist, was du verwenden solltest, wenn du nicht sicher bist, welchen Teil von ASF du übersetzt und was der Kontext des gegebenen Strings oder sein Sinn ist. Dieser Typ sollte nur für die Entwicklung von ASF verwendet werden, da er bedeutet, dass du technische Hilfe benötigst, weil du nicht sicher bist, wie der String zu übersetzen ist.
* Fehler im Quell-String - Dies sollte nur verwendet werden, wenn du denkst, dass das Original (Englisch) falsch ist. Sehr selten, aber meine eigene Muttersprache ist auch nicht Englisch und deshalb solltest du dich frei fühlen diesen zu verwenden, wenn du eine generelle Idee hast, wie man es verbessern könnte.

* * *

### Übersetzungsfortschritt

Every language has two states of completion - translation, and proof-reading.

Language is considered **translated** when its translation progress reaches 100%. At this point every localizable string used by ASF has proper meaning, which is great. However, that doesn't mean that there is no room for improvement - community voting is enabled all the time and you can still suggest better translation for already-translated parts, as well as vote for existing ones. Please note that fully-translated languages can still drop below 100% when we change existing strings or add new ones during development. You can set up appropriate crowdin notifications if you'd like to receive e-mail when this happens.

Selected languages might have appropriate proof-readers that validate translations and approve final versions. This is final pass after translation takes place and allows to further improve localization.

ASF will include given language **as soon as possible**, which means that it doesn't need to be approved, or even 100% translated. The actual strings that will be used are always the most popular ones in terms of the votes, unless chosen proofreader decided otherwise (rarily). Therefore, you can see your efforts included in the very next ASF release, as soon as translation is pushed to Crowdin - we typically merge localization updates the moment we're about to release new ASF version.

* * *

## Fehlende Sprachen

By default ASF project has open translation only for top 30 languages that are spoken worldwide. If you'd like to add another one (or a local dialect to already available one), please **[let us know](https://crowdin.com/messages/create/13177432)** and we'll add it ASAP. We don't want to open several hundred different languages if nobody is going to translate them, that's why we limited it to some fair number. Please don't hesitate to contact us if you'd like to translate some not-listed language, it's very easy for us to add another one.

For a complete list of all available languages that ASF can be translated to, **[click here](https://support.crowdin.com/api/language-codes)**.

* * *

## Wiki

Our crowdin platform also allows you to localize even the wiki itself. This is a very powerful tool, since it allows you to create a whole ASF documentation in your native language, effectively solving the very last issue when it comes to ASF localization. Together with translation of the program and all its parts, this makes localization complete.

Wiki is a bit special in this regard, since it's online help where you don't need to stick with original sentence too much. This means that you want to be as natural with your language as possible, and deliver original meaning and help - not necessarily stick with original string, used words and actual punctuation. Don't be afraid of rewriting the string into something far more natural for your language, as long as you keep the general direction and help included in the sentence.

* * *

### Global links

Our crowdin platform also allows you to adapt the original text in order to make it point to new (localized) locations.

ASF includes links on almost every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit all of that, "fixing" links to point to proper localized pages for your language. It requires to be a bit careful doing that, but it's possible.

For example, ASF **[home page](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home)** includes a text such as:

> Wenn du ein neuer Benutzer bist, empfehlen wir dir mit dem Leitfaden zur **[Installation](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-de-DE)** anzufangen.

Which is originally written as:

```markdown
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** guide.
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
Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.
```

And finally into wiki text:

> Jeśli jesteś nowym użytkownikiem, zalecamy rozpoczęcie od korzystania z **[przewodnika po konfiguracji](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-pl-PL)**.

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

For example you can find `#introduction` link in our **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#introduction)** section:

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