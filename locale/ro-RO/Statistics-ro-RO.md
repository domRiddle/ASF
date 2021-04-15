# Statistici

Dezvoltarea ASF este susținută de 3 lucruri majore: donații, feedback-ul utilizatorilor și statistici. Donațiile influențează direct voința noastră de a lucra la proiect, feedback-ul utilizatorilor este întotdeauna plăcut de citit (mai ales unul pozitiv), în timp ce statisticile ne furnizează informaţii cu privire la modul în care este utilizat software-ul nostru, şi de câţi oameni - în acest fel putem şti ce să îmbunătăţim, ce să reparăm şi pe ce să ne concentrăm.

ASF are implicit activată proprietate de configurare globală `Statistics`. Dacă doriţi să vedeţi noi versiuni care apar, ce bug-uri sunt reparate şi ce noi caracteristici sunt implementate, ar trebui să luați în considerare menținerea acestora ca activate, astfel încât să putem utiliza aceste date pentru a vă oferi un software mai bun (dar nu numai). Acest lucru este deosebit de important deoarece ASF îți este oferit **gratuit**, și acesta este minimul pe care îl poți face pentru a mulțumi - **spune-ne că folosești ASF**, întrucât statisticile noastre actuale fac în principal acest lucru. ASF nu colectează sau nu utilizează date care ar putea fi considerate private și/sau utilizate împotriva ta. Menţinem utilizarea statisticilor la minimum, şi fiecare informaţie colectată ne cere să o menţionăm cu precizie aici, împreună cu explicaţii practice. Acest lucru vă oferă o imagine completă a ceea ce colectăm şi cum ar trebui să ajute. Toate aceste informații pot fi găsite mai jos.

* * *

## Politica de confidenţialitate actuală

Când `Statistics` sunt active, se vor întâmpla următoarele:

1. Contul folosit în ASF se va alătura grupului nostru **[Steam](https://steamcommunity.com/gid/103582791440160998)** la autentificare. Acest lucru se realizează din trei motive:

* Iti oferă anunțuri de grup, în special versiuni noi, probleme critice, probleme Steam și alte lucruri importante pentru a ține comunitatea la zi (niciun spam nelegat de ASF garantat)
* Permite să utilizați asistența noastră tehnică, punând întrebări, rezolvând probleme, raportând probleme sau sugerând îmbunătățiri
* Acest lucru ne permite să vedem câte conturi de Steam sunt folosite de ASF

Considerăm că grupul Steam este un element crucial în ceea ce privește comunitatea ASF. Acesta este principalul nostru canal de comunicare pe care îl folosim pentru toate aspectele importante în ceea ce priveşte ASF, în special pentru a vă ţine la curent cu dezvoltarea, eventuale probleme, eventuale avertismente și toate celelalte aspecte la care ar trebui să aveți acces în calitate de utilizator. Nu beneficiem în niciun fel de menţinerea acestui grup, este locul dedicat utilizatorilor ASF și noi vă considerăm parte a comunității noastre. Din moment ce apartenența la grup nu te identifică în niciun fel ca utilizator ASF, nu considerăm că aceasta este o problemă din punctul de vedere al vieţii private.

2. Dacă contul tău este **[nerestricționat](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**, folosind **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**, are **[inventar public](https://steamcommunity.com/my/edit/settings)** cu cel puțin 100 `MatchableTypes` articole în el și ați activat intenționat `SteamTradeMatcher` în `TradingPreferences`, apoi ASF va comunica periodic cu **[serverul nostru](https://asf.justarchi.net)** pentru a ține funcționalitatea activată. Datele efective constau în ID ASF unic (generat de ASF) și următoarele informații legate de cont:

* Your Steam identificator (in 64-bit form, for generating links, public info)
* Your nickname (for display purposes, public info)
* Your avatar (hash, for display purposes, public info)
* **[simbolul tău de tranzacționare](https://steamcommunity.com/my/tradeoffers/privacy)** (astfel încât oamenii din afara listei tale de prieteni îți pot trimite tranzacții)
* Valoarea ta de `MatchableTypes` (pentru scopuri de afișare și potrivire)
* Valoarea de `MatchEverything` în `TradingPreferences` (pentru scopuri de afișare și potrivire)
* Numărul total de `MatchableTypes` articole Steam din inventarul tău (pentru scopuri de afișare și potrivire)
* Numărul total de `MatchableTypes` articole Steam din inventarul tău (pentru scopuri de afișare și potrivire)

ASF **nu va** colecta alte date neînregistrate mai sus fără o notificare prealabilă importantă în changelog, în primul rând pentru că este un motiv practic foarte bun. Nu considerăm că există nimic din lista de mai sus ca fiind o problemă serioasă, menționăm asta pentru a vă anunța exact ce face ASF în plus față de ceea ce l-ați configurat pentru a vă face singur, astfel încât oamenii să poată înțelege mai bine punctul nostru de vedere.

* * *

# Utilizarea datelor

All values specified in second point are being used for our **Public ASF STM listing**, which is explained below. We do not use any other data for any other purpose.

* * *

## Public ASF STM listing

Our public ASF STM listing is located on **[our website](https://asf.justarchi.net/STM)** and used as a public service for both ASF users that make use of `MatchActively`, as well as ASF and non-ASF users for manual matching.

### How it exactly works

ASF sends initial data once after logging in, that contains all properties public listing makes use of. Then, every 10 minutes ASF sends one, very tiny "heartbeat" request that notifies our server that the bot is still up and running. If for some reason the heartbeat didn't arrive, for example due to networking issues, then ASF will retry sending it each minute, until server registers it.

This allows our website to record which accounts can be used for matching, as well as if they're still active. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**.

Users are sorted according to their inventories (in descending order) - `MatchEverything` bots with `Any` banner that accept all 1:1 trades, then `MatchableTypes` unique games count, and finally `MatchableTypes` items count.

Please note that you will **not** be displayed on the website if you do not meet all of the requirements. ASF won't even bother communicating with our server in this case, so second point is entirely skipped for you if you didn't intentionally enable `SteamTradeMatcher` in order to help yourself match dupes. Also public listing is compatible only with latest stable version of ASF and may refuse to display outdated bots, especially if they're missing core functionality that can be found only in newer versions.

### API

ASF STM listing only accepts ASF bots for time being. There is no way to list third-party bots on our listing for now (as we can't review their code easily and ensure they meet our entire trading logic).

If you're looking for easy way to access our listing in programmatic way, we have a very simple **[/Api/Bots](https://asf.justarchi.net/Api/Bots)** endpoint that you can use. This is also the endpoint that ASF uses internally for `MatchActively` users.

### Notice

*The entire concept, together with website integration and ASF reporting is still in beta - it can be improved/changed over time - also removed if we feel like there is not enough interest for this feature.*

* * *

## Opting out

Participating in statistics is **not mandatory**, although highly encouraged for future of the program. We do not judge you, and if you have inner urge of hiding the fact that you're ASF user then you can disable statistics **entirely** by switching `Statistics` global config property to `false`. Disabled statistics make entire module non-operative, and will not do any of actions specified in our privacy policy above.

Disabling statistics may influence **our technical support, ASF functionality and other things that are offered to you for free**. For example, you can't make use of `MatchActively` without having `Statistics` enabled (as you refuse to communicate with our server for the functionality to work).