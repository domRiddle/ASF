# Локализация

Перевод ASF осуществляется с помощью сервиса Crowdin, который даёт любому возможность участвовать в переводе ASF на любой язык мира. Для более подробного объяснения, как работает Crowdin, прочтите пожалуйста **[введение в Crowdin](https://support.crowdin.com/crowdin-intro)**.

Если вам интересно, как проходит локализация, вы можете посмотреть **[Ленту активности ASF в Crowdin](https://crowdin.com/project/archisteamfarm/activity_stream)**.

* * *

## Цели и задачи

Наша платформа поддерживает локализацию основной программы ASF, а также полную локализацию содержимого, поставляемого вместе с ним. В частности, это включает в себя web-конфигуратор, интерфейс IPC, а также эту wiki. Всё это можно переводить через удобный интерфейс Crowdin.

* * *

## Регистрация

Если вы хотите помочь ASF как переводчик, рецензент или корректор, пожалуйста, зарегистрируйтесь на **[странице нашего проекта в Crowdin](https://crowdin.com/project/archisteamfarm)**. Регистрация очень простая и абсолютно бесплатна! После того, как вы зарегистрируетесь и войдёте, вы можете выбрать языки, с которыми вы хотите работать, а затем перейти к строкам ASF и помочь остальному сообществу с переводом ASF на все наиболее популярные языки!

* * *

### Перевод

Если выбранный вами язык ещё имеет непереведенные строки, вы можете взять их и начать работу над переводом. Мы постарались сделать перевод максимально гибким, поэтому многие строки включают в себя дополнительные переменные, которые ASF подставляет во время работы - они выглядят как число в фигурных скобках, например `{0}`. Это позволяет вам менять исходную формулировку строки в ASF, например перемещая пременную ASF в место, соответствующее языку перевода, без жёсткой привязки к контексту и формату. Это особенно важно для языков с письмом справа налево, таким как иврит.

Например, у вас может быть такая строка:

> We have {0} games to idle.

Но из-за структуры вашего языка, следующее предложение будет иметь больше смысла:

> The number of games to idle is equal to {0}.

Или:

> {0} is the number of games to idle.

Эта гибкость специально дана вам чтобы вы могли перефразировать предложения ASF согласно вашему языку, и переместить число (или другую информацию) подставляемое ASF в подходящее для перевода место (а не переводить каждую часть отдельно). Это улучшает общее качество перевода.

* * *

### Рецензирование

Если строка уже была переведена кем-то, вы можете голосовать за неё. Голосование позволяет выбрать лучший вариант перевода, не привязываясь к первому предложенному - это позволяет сделать качество перевода ещё лучше. Вы можете голосовать за имеющиеся предложения, или предложить собственный перевод, который будет участвовать в том же процессе. В конце концов, окончательной будет выбрана версия набравшая большинство голосов, или же выбранная корректором этого языка, который персонально одобряет перевод (основываясь, в том числе, на количестве голосов).

**Вам не нужно одобрение корректора чтобы увидеть перевод строки в ASF**. Одобрение означает только что доверенный человек проверил содержимое и утвердил окончательную версию перевода. Совершенно нормально, если созданный сообществом перевод не был одобрен, если путём голосования был выбран лучший вариант. Если предложение переведено - это главное! Если же вы думаете, что текущий перевод плох, вы всегда можете голосовать за тот, который лучше, или сами предложить лучший вариант!

* * *

### Корректура

Лучше иметь единообразный перевод, даже если для этого может понадобиться ограничить свободу сообщества в рецензировании/голосовании за переводы описанном выше. Основная причина в том, что неправильный перевод (который не обязательно плох) может набрать так много голосов, что не удастся предложить лучший перевод, даже если у кого-то он есть.

Если у вас есть положительная история участия на Crowdin или других платформах по переводу, которую мы можем проверить чтобы считать вас надёжным, мы будем рады дать вам роль корректора в выбранном языке, чтобы вы могли утверждать перевод и делать его единообразным. Корректура не простая задача, особенно учитывая что ASF может иногда использовать достаточно "технический" язык или сленг, и могут возникать сложности перевода, но мы считаем что она нужна для идеального перевода. Поэтому если вы хотели бы помочь в качестве корректора какого-то языка, **[дайте нам знать](https://crowdin.com/messages/create/13177432)**, но помните что вам нужно будет подкрепить своё предложение прошлым участием в локализациях (например, участием в локализации ASF на Crowdin, или в любом другом проекте). Мы можем также позволить более продвинутым пользователям проводить начальную корректуру, если мы знаем их лично или они способны взаимодействовать с остальным сообществом с целью локализации ASF наилучшим для данного языка образом.

Основное правила корректуры - не торопитесь, прислушивайтесь к мнению пользователей, взаимодействуйте с управляющим проекта, решайте возникшие проблемы, и следите за тем чтобы состояние перевода улучшалось, а не ухудшалось.

* * *

### Проблемы

Если вы видите проблему с переводом какой-то конкретной строки, например, вы не знаете как её перевести, или утверждённый перевод неверен, или вам нужно уточнить контекст, и тому подобное - напишите пожалуйста комментарий к этой строке и пометьте его как [X] Issue.

**Пожалуйста, избегайте ставить отметку "issue" если вы нуждаетесь в технической консультации или действии администрации**. Вы можете использовать комментарии для обсуждений связанных с переводом для данной строки, но сообщения о проблемах (с пометкой "issue") следует использовать только если вам нужны подробное техническое пояснение или коррекция со стороны администрации, и обычно в обработке таких сообщений будет участвовать кто-то, кто даже не говорит на языке, на который вы переводите, поэтому пожалуйста пишите по-английски если вы отмечаете комментарий как сообщение о проблеме (чтобы мы могли понять, в чём собственно проблема).

На данный момент поддерживается 4 вида сообщений о проблемах:

* General question - Общие вопросы. Для всего, что не попадает в одну из категорий ниже. В общем случае этого типа сообщений о проблемах **следует избегать**, поскольку если ваша проблема не попадает в одну из категорий, то с большой вероятностью это **не** проблема перевода. Однако, этот вариант доступен для всех прочих случаев.
* Current translation is wrong - Текущий перевод неверен. Этот тип сообщения должен использоваться **только** если перевод был уже утверждён корректором, и вы считаете что он неверен, например в нём присутствуют опечатки или у вас есть предложения как можно этот перевод улучшить. Этот тип сообщения никогда не следует использовать для переводов, выбранных путём голосования сообщества, поскольку в таком случае вам следует связаться с автором перевода и попросить его внести исправления, или просто проголосовать за лучший перевод, как описано в разделе про рецензирование.
* Lack of contextual information - Недостаточно контекстной информации. Этот тип сообщения следует использовать, если вы не уверены, в каком случае ASF использует это сообщение, в каком контексте, или с какой целью. Этот тип должен использоваться только при разработке ASF, и означает что вам нужна техническая консультация, потому что вы не уверены, как следует перевести данную строку.
* Mistake in the source string - Ошибка в исходной строке. Этот тип должен использоваться только если вы полагаете что исходная (английская) строка неверна. Это довольно редкий случай, но поскольку английский не родной для разработчиков, не стесняйтесь пользоваться этим типом сообщений есть у вас идеи по улучшению.

* * *

### Прогресс перевода

Для каждого языка есть две стадии завершенности - перевод и корректура.

Содержимое ASF считается **переведенным** на какой-то язык если прогресс перевода достигает 100%. На этом этапе каждая локализованная строка в ASF имеет правильное значение, и это здорово. Однако это не означает, что нет места улучшениям - голосование сообщества остаётся активным, и вы можете предлагать лучший перевод для уже переведенных частей, и голосовать за существующие. Обратите внимание, что даже языки, перевод на которые был завершён, могут снова упасть ниже 100% если мы внесём изменения в исходные строки или добавим новые в процессе разработки. Вы можете настроить уведомления crowdin если хотите получать e-mail когда такое происходит.

Часть языков может иметь назначенных корректоров, которые проверяют перевод и утверждают окончательную версию. Это последний этап после перевода, позволяющий дополнительно улучшить локализацию.

Переведенные языки будут включены в ASF **настолько быстро, насколько это возможно**, то есть для этого не нужно чтобы перевод был утверждён или даже закончен на 100%. В программу будут всегда включены строки, набравшие максимум голосов, если корректор не решил иначе (что случается редко). Так что вы сможете увидеть результат своих усилий уже в следующей версии ASF после того как перевод появится в Crowdin - мы обычно включаем обновления локализации при выпуске каждой новой версии ASF.

* * *

## Отсутствующие языки

По умолчанию в проекте ASF был открыт перевод только 30 наиболее используемых в мире языков. Если вы хотите добавить ещё одни (или местный диалект уже существующего), пожалуйста **[дайте нам знать](https://crowdin.com/messages/create/13177432)** и мы добавим его как можно скорее. Мы не хотим открывать несколько сотен разных языков если никто не собирается их переводить, поэтому мы ограничились каким-то разумным числом. Не стесняйтесь связаться с нами если вы хотите перевести ASF на какой-то отсутствующий в списке язык, добавить новый язык очень просто.

Чтобы увидеть полный список языков, на которые может быть переведен ASF, **[нажмите здесь](https://support.crowdin.com/api/language-codes)**.

* * *

## Wiki

Платформа Crowdin также позволяет нам локализовать даже саму эту wiki. Это очень мощный инструмент, поскольку это позволяет перевести целиком всю документацию ASF на ваш родной язык, полностью решая задачу локализации ASF. В совокупности с переводом программы и всех её компонентов, это делает локализацию завершённой.

Wiki в этом плане несколько особенная, поскольку это онлайн помощь в которой не нужно жёстко придерживаться исходных предложений. Это означает, что вы можете писать максимально естественно, передавая исходный смысл - не обязательно придерживаясь оригинальных строк, использованных формулировок или пунктуации. Не бойтесь переписывать строки во что-то более естественное для вашего языка, главное сохранить общее направление и помощь содержащуюся в предложении.

* * *

### Global links

Our crowdin platform also allows you to adapt the original text in order to make it point to new (localized) locations.

ASF includes links on almost every page for easier navigation, as well as sidebar on the right. The awesome fact is that you can edit all of that, "fixing" links to point to proper localized pages for your language. It requires to be a bit careful doing that, but it's possible.

For example, ASF **[home page](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home)** includes a text such as:

> Если вы здесь впервые, рекомендуем начать с инструкции по **[установке](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-ru-RU)**.

Which is originally written as:

```markdown
If you're a new user, we recommend starting with **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** guide.
```

On the crowdin, first thing you should do is going to your editor settings and ensuring that HTML tags are set to "Show" for you. This is very important if you decide to localize the wiki.

* * *

![Crowdin](https://i.imgur.com/YqAxiZ4.png)

* * *

Now, during translating on the crowdin, depending on formatting, you'll see ASF links in the text either as:

* Строки для перевода с тегами HTML (большинство строк, где только часть предложения является ссылкой)
* Отдельной строки для перевода, со ссылкой приведенной в разделе `Hidden texts` -> `Link addresses` (Скрытый текст -> Адреса ссылок) (в редких случаях, когда вся строка является ссылкой, в основном встречается в боковой панели)

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

Be extremely careful when you translate sentences with `<code></code>` blocks inside. Code block indicates fixed ASF code names or terms that should not be translated. Например:

    This is especially useful if you have a lot of keys to redeem and you're guaranteed to hit <code>RateLimited</code> status before you're done with your entire batch.
    

As you can see, `RateLimited` word here is inside a code block and indicates internal ASF code status - this should not be translated. Likewise, you shouldn't translate other code blocks, such as names of config properties (e.g. `TradingPreferences`), enum members (e.g. `Stable` and `Experimental` options of `UpdateChannel`) and likewise.

If you believe that something inappropriate is included in a code block, or that there is a text that is not in a code block but should be inside it, feel free to ask on our crowdin by creating appropriate **[issue](#issues)**.

* * *

Спасибо Вам за помощь в переводе ASF на все языки мира!