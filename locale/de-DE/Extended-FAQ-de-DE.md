# Erweiterte FAQ

In unserem erweiterten FAQ finden Sie Antworten auf die etwas weniger häufig gestellten Fragen die Sie eventuell haben. Für häufiger gestellte Fragen besuchen Sie bitte stattdessen unser **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-de-DE)**.

* * *

### Wer hat ASF erschaffen?

ASF wurde von **[Archi](https://github.com/JustArchi)** im Oktober 2015 erschaffen. Falls Sie sich wundern- ich bin ein **[Steam-Benutzer](https://steamcommunity.com/profiles/76561198006963719)** genau wie Sie. Neben dem Spielen stelle ich auch gerne meine Fähigkeiten und meine Entschlossenheit zur Verfügung, wie Sie in Kürze feststellen können. Hier ist weder eine große Firma, noch ein Team von Entwicklern noch ein Budget von einer Millionen-Dollar verfügbar um das alles abzudecken - Nur ich, der Dinge repariert, die noch nicht kaputt sind.

ASF ist jedoch ein Open-Source-Projekt, und ich kann nicht genug ausdrücken, dass ich nicht hinter allem stehe, was man hier sehen kann. Wir haben ein paar **[andere](https://github.com/JustArchiNET?q=ASF-)** ASF-Projekte, die fast ausschließlich von anderen Entwicklern entwickelt werden. Sogar das ASF-Kernprojekt hat eine Vielzahl von **[Mitwirkenden](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)**, die mir geholfen haben, all dies zu erreichen. Darüber hinaus gibt es mehrere Drittanbieter-Dienste, welche die ASF-Entwicklung unterstützen, insbesondere **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)**, **[AppVeyor](https://www.appveyor.com)** und **[Crowdin](https://crowdin.com)**. Und natürlich darf man nicht all die fantastischen Bibliotheken und Programme vergessen die ASF überhaupt erst ermöglicht haben. Insbesondere **[Rider](https://www.jetbrains.com/rider)**, welches wir als IDE verwenden und wir lieben die **[ReSharper](https://www.jetbrains.com/resharper)**-Erweiterungen und besonders **[SteamKit2](https://github.com/SteamRE/SteamKit)** (ohne dessen ASF überhaupt nicht existieren würde). ASF wäre auch nicht dort, wo es heute ist, ohne dass meine **[Sponsoren](https://github.com/sponsors/JustArchi)**, **[Patreons](https://www.patreon.com/JustArchi)** und diverse Spender mich in allem, was ich hier tue, unterstützen.

Vielen Dank an alle, die bei der ASF-Entwicklung geholfen haben! Ihr seid großartig!

* * *

### Warum wurde ASF in erster Linie erstellt?

ASF wurde mit dem Hauptziel entwickelt, ein vollautomatisches Steam-Karten-Sammel-Programm für Linux zu sein, ohne dass externe Abhängigkeiten (z.B. Steam-Client) erforderlich sind. Tatsächlich bleibt dies immer noch der Hauptzweck und Fokus, denn mein Konzept von ASF hat sich seitdem nicht geändert und ich benutze es immer noch genauso, wie ich es bereits 2015 benutzt habe. Natürlich gab es seitdem wirklich **viele** Änderungen, und ich bin sehr froh zu sehen wie weit ASF gekommen ist, vor allem dank seiner Benutzer, denn ich würde nie auch nur die Hälfte der Features programmieren wenn es für mich selbst wäre.

Es ist gut zu wissen, dass ASF nie dazu bestimmt war mit anderen Sammelprogrammen zu konkurrieren, besonders nicht mit **[Idle Master](https://www.steamidlemaster.com)**, weil ASF nie als Desktop/Benutzer-App konzipiert wurde und es auch heute noch nicht ist. Wenn Sie den Hauptzweck von ASF, wie oben beschrieben, analysieren, dann werden Sie feststellen, dass Idle Master **das genaue Gegenteil** von all dem ist. Obwohl man heute auf jeden Fall ähnliche Sammelprogramme zu ASF finden kann, war mir damals nichts gut genug (und ist es heute immer noch nicht), also habe ich mein eigenes Sammelprogramm erstellt, so wie ich es wollte. Benutzer sind im Laufe der Zeit hauptsächlich aufgrund von Robustheit, Stabilität und Sicherheit auf ASF umgestiegen aber auch alle Funktionen, die ich in all diesen Jahren entwickelt habe. Heute ist ASF besser als je zuvor.

* * *

### Okay, wo ist der Haken? Was nützt Ihnen das Teilen von ASF?

Es gibt keinen Haken. Ich habe ASF **für mich selbst** erstellt und es mit dem Rest der Community geteilt in der Hoffnung, dass es nützlich sein wird. Genau das Gleiche geschah 1991, als Linus Torvalds **[seinen ersten Linux-Kernel](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** mit dem Rest der Welt teilte. Es gibt keine versteckte Malware, Data-Mining, Crypto-Mining oder andere Aktivitäten, die mir einen finanziellen Nutzen bringen würden. ASF Projekt wird vollständig durch nicht-obligatorische Spenden unterstützt, die von glücklichen Benutzern wie Ihnen gesendet werden. Sie können ASF genauso verwenden wie ich es benutze und wenn Sie es mögen, Sie können mir immer einen Kaffee kaufen, der Ihre Dankbarkeit für das, was ich tue, zeigt.

Ich benutze auch ASF als perfektes Beispiel für ein modernes C# Projekt, das immer für Perfektion und das bestmögliche Verfahren sorgt, sei es mit Technologie, Projektmanagement oder dem Code selbst. Es ist meine Definition von "Sachen richtig angehen/erledigen". Wenn Sie durch irgendeinen Zufall etwas Nützliches aus meinem Projekt lernen, dann wird das mich nur glücklicher machen.

* * *

### I've launched the program on April the 1st and the ASF language changed into something strange, what is going on?

CONGRATULASHUNS ON DISCOVERIN R APRIL FOOLS EASTR EGG! If you didn't set custom `CurrentCulture` option, then ASF launched on April the 1st will actually use **[LOLcat](https://en.wikipedia.org/wiki/Lolcat)** language instead of your system language. If by any chance you'd like to disable that behaviour, you can simply set `CurrentCulture` to the locale that you'd like to use instead. It's also nice to note that you can enable our easter egg unconditionally, by setting your `CurrentCulture` to `qps-Ploc` value.