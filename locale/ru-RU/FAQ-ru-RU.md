# ЧаВО

Наш ЧаВО даёт ответы на стандартные вопросы, которые у вас могут возникнуть. Для получения ответов на менее распространённые вопросы, вы можете посетить **[Расширенное ЧаВО](https://github.com/JustArchi/ArchiSteamFarm/wiki/Extended-FAQ)**.

# Содержание

- [Общие вопросы](#Общие-вопросы)
- [Сравнение с другими программами](#Сравнение-с-другими-программами)
- [Безопасность / Конфиденциальность / VAC / Баны / ToS](#Безопасность--Конфиденциальность--vac--Баны--tos)
- [Разное](#Разное)
- [Возможные проблемы](#Возможные-проблемы)

* * *

## Общие вопросы

### Как это работает?

Чтобы понять, что такое ASF, вам для начала нужно знать что такое коллекционные карточки Steam и как их получить, это хорошо описано в официальном **[руководстве](https://steamcommunity.com/tradingcards/faq)**.

Вкратце, карточки Steam - это коллекционные предметы, право на получение которых вы получаете при покупке некоторых игр. Они могут использоваться для создания значков, продажи на торговой площадке или другим иным образом.

Основные принципы описаны ниже, поскольку это часто вызывает непонимание:

- **Да, вам нужно иметь игру в библиотеке чтобы получить из неё карточки. Игры из Семейного Доступа (Family Sharing) не считаются.**
- **Нет, вы не можете фармить игру бесконечно, из каждой игры выпадает фиксированное количество карточек. Когда карточки в игре закончились, фармить её нет смысла.**
- **Нет, карточки не падают из бесплатных игр формата Free2Play если вы не потратили в этой игре деньги. Это применимо к таким играм, как Team Fortress 2, Dota 2 и др.**
- **Нет, карточки не выпадают на ограниченных аккаунтах (тех, на которых не было потрачено 5$), независимо от того, какие игры есть на этом аккаунте. Ранее это было возможно, но теперь - нет.**

Как видите, вы получаете коллекционные карточки Steam за запуск приобретенной вами игры или за трату денег в бесплатных играх. Другими словами, если вы играете в игру достаточно долго то, все карточки выпадут в инвентарь, давая возможность создать значок, продать их или сделать с ними что-нибудь ещё.

ASF довольно сложная программа, чтобы полностью её понять, поэтому вместо того, чтобы вдаваться в технические детали, мы предлагаем ниже упрощённое описание.

ASF входит в ваш аккаунт Steam с помощью встроенного мини-клиента Steam, используя ваши учётные данные. После успешного входа, программа анализирует страницу с вашими **[значками](https://steamcommunity.com/my/badges)** с целью обнаружить игры, которые можно фармить (Ещё выпадет карточек: X). После анализа страницы и составления списка доступных игр, ASF выбирает наиболее эффективный алгоритм и запускает процесс фарма. Процесс зависит от выбранного **[алгоритма фарма](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**, но, как правило, включает в себя запуск подходящих игр и периодической (или при выпадении предметов) проверки, получены ли все карты. Если да, то ASF переключается на другую игру и повторяет эту процедуру до получения всех карточек.

Имейте в виду, что описание выше - очень упрощенное. Оно не учитывает и десяток из тех настроек и параметров, которые предлагает ASF. Если хотите разобраться во всех деталях работы, то посетите страницу **[нашей wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)**. Я постарался сделать её максимально простой для понимания, не вдаваясь в технические детали. Продвинутым пользователям предлагаю погрузиться глубже.

Как программа, ASF делает немного волшебства. Во-первых, нет необходимости скачивать файлы игры, для того чтобы эту игру запустить. Во-вторых, ASF совершенно не зависит от обычного клиента Steam - вам не нужно, чтобы клиент Steam был запущен или даже установлен. В-третьих - это автоматическое решение - а значит ASF будет автоматически делать всё без необходимости вашего участия - что сбережёт вам много времени и сил. И наконец, у ASF нет необходимости обманывать сеть Steam используя эмуляцию процессов (как например делает Idle Master), поскольку ASF взаимодействует с сетью Steam напрямую. Кроме того, программа очень быстрая и лёгкая, отличное решения для тех, кто хочет получить карточки без особых хлопот - особенно удобно запускать её в фоновом режиме а самому заниматься чем-то ещё, или даже играть в офлайн-режиме.

Всё это хорошо, но у ASF также есть технические ограничения, обусловленные требованиями Steam - мы не можем фармить игры, которых нет у вас в библиотеке, мы не можем фармить игры бесконечно чтобы получить карточек больше нормы, и мы не можем фармить игры когда вы играете на том же аккаунте. Всё это должно быть "логично", учитывая принцип работы ASF, но нужно отметить, что у ASF нет каких-то супер-сил и мы не можем сделать что-то, что физически невозможно, так что имейте это в виду - это всё равно что кто-то зайдёт на ваш аккаунт с другого ПК и будет запускать эти игры вместо вас.

Подытожим - ASF это программа, которая поможет вам получить все доступные для вас карточки без лишних хлопот. Она также включает в себя несколько других функций, но пока ограничимся этим.

* * *

### Мне нужно вводить учётные данные моего аккаунта?

**Да**. Для работы ASF требуются ваши учётные данные, точно так же как для официального клиента Steam, поскольку используются те же методы взаимодействия с сетью Steam. Это, однако, не означает, что ваши учётные данные обязательно сохранять в конфигурационных файлах, вы можете пользоваться ASF указав `null` или пустую строку в `SteamLogin` и/или `SteamPassword`, и вводя свои данные при каждом запуске ASF, по требованию (так же можно поступить и с некоторыми другими данными, читайте раздел посвящённый конфигурированию). При таком подходе ваши данные нигде не сохраняются, но конечно же ASF не сможет стартовать без вашей помощи. ASF также предоставляет ещё несколько способов повысить свою **[безопасность](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security-ru-RU)**, вы можете прочитать о них в соответствующем разделе wiki, если считаете себя продвинутым пользователем. Если нет, и вы не хотите вводить учётные данные в конфигурационных файлах ASF, просто не делайте этого, и вводите их при необходимости по запросу ASF.

Помните о том, что ASF инструмент для персонального использования, и ваши учётные данные никогда не покидают ваш компьютер. Вы также не передаёте их никому, что соответствует условиям использования (далее ToS) Steam - важная вещь, о которой многие забывают. Вы не посылаете ваши данные на наши сервера или любые сервера третьих Сторон, только напрямую на сервера Steam, принадлежащие Valve.

* * *

### Сколько мне ждать пока выпадут карточки?

**Столько, сколько надо**, серьёзно. У каждой игры своя сложность фарма, установленная разработчиком/издателем, и только они решают как быстро будут выпадать карточки. В большинстве игр выпадает примерно 1 карточка в 30 минут игры, но встречаются игры, в которые надо играть несколько часов прежде чем выпадет хотя бы одна карточка. В добавок к этому, ваш аккаунт может иметь ограничение на выпадение карточек из игр, в которые вы играли меньше определённого времени, как описано в разделе "**[Производительность](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance-ru-RU)**". Не пытайтесь предсказать как долго ASF будет фармить определённую игры - это решать не вам и не ASF. Вы не можете ничего сделать чтобы ускорить выпадение, и то что карточки выпадают недостаточно быстро - не "баг", ни вы ни ASF не контролируете процесс выпадения карточек. В лучшем случае вы получите примерно одну карточку в 30 минут. В худшем случае вы не получите ни одной карты за 4 часа со старта ASF. Оба этих случая целиком нормальны и описаны в разделе "**[Производительность](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance-ru-RU)**".

* * *

### Фарм идёт слишком долго, можно его как-то ускорить?

Единственное, что серьёзно влияет на скорость фарма, это выбранный **[алгоритм фарма](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance-ru-RU)** для вашего бота. Эффектом от всего остального можно пренебречь, ничто не сделает фарм быстрее, и в то же время некоторые действия, например запуск нескольких копий ASF, могут его **замедлить**. Если вам очень уж хочется сэкономить каждую секунду фарма, ASF позволяет вам провести тонкую настройку переменных, относящихся к фарму, таких как `FarmingDelay` - все они описаны в разделе, посвященном **[конфигурированию](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU)**. Однако, как и было сказано, эффект будет незначительным, и выбор правильного алгоритма фарма для конкретного аккаунта это единственный важный выбор, который серьёзно влияет на скорость фарма, все остальные изменения - чисто косметические. Вместо того, чтобы беспокоиться о скорости фарма, просто запустите ASF и дайте ему делать свою работу - я уверяю вас, он сделает это самым эффективным способом. Чем меньше вы беспокоитесь, тем более будете удовлетворены.

* * *

### Но ASF пишет что фарм займёт X времени!

ASF даёт грубую оценку основываясь на количестве оставшихся карточек и выбранном алгоритме фарма - эта величина далека от рельного времени фарма, который скорее всего займёт больше времени, поскольку ASF рассчитывает идеальный случай и не учитывает глюки сети Steam, отключения интернета, большую нагрузку на сервера Steam и тому подобное. Это время следует рассматривать просто как индикатор того, сколько примерно вам ждать в лучшем случае, и это реальное время может отличаться, иногда даже значительно. Как сказано выше - не пытайтесь угадать, сколько будет длится фарм игры, ни вы ни ASF не можете на это повлиять.

* * *

### Может ли ASF работать на моем смартфоне?

ASF это программа, написанная на C# и требующая работоспособной реализации .NET Core. На данный момент нет официальных сборок .NET Core для Android, но есть работоспособные сборки для линукс на архитектуре ARM, поэтому вполне возможно использовать утилиту, аналогичную **[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)** чтобы установить на ваш телефон линукс, а затем запустить ASF в этом линуксе как обычно.

Вполне возможно что в будущем мы увидим .NET Core работающий непосредственно под Android.

* * *

### Может ли ASF фармить предметы из игр Steam, таких как CS:GO или Unturned?

**Нет**, это противоречит ToS Steam, и Valve ясно дали это понять прошлой волной банов сообщества за фарм предметов TF2. ASF это программа для фарма карточек Steam, не для фарма внутриигровых предметов - в ней нет возможности фарма внутриигровых предметов, и добавление такого функционала в будущем не планируется, в основном из-за того что это нарушает правила пользования Steam. Пожалуйста, не просите об этом - максимум что вы получите это рапорт от какого-то обиженного пользователя и проблемы из-за этого.

* * *

### Могу ли я выбрать какие игры нужно фармить?

**Да**, даже несколькими способами. Если вы хотите поменять порядок очередности фарма, принятый по-умолчанию, вы можете использовать **[параметр конфигурации бота](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Конфигурация-бота)** `FarmingOrders`. Если вы хотите вручную исключить определённые игры из автоматического фарма, вы можете создать "черный список" **[командой](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `ib`. Если вы хотите фармить все игры, но задать для некоторых из них приоритет над всем остальным, вы можете создать приоритетную очередь фарма **[командой](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `iq`. И наконец, если вы хотите фармить только выбранные вами игры, вы можете воспользоваться **[параметром конфигурации бота](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Конфигурация-бота)** `IdlePriorityQueueOnly` и добавить нужные игры в приоритетную очередь фарма.

В дополнение к настройками модуля автоматического фарма, описанным выше, вы можете также переключить ASF в ручной режим **[командой](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `play`, или использовать другие дополнительные функции, такие как **[параметр конфигурации бота](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration-ru-RU#Конфигурация-бота)** `GamesPlayedWhileIdle`.

* * *

### Я пользователь Linux / OS X, будет ли ASF фармить игры которые не доступны для моей OS? Будет ли ASF фармить 64-битные игры если я запущу его на 32-битной ОС?

Да, ASF даже не скачивает файлы игры, поэтому он будет работать со всеми лицензиями на вашем аккаунте Steam, независимо от платформы или технических требований. Он также должен работать и с играми, привязанными к конкретному региону (игры с "регион-локом") даже если вы не находитесь в подходящем регионе, но мы не проверяли это.

* * *

## Сравнение с другими программами

* * *

### ASF это клон Idle Master?

Единственное сходство - это общее назначение обеих программ, имитация запуска игр Steam для получения коллекционных карточек. Всё остальное, включая сам метод имитации запуска игр, используемые алгоритмы, структура программы, функционал, совместимость и заканчивая исходным кодом, полностью различно и у этих двух программа нет ничего общего между собой, даже базовой системы (IM работает на .NET Framework, ASF на .NET Core). Целью создания ASF было решение проблем IM, которые нельзя решить простым изменением кода - поэтому ASF создавался с нуля, не использовав ни единой строки кода и ни даже основной идеи из IM, потому что этот код и идеи имели принципиальные недостатки. IM и ASF это как Windows и Linux - и то и другое операционная система и может быть установлена на ваш ПК, но между ними нет почти ничего общего, кроме общих целей, которым они служат.

И поэтому не стоит сравнивать ASF с IM основываясь на ожиданиях от IM. Относитесь к ASF и IM как к полностью независимым программам, с разными наборами функций. Некоторые из них действительно пересекаются, и часть функций вы найдёте в обеих программах, но это редкость, поскольку ASF выполняет свою задачу совершенно другим путём по сравнению с IM.

* * *

### Есть ли смысл переходить на ASF, если я пользуюсь Idle Master и меня всё устраивает?

**Да**. ASF гораздо надёжнее и включает много функций которые **необходимы** независимо от того, сколько вы фармите, и которые просто отсутствуют в IM.

В ASF реализована правильная логика работы с **невышедшими играми** - IM будет пытаться фармить карты из таких игр, даже если они ещё не вышли. Разумеется, получить карты из игр до даты релиза невозможно, поэтому фарм в таком случае зависнет. Поэтому вам понадобится либо вручную добавлять игру в черный список, либо ждать релиза, либо пропускать её вручную. Ни одно из этих решений не является хорошим, все они требуют вашего внимания, в то время как ASF автоматически пропустит (временно) фарм невышедших игр, и вернётся к ним позже, когда они выйдут, полностью решая эту проблему.

ASF также корректно обрабатывает **многосерийные** видео. В Steam есть много видео из которых выпадают карточки, однако для них указаны неверные `appID` на странице значков, как например для **[Double Fine Adventure](https://store.steampowered.com/app/402590)** - IM будет пытаться фармить неверное `appID`, при этом карточки выпадать не будут и процесс зависнет. Опять же, вам придётся либо добавлять его в черный список либо пропускать вручную, и оба варианта требуют вашего внимания. ASF автоматически обнаруживает правильное `appID` для фарма, которое обеспечивает выпадение карточек.

В дополнение к этому, ASF **намного стабильнее и надёжнее** когда дело доходит до проблем с сетью и глюков Steam - ASF корректно работает большую часть времени, и не требует вашего внимания после начальной настройки, в то время как IM у многих пользователей "ломается", требуя дополнительных действий для восстановления работоспособности, или не работает вовсе. Также он полностью зависит от клиента Steam, и как следствие не может работать если с вашим клиентом Steam какие-то проблемы. Для правильной работы ASF небоходимо только подключиться к сети Steam, и нет необходимость запускать и даже устанавливать клиент Steam.

Это 3 **очень важных** причины, из-за которых вам стоит подумать о использовании ASF, поскольку они непосредственно влияют на всех, кто фармит карточки Steam, и никто не может сказать "это меня не касается", поскольку техобслуживание Steam и его глюки касаются каждого. Также есть десятки дополнительных более или менее важных причин, о которых вы можете узнать дочитав до конца эти ЧаВО. Говоря короче, **да**, вам стоит использовать ASF даже если вам не нужны никакие дополнительные функции которые предоставляет ASF в сравнении с IM.

В добавок, IM официально не поддерживается, и может полностью перестать работать в будущем, и никто не станет его чинить, учитывая существование более эффективных решений (не только ASF). У многих людей IM уже не работает, и число таких людей только возрастает, но не уменьшается. Вам стоит избегать использования устаревшего программного обеспечения, не только IM, но и других не поддерживаемых программ. Отсутствие активного разработчика означает что никому нет дела, работает программа или нет, никто её не проверяет и **никто не отвечает за её функционал**, что является чрезвычайно важным с точки зрения безопасности.

* * *

### Какие интересные функции, отсутствующие в Idle Master, может предложить ASF?

Зависит от того, что вы считаете "интересным". Если вы планируете фармить больше чем на одном аккаунте - ответ очевиден, поскольку ASF позволяет фармить одновременно все ваши аккаунты без лишних хлопот, проблем с совместимостью, и не используя много ресурсов. Однако, если вы задаёте такой вопрос, вероятно эта возможность вам не нужна, поэтому давайте оценим прочие достоинства, которые вы можете получить даже с одним аккаунтом используемом в ASF.

Первое и главное, у вас будут некоторые встроенные функции, упомянутые **[выше](#Есть-ли-смысл-переходить-на-asf-если-я-пользуюсь-idle-master-и-меня-всё-устраивает)**, которые необходимы для фарма независимо от ваших целях, и часто уже этого достаточно чтобы подумать об использовании ASF. Но это вы уже знаете, поэтому перейдём к некоторым более интересным функциям:

- **Вы можете фармить оффлайн** (параметр `OnlineStatus` равный `Offline`). Фрам офлайн делает возможным не отображать статус "В игре" в Steam, что поволяет вам фармить с помощью ASF и в то же время иметь статус "Online" в Steam, чтобы ваши друзья не видели что ASF запускает за вас игры. Это превосходная функция, поскольку позволяет вам быть онлайн в клиенте Steam, не докучая вашим друзьям постоянными уведомлениями о смене игр, и не вводя их в заблуждение что вы играете, когда на самом деле нет. Уже одно это делает использование ASF оправданным, если вы уважаете своих друзей, но это только начало. Также рады отметить, что эта функция никак не связана с настройками приватности Steam - если вы сами запустите игру, вы будете отображены для всех друзей как "В игре", таким образом работа ASF никак не влияет на ваш аккаунт.

- **Вы можете пропускать игры, на которые можно оформить возврат** (параметр `IdleRefundableGames`). В ASF присутствует логика для работы с играми, за которые можно запросить возврат средств, и вы можете настроить ASF так, чтобы он не фармил такие игры автоматически. Это позволяет вам оценить, стоит ли свежекупленная в Steam игра своих денег, не давая ASF попытаться получить карточки как можно скорее. Если вы играли в игру более 2 часов, или с момента покупки прошло 2 недели, ASF продолжит фарм игры, поскольку запросить возврат средств за неё уже нельзя. До тех пор, вы полностью контролируете запуск игры, и при необходимости можете запросить возврат средств за неё, и для этого вам не нужно добавлять игру в черный список или выключать ASF.

- **Вы можете автоматически отмечать уведомления о новых предметах как просмотренные** (Значение `DismissInventoryNotifications` в параметре `BotBehaviour`). Фарм с помощью ASF приведет к получению коллекционных карточек. Вы уже знаете, что это должно произойти, поэтому позвольте ASF убрать эти ненужные уведомления, чтобы ваше внимание привлекали только тогда, когда это действительно важно. Разумеется, если вы этого хотите.

- **Вы можете автоматически получать карточки распродаж в Steam** (параметр `AutoSteamSaleEvent`). ASF позволяет вам автоматически проходить по списку рекомендаций и голосовать в номинациях Steam во время распродажи, разумеется только если вы этого хотите. Это сбережёт вам кучу времени каждый день во время распродажи, и гарантирует что вы никогда не пропустите ежедневное выпадение карточек.

- **Вы можете настроить предпочитаемый порядок фарма по большему количеству параметров** (параметр `FarmingOrders`). Возможно вы хотите сначала фармить игры, которые купили недавно? Или наоборот, самые старые? В соответствии с количеством карточек? Согласно уровню уже созданных значков? По наигранному времени? По алфавиту? По AppID? Или вообще в случайном порядке? Решать вам.

- **ASF может помочь вам дополнить наборы карточек** (значение `SteamTradeMatcher` в параметре `TradingPreferences`). Путём чуть более глубокой настройки, вы можете превратить ваш ASF в полнофункционального бота, который будет автоматически принимать предложения обмена с **[STM](https://www.steamtradematcher.com)**, помогая вам собирать наборы карточек без участия пользователя. ASF даже включает в себя собственный модуль 2ФА ASF, позволяющий вам импортировать ваш мобильный аутентификатор Steam, что позволяет полностью автоматизировать весь процесс принятия подтверждений. Или может быть вы хотите принимать их вручную и хотите чтобы ASF только подготавливал эти предложения обмена для вас? Это, опять же, целиком на ваше усмотрение.

- **ASF может активировать для вас ключи в фоновом режиме** (модуль **[фоновой активации ключей](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer-ru-RU)**). Возможно у вас есть сотня ключей из разных бандлов, которые вам лень активировать самому, открывая много окон и соглашаясь на лицензионное соглашение Steam снова и снова. Почему бы не скопировать список ключей в ASF и не поручить ему эту работу? ASF автоматически активирует все эти ключи в фоновом режиме, предоставив вам отчёт о результатах каждой попытки активации. Более того, если у вас сотни ключей - вы гарантировано рано или поздно столкнётесь с ошибкой "Слишком много попыток активации", и ASF знает об этом и будет терпеливо ждать, пока пройдёт этот таймаут и продолжит с того же места.

Мы можем продолжать и дальше, цитируя тут всю **[ASF wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Home-ru-RU)**, и описывая каждую функцию программы, но надо где-то подвести черту. Поэтому ограничимся этим списком функций, которые вам могут понравится, и даже одна из них может стать главной причиной для вашего решения, мы перечислили достаточно **много**, но ещё больше вы можете найти если прочтёте остальные статьи в нашей wiki. Ах да, и мы даже не углубились детально в такие вещи как API ASF, позволяющее вам создавать скрипты со своими собственными командами, или великолепное управление ботами, поскольку старались ограничиться простыми вещами.

* * *

### ASF быстрее чем Idle Master?

**Да**, хотя объяснение довольно сложное.

Для каждого процесса, запускаемого и выключаемого в вашей системе, клиент Steam автоматически отсылает запрос, содержащий все игры в которые вы сейчас играете - таким образом сеть Steam может вычислить часы в игре и обеспечить выпадение карточек. Однако, сеть Steam учитывает ваше время с 1-секундным интервалом, и отправка нового запроса сбрасывает текущий статус. Другими словами, если вы будете запускать/выключать новый процесс каждые 0.5 секунды, у вас никогда ну упадут карточки, поскольку каждые 0.5 секунды клиент Steam будет отправлять новый запрос и сеть Steam никогда не насчитает 1 секунду игрового времени. Более того, из-за того как работает операционная система, на самом деле довольно часто запускаются и уничтожаются новые процессы, даже без вашего ведома, поэтому даже если вы ничего не делаете на вашем ПК - многие процессы работают в фоне, всё время создавая/уничтожая другие процессы. Idle master основан на клиенте Steam, поэтому этот механизм будет влиять и на его работу.

ASF не основан на клиенте Steam, в нём используется собственная реализация клиента Steam. Благодаря этому, ASF не создаёт никаких процессов, а просто отсылает один, реальный, запрос к сети Steam, говорящий о том что мы запустили игру. Это такой же запрос как те, что отсылает клиент Steam, но из-за того что у нас есть контроль над клиентом Steam ASF, нам не нужно создавать новые процессы, и мы не повторяем поведение клиента Steam в том, чтобы отсылать запросы по каждому изменению в запущенных процессах, поэтому описанный ваше механизм не будет работать. Благодаря этому, мы никогда не прерываем 1-секундный интервал, учитываемый на серверах Steam, что увеличивает скорость фарма.

* * *

### Но реально ли заметна эта разница?

Нет. Прерывания которые происходят в обычном клиенте Steam и, соответственно, в Idle Master, оказывают незначительный эффект на выпадение карточек, поэтому нет никакой заметной разницы, которая давала бы ASF превосходство.

However, there **is** a difference, and you can clearly notice that, as depending on how busy your OS is, cards **will** drop faster, from a few seconds to even a few minutes, if you're extremely unlucky. Although I wouldn't consider using ASF only because it drops cards faster, as both ASF and Idle Master are affected by how steam web works, ASF just interacts with steam web more effectively, while Idle Master can't control what steam client is actually doing (so it's not Idle Master's fault, but steam client's itself).

* * *

### Может ли ASF фармить карточки из нескольких игр одновременно?

**Yes**, although ASF knows better when to use that feature, based on selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. You do not have direct choice on cards farming algorithm, but you can suggest ASF one, via setting config properties properly. You should focus on configuration part of the ASF, and let algorithms decide what is the most optimal way to achieve the goal.

* * *

### Can ASF skip through games fast?

**No**, ASF doesn't support, neither encourages usage of **[Steam glitches](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance#steam-glitches)**.

* * *

### Может ли ASF автоматически фармить карточки в каждой игре в течении X часов до того, как карточки выпадут?

**No**, the whole point of Steam cards system change was to fight with false statistics and ghost players. ASF won't contribute towards that more than necessary, adding such feature is not planned and won't happen.

* * *

### Могу ли я играть в игры пока ASF фармит?

**Нет**. ASF unlike IM has independent Steam client included, and Steam network allows only **one Steam client at a time** to play a game. You can however disconnect ASF any time you like by starting a game (and clicking "OK" when asked if Steam network should disconnect other client) - ASF will then patiently wait till you're done playing, and resume the process afterwards. Alternatively, you can still play in offline mode anytime you like, if that is satisfying for you.

Keep in mind that cards drop rate when playing multiple games is close to 0 anyway, therefore there are no direct benefits from being able to do that with IM, while there are strong benefits of no interfering with other games launched with ASF, which is crucial e.g. VAC-wise.

* * *

## Безопасность / Конфиденциальность / VAC / Баны / ToS

* * *

### Могу ли я получить VAC бан за использование ASF?

No, it's not possible because ASF (unlike Idle Master or SAM) does not interfere in any way with steam client nor its processes. It's physically impossible to get VAC ban for using ASF, even during playing on secured servers while ASF is running - this is because **ASF doesn't even require Steam Client being installed at all** in order to work properly. ASF is the only farming program that can currently guarantee being VAC-free.

* * *

### Can using ASF prevent me from playing on VAC-secured servers, as stated **[here](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**?

ASF does not require Steam client being running or even installed at all. According to this concept, it should **not** cause any VAC-related issues, because ASF guarantees lack of interfering with Steam client and all its processes - this is the main point when talking about VAC-free guarantee that ASF offers.

According to users and best of my knowledge, this is the case right now, as nobody reported any issues like stated in the link above while using ASF. We couldn't reproduce the issue above with ASF as well, while clearly reproducing it with Idle Master.

However, keep in mind that Valve might still add ASF to the blacklist at some point, but it's a complete nonsense as even if they do that, you could still play VAC-secured games from your PC, and use ASF at the same time e.g. on your server, so I'm pretty sure that they know very well that ASF should not be a suspect VAC-wise, and they won't make our lifes harder by blacklisting ASF for no reason. Still, in the worst case you'll be unable to play, like stated above, because VAC-free guarantee of ASF is still here regardless if Steam blacklists ASF binary, or not (and you can still launch ASF on any other machine without Steam client being installed at all). Right now there is no need to do any of that, and let's hope it stays like this.

* * *

### Это безопасно?

If you ask if ASF is safe as a software, which means that it won't cause any damage to your computer, won't steal your private data, install viruses or any other stuff like that - it is safe. Code is open-source, and distributed binaries are always compiled from **[publicly available sources](https://en.wikipedia.org/wiki/Open-source_software)** by **[automated and trusted continuous integration systems](https://en.wikipedia.org/wiki/Build_automation)**, and not even developers themselves. Each build is reproducible by following our build script and will result in exactly the same, **[deterministic](https://en.wikipedia.org/wiki/Deterministic_system)** IL (binary) code.

If you for whatever reason don't trust our builds, you can always compile and use ASF from source, including all libraries that ASF is using (such as SteamKit2), which are open-source too. In the end however, it's always a matter of trust to the developer(s) of your application, so you should decide yourself if you consider ASF safe or not, potentially supporting your decision with technical arguments. Do not blindly believe something only because I said so - check yourself, as that's the only way to make sure.

* * *

### Могут ли меня забанить за это?

In order to answer that question, we should take a closer look at **[Steam ToS](https://store.steampowered.com/subscriber_agreement)**. Steam doesn't prohibit using of multiple accounts, in fact, **[it allows it](https://support.steampowered.com/kb_article.php?ref=8625-WRAH-9030#share)** implying that you can use same mobile authenticator on more than one account. What it however doesn't allow is sharing accounts with other people, but we're not doing that here.

The only real point that considers ASF is the following:

> You may not use Cheats, automation software (bots), mods, hacks, or any other unauthorized third-party software, to modify or automate any Subscription Marketplace process.

The question is what in fact is Subscription Marketplace process. As we can read:

> An example of a Subscription Marketplace is the Steam Community Market

We're not modifying or automating subscription marketplace process, if by subscription marketplace we understand steam community market or steam store. However...

> Valve may cancel your Account or any particular Subscription(s) at any time in the event that (a) Valve ceases providing such Subscriptions to similarly situated Subscribers generally, or (b) you breach any terms of this Agreement (including any Subscription Terms or Rules of Use).

Therefore, as with every Steam software. ASF is not authorized by Valve and I cannot guarantee that you won't be suspended if Valve suddenly decides that they're banning accounts using ASF now.

Especially:

> In regard to all Subscriptions, Contents and Services, that are not authored by Valve, Valve does not screen such third party content available on Steam or through other sources. Valve does not assume any responsibility or liability for such third party content. Some third party application software is capable of being used by businesses for business purposes - however, you may only acquire such software via Steam for private personal use.

However, Valve clearly acknowledges "Steam idlers" existing, as stated **[here](https://support.steampowered.com/kb_article.php?ref=2117-ilzv-2837)**, so I'm pretty sure that if they weren't fine with them, they'd already do something instead of pointing out that they might cause problems VAC-wise.

Please note that above is only our interpretation of Steam ToS and various points - ASF is licensed under Apache 2.0 License, which clearly states:

> Unless required by applicable law or agreed to in writing, ASF is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

**TL;DR** - You're using this software at your own risk. It's very unlikely that you can get banned for that, but if you do, you can blame only yourself.

* * *

### Кого-нибудь забанили за это?

**Yes**, we had two incidents so far that resulted in Steam suspension.

First case was a guy with over 1000+ bots getting trade banned (together with all bots), most likely due to excessive usage of `loot ASF` executed on all bots at once, or other suspicious one-side amount of trades in very short time.

> Hello XXX, Thank you for contacting Steam Support. It looks like this account was used to manage a network of bot accounts. Botting is a violation of the Steam Subscriber Agreement.

Please, use some common sense and don't assume that you can do such crazy things only because ASF allows you to do that. Doing `loot ASF` on over 1k of bots can be easily considered a **[DDoS](https://en.wikipedia.org/wiki/DDoS)** attack, and personally I'm not shocked that somebody got banned for such a thing. Please keep in mind some bare minimum of fair use in regards to Steam service, and *most likely* you'll be fine.

Second case was a guy with 170+ bots getting banned during Steam's 2017 Winter Sale.

> Your account was blocked for violation of the agreement of the subscriber Steam. Judging by the exchanges and other factors, this account was used to illegally collect collectible cards on Steam, as well as related and not only commercial activities. The account has been permanently blocked and Steam Support can not provide additional support on this issue.

This case is once again very hard to analyze, because of vague response where even Steam support is not entirely sure what happened(?) Personally I believe that once again somebody sent massive amount of one-sided trades in a very short timespan, triggering Steam's anti-abuse mechanics.

* * *

ASF is just a tool and it's **your** decision how you're going to make use of it. You do not get banned for using ASF directly, but for **how** you're using it. It can be a helper tool idling just one single account, or a massive farming network made from thousands of bots. In any of those cases, I'm not offering legal advice, and you should decide yourself about your ASF usage in the first place. I'm not hiding any information that could help you, e.g. the fact that ASF got some people banned, as I have no reason to - it's your choice what you want to do with that information. If you ask me - use some common sense, avoid creating a massive network of idling bots, do not send hundreds of trades at the same time, and you *should* be fine. Every single incident of this nature for **some reason** always happened to people running hundreds of accounts in the first place. Whether it's just a coincidence or some actual factor, that's up to you to decide. I'm not offering any legal advice, only giving you my thoughts that you can find useful, or disregard them entirely and operate only on facts linked above.

* * *

### What privacy information ASF discloses?

You can find detailed explanation in **[statistics](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)** section. You should review it if you care about your privacy, e.g. if you're wondering why accounts being used in ASF are joining our Steam group. ASF doesn't collect any sensitive information, and doesn't share it with any third-parties.

* * *

## Прочее

* * *

### Я использую неподдерживаемую ОС, такую, как 32-битная Windows. Могу ли я всё ещё использовать ASF V3?

Yes, and that version is not unsupported in any way, just not officially built. Check out **[compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)** section for generic variant.

* * *

### ASF - супер! Могу ли я сделать пожертвование?

Да, и мы будем очень счастливы услышать, что Вам нравится наш проект! You can find various donation possibilities under every **[release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** and also **[on the main page](https://github.com/JustArchi/ArchiSteamFarm)**. Thank you in advance!

* * *

### I'm using Steam parental PIN to protect my account, do I need to input it somewhere?

Yes, you must set it in `SteamParentalPIN` bot config property. This is mainly because ASF does access many protected parts of your Steam account and it's impossible for ASF to operate without it.

* * *

### I don't want ASF to farm any games by default, yet I want to use extra ASF features. Is this possible?

Yes, you can set `Paused` bot config property to `true` in order to launch ASF with paused cards farming module, then you can make use of extra ASF features, such as `GamesPlayedWhileIdle`.

* * *

### Может ли ASF сворачиваться в трей?

ASF - консольное приложение, у которого нет окна, чтобы сворачиваться, поэтому окно создаётся для Вас Вашей ОС. You can however use any third-party tool capable of doing so, such as **[RBTray](http://rbtray.sourceforge.net)** for Windows, or **[screen](https://linux.die.net/man/1/screen)** for Linux/OS X. Those are only examples, there are many other apps with similar functionality.

* * *

### Does using ASF preserve eligibility for receiving booster packs?

**Да**. ASF is using the same method to log in to Steam network as the official client, therefore it also preserves ability to receive booster packs for accounts that are being used. However, it's not confirmed if logging in to Steam community is actually required or not, therefore in order to be sure I suggest to keep `OnlineStatus` on `Online`, at least until somebody confirms that using Steam network alone is enough. It should be, but I'm not sure.

* * *

### Is there any way to communicate with ASF?

Yes, through steam chat, or by using **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)**. Check out **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** section for more info.

* * *

### Я хочу помочь с переводом ASF. Что мне нужно для этого сделать?

Thank you for your interest! You can find all details in our **[localization](https://github.com/JustArchi/ArchiSteamFarm/wiki/Localization)** section.

* * *

### I have only one (main) account added to ASF, can I still issue commands through steam chat?

**Yes**, it's explained in **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands#notes)** section. You can do so through Steam group chat.

* * *

### ASF seems to be working, but I'm not receiving any card drops!

Cards farming rate differs from game to game, as you can read in **[performance](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)**. It takes a while, usually **several hours per game**, and you shouldn't expect cards to drop in a few minutes since launching a program. If you can see that ASF actively checks cards status, and switches the game after current one is fully idled, then everything works fine - you're probably referring to inventory notifications, which are automatically dismissed by ASF through `BotBehaviour` of `DismissInventoryNotifications` bot config property. Check out **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** for details.

* * *

### How to completely stop ASF process for my account?

Simply shutdown the ASF process, for example by clicking [X] on Windows. If instead you want to stop a particular bot of your choice but keep other ones running, then take a look at `Enabled` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)**, or `stop` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If you instead want to stop automatic idling process, yet keep ASF running for your account, then that's what `Paused` **[bot config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#bot-config)** and `pause` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** is for.

* * *

### Как много ботов я могу запустить с ASF?

ASF as a program doesn't have any upper limit of bot instances, however you're still being limited by Steam Network. Currently you can run up to 100-110 bots with single IP and single ASF instance. It is possible to run more bots with more IPs and more ASF instances. Keep in mind that if you're using that big amount of bots, you should control their number yourself (such as making sure that all of them in fact are logging in and working at the same time). Also notice that the limit above in general depends on many internal factors - it's approximation rather than strict limit - you will most likely be able to run more/less bots than specified above.

* * *

### Can I run more ASF instances then?

You can run as many ASF instances on one machine as you like, assuming every instance has its own directory and its own configs, and account used in one instance is not used in another one. However, ask yourself why you want to do that. ASF is optimized to handle a dozen, even a hundred of accounts at the same time, and launching those dozen of bots in their own ASF instances degrades performance, takes more OS resources, and causes lack of synchronization between bots - so for example you're more likely to hit `InvalidPassword/RateLimitExceeded` issue described below, as logging in requests are not being synchronized between ASF instances. Therefore, my **strong suggestion** is, always run maximum of one ASF instance per one IP/interface. If you have more IPs/interfaces, by all means you can run more ASF instances, every instance using its own IP/interface. If you don't, launching more ASF instances is totally pointless, and does not only degrade performance and takes more OS resources (such as memory), but also causes lack of synchronization and increased likehood of causing issues. You won't gain anything from launching more than 1 instance per a single IP/interface.

* * *

### What is the meaning of status when redeeming a key?

Status indicates how given redeem attempt turned out. There are many different statuses possible, most common ones include:

| Status                  | Описание                                                                                                                                                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NoDetail                | "OK" status indicating success - the key was successfully redemeed.                                                                                                                                                            |
| Timeout                 | Steam network didn't respond in given time, we don't know if the key was redeemed, or not (most likely was, but you can try again).                                                                                            |
| BadActivationCode       | The provided key is invalid (not recognized as any valid key by Steam network).                                                                                                                                                |
| DuplicateActivationCode | The provided key was already redeemed by some other account, or revoked by developer/publisher.                                                                                                                                |
| AlreadyPurchased        | Your account already owns `packageID` that is connected with this key. Keep in mind that this does not indicate whether the key is `DuplicateActivationCode` or not - only that it's valid and it wasn't used in this attempt. |
| RestrictedCountry       | This is region-locked key and your account is not in the valid region that is permitted to redeem it.                                                                                                                          |
| DoesNotOwnRequiredApp   | You can't redeem that key as you're missing some other app - mainly base game when you're attempting to redeem DLC package.                                                                                                    |
| RateLimited             | You made too many redeem attempts and your account was temporarily blocked. Try again in an hour.                                                                                                                              |

* * *

### Are you affiliated with any cards farming/idling service?

**Нет**. ASF is not affiliated with any service and all such claims are false. Your Steam account is your property and you can use your account in whatever way you wish, but Valve clearly stated in **[official ToS](https://store.steampowered.com/subscriber_agreement/english)** that:

> Any use of your Account with your login and/or password is deemed made by you and you are responsible for it and for the security of your computer system

ASF is licensed on liberal Apache 2.0 License, which allows other developers to further integrate ASF with their own projects and services legally. However, such third-party projects utilizing ASF are not guaranteed to be secure, reviewed, appropriate or legal according to **[Steam ToS](https://store.steampowered.com/subscriber_agreement/english)**. If you want to know our opinion, **we strongly encourage you to NOT share ANY of your account details with third-party services**. If such service turns out to be a **typical scam**, you'll be left alone with the problem, most likely without your Steam account and ASF won't take any responsibility for third-party services claiming to be safe and secure, because ASF team did not authorize neither reviewed any of those. In other words, **you're using them at your own risk, against our suggestion made above**.

In addition to that, official Steam ToS clearly states that:

> You may not reveal, share or otherwise allow others to use your password or Account except as otherwise specifically authorized by Valve

It's your account and your choice. Just don't say that nobody warned you. ASF as a program meets all rules mentioned above, as you're not sharing your account details with anyone, and you're using the program for your own personal use, but any other "cards farming service" does require from you your account credentials, so it also violates the rule above (actually several of them). Like with Steam ToS evaluation, we're not offering any legal advice, and you should decide yourself if you want to use those services, or not - according to us **it directly violates Steam ToS** and might result in suspension if Valve finds out. Like pointed out above, **we strongly recommend to NOT use any of such services**.

* * *

## Возможные проблемы

* * *

### ASF doesn't detect game `X` as available for farming, yet I know it includes Steam trading cards!

There are two main reasons here. First and most obvious reason is the fact that you're referring to **Steam store** where given game is announced as card drops enabled game. This is **wrong** assumption, as it simply states that the game **has** card drops included, but not necessarily this function for that game is **enabled** right away. You can read more about this in **[official announcement](https://steamcommunity.com/games/593110/announcements/detail/1954971077935370845)**.

In short, card drops icon in Steam store doesn't mean anything, check your **[badge pages](https://steamcommunity.com/my/badges)** for confirmation whether a game has card drops enabled or not - this is also what ASF is doing. If your game doesn't appear on the list as a game with cards possible to drop, then this game is **not** possible to idle, regardless of reason.

Second issue is less obvious, and it's the situation when you can see that your game indeed is available with card drops on your badge page, yet it's not being idled by ASF right away. Unless you're hitting some other bug, such as ASF being unable to check badge pages (described below), it's simply a cache effect and on ASF side Steam is still reporting outdated badges page. This issue should solve itself sooner or later, when cache gets invalidated. There is also no way to fix this on our side.

Of course, all of that assumes that you're running ASF with default untouched settings, since you could also add this game to idling blacklist, use `IdleRefundableGames` of `false` and so on.

* * *

### What is the difference between a warning and an error in the log?

ASF writes to its log a bunch of information on various logging levels. Our objective is to explain **precisely** what ASF is doing, including what Steam issues it has to deal with, or other problems to overcome. Most of the time not everything is relevant, this is why we have two major levels being used in ASF in terms of problems - a warning level, and error level.

General ASF rule is that warnings are **not** errors, therefore they should **not** be reported. A warning is an indicator to you that something potentially unwanted happen. Whether it was Steam not reacting, API throwing errors or your network connection being down - it's a warning, and it means we expected it to happen, so don't bother ASF development with it. Of course you're free to ask about them or get help by using our support, but you shouldn't assume that those are ASF errors worth reporting (unless we confirm otherwise).

Errors on the other hand indicate a situation that should not happen, therefore they're worth reporting as long as you made sure that it's not you who is causing them. If it's a common situation that we expect to happen, then it'll be converted to a warning instead. Otherwise, it's possibly a bug that should be corrected, not silently ignored, assuming it's not a result of your own technical issue. For example, removing core `ASF.json` file will throw an error, as ASF can't operate without that file, but it was you who removed it, so you should not report that error to us (unless you confirmed that ASF is wrong and the file is there).

In one TL;DR sentence - report errors, don't report warnings. You can still ask about warnings and receive help in our support sections.

* * *

### ASF не может запуститься, окно программы сразу закрывается после запуска!

If even `log.txt` is not being generated then you most likely forgot to install .NET Core prerequisites, as stated in **[setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up#os-specific-setup)** guide. Other common problems might include trying to launch wrong ASF variant for your OS, or in other way missing native .NET Core runtime dependencies. If the console window closes too soon for you to read the message, then open independent console and launch ASF binary from there. For example on Windows, open ASF directory, hold `Shift`, right click inside the folder and choose "open command window here" (or powershell), then type into the console `.\ArchiSteamFarm.exe` and hit enter. This way you'll get precise message why ASF is not starting properly.

* * *

### ASF is kicking my Steam Client session while I'm playing! / `This account is logged on another PC`

This shows up as a message in Steam overlay that the account is being used somewhere else while you're playing. This issue can have two different reasons.

One reason is caused by broken packages (games) that specifically don't hold a playing lock properly, yet expect that lock to be possesed by the client. An example of such package would be Skyrim SE. Your Steam client launches the game properly, but that game doesn't register itself as being used. Because of that, ASF sees that it's free to resume the process, which it does, and that kicks you out of Steam network, as Steam suddenly detects that the account is being used in another place.

Second reason might come up if you're playing on your PC while ASF is waiting (especially on another machine) and you lose your network connection. In this case, Steam network marks you as offline and releases playing lock (like above), which triggers ASF (e.g. on another machine) into resuming farming. When your PC comes back online, Steam can't acquire playing lock anymore (that is now held by ASF, also similar to above) and shows the same message.

Both causes on the ASF side are actually very hard to workaround, as ASF simply resumes farming once Steam network informs it that account is free to be used again. This is what is happening normally when you close the game, but with broken packages this can happen immediately, even if your game is still running. ASF has no way to know whether you got disconnected, stopped playing a game or that you're still playing a game that doesn't hold playing lock appropriately.

The only proper solution to this problem is manually pausing your bot with `pause` before you start playing, and resuming it with `resume` once you're done. Alternatively you can just ignore the problem and act the same as if you played with offline Steam client.

* * *

### `Disconnected from Steam!` - I can't establish connection with Steam servers.

ASF can only **try** to establish connection with Steam servers, and it can fail due to many reasons, including lack of internet connection, Steam being down, your firewall blocking connection, third-party tools, incorrectly configured routes or temporary failures. You can enable `Debug` mode to check out more verbose log stating exact failure reasons, although usually it's simply caused by your own actions, such as using "CS:GO MM Server Picker" that blacklists a lot of Steam IPs, making it very hard for you to actually reach Steam network.

ASF will do its best to establish connection, which includes not only asking about updated list of servers but also trying another IP when last one fails, so if it's truly a temporary problem with some specific server or route, ASF will connect sooner or later. However, if you're behind firewall or in some other way unable to reach Steam servers, then obviously you need to fix it yourself, with potential help of `Debug` mode.

It's also possible that your machine is not able to establish connection with Steam servers using default protocol in ASF. You can alter protocols that ASF is permitted to use by modifying `SteamProtocols` global configuration property. For example, if you have problems reaching Steam with `TCP` protocol, then you can try `UDP` or `WebSocket`.

In a very unlikely situation of having incorrect servers being cached, for example because of moving ASF `config` folder from one machine to machine located in another country, deleting `ASF.db` in order to refresh Steam servers on next launch might help. Very often it's not needed and doesn't have to be done, as that list is automatically refreshed on first launch, as well as when the connection is established.

* * *

### `Could not get badges information, will try again later!`

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalPIN` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalPIN`.

Other reasons might include temporary Steam problem, network issue or likewise. If issue won't solve itself after several hours and you're sure that you configured ASF appropriately, feel free to let us know about that.

* * *

### ASF is failing with `Request failed despite of 5 tries` errors!

Usually it means that you're using Steam parental PIN to access your account, yet you forgot to put it in ASF config. You must put valid PIN in `SteamParentalPIN` bot config property, otherwise ASF will not be able to access most of web content, therefore will not be able to work properly. Head over to **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** in order to learn more about `SteamParentalPIN`.

If parental PIN is not the reason, then this is a most common error, and you should get used to that - it simply means that ASF sent a request to Steam Network, and didn't get a valid response, in addition to that - in 4 retries. Usually it means that Steam is either down or is having some difficulties or maintenance - ASF is aware of such issues and you should not worry about them, unless they're happening constantly for longer than several hours, and other users do not have such problems.

How to check if Steam is being down? **[Steam Status](https://steamstat.us)** is an excellent source of checking if Steam **should be** up, if you notice errors, especially related to Community or Web API, then Steam is having difficulties, either leave ASF alone and let it do its job after a short while, or wait yourself.

That's however not always the case, as in some situations Steam issues might not be detected by Steam Status, for example such case happened when Valve broke HTTPS support for Steam Community 7th June 2016 - accessing **[SteamCommunity](https://steamcommunity.com)** through HTTPS was throwing an error. Therefore, do not blindly trust Steam Status either, it's best to check yourself if everything works as supposed to.

Lastly, if nothing helps you can always enable `Debug` mode and see yourself in ASF log why exactly requests are failing. For example, above HTTPS issue caused:

    <HTML><HEAD><TITLE>Error</TITLE></HEAD><BODY>
    An error occurred while processing your request.<p>
    

Which is clearly Steam issue and nothing to fix in ASF. You can always try to visit the link mentioned by ASF yourself and check if it works - if it doesn't, then you know why ASF can't access that either. If it does, and error doesn't go away after a day or two, it might be worth investigating and reporting.

Before doing that you should **make sure that the error is worth reporting in the first place**. If it's mentioned in this FAQ, such as trading-related issue, then that's out. If it's temporary issue that happened once or twice, especially when your network was unstable or Steam was down - that's out. However, if you were able to reproduce your issue several times in a row, across 2 days, restarted ASF as well as your machine in the process and made sure that there is no FAQ entry here to help resolve it, then this might be worth asking about.

* * *

### ASF seems to freeze and doesn't print anything on the console until I press a key!

You're most likely using Windows and your console has QuickEdit mode enabled. Refer to **[this](https://stackoverflow.com/questions/30418886/how-and-why-does-quickedit-mode-in-command-prompt-freeze-applications)** question on StackOverflow for technical explanation. You should disable QuickEdit mode by right clicking your ASF console window, opening properties, and unchecking appropriate checkbox.

* * *

### ASF не может принимать и отправлять запросы обмена!

Obvious thing first - new accounts start as limited. Until you unlock account by loading its wallet or spending 5$ in the store, account itself can't accept neither send trades. In this case, ASF will state that inventory seems empty, because every card that is in it is non-tradable. It also won't be possible to receive any trade, as that part requires ASF to be able to fetch API key, and API key functionality is disabled for limited accounts. In short - trading is off for all limited accounts, no exceptions.

Next, if you do not use **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**, it's possible that ASF in fact accepted/sent trade, but you need to confirm it via your e-mail. Likewise, if you use classic 2FA, you need to confirm the trade via your authenticator. Confirmations are **mandatory** now, so if you don't want to accept them by yourself, consider either adding or importing your authenticator into ASF 2FA.

Also notice that you can trade only with your friends, and people with known trade link. If you're trying to initiate Bot->Master trade, such as `loot`, then you need to either have your bot on your friendlist, or your `SteamTradeToken` declared in Bot's config. Make sure that the token is valid - otherwise, you won't be able to send a trade.

Lastly, remember that new devices have 7-days trade lock, so if you've just added your account to ASF, wait at least 7 days - everything should work after that period. That limitation includes **both** accepting **and** sending trades. It does not always trigger, and there are people who can send and accept trades instantly. Majority of the people are affected though, and the lock **will** happen, even if you can send and accept trades through your steam client on the same machine. Just wait patiently, there's nothing you can do to make it faster.

And finally, keep in mind that one account can have only 5 pending trades to another one, so ASF will fail to send trades if you have 5 (or more) pending ones from that one bot to accept already. This is rarily a problem, but it's also worth mentioning, especially if you set ASF to auto-send trades, yet you're not using ASF 2FA and forgot to actually confirm them.

If nothing helped, you can always enable `Debug` mode and check yourself why requests are failing. Please note that Steam talks crap most of the time, and provided reason might not make any sense, or can be even entirely incorrect - if you decide to interpret that reason, make sure you have decent knowledge about Steam and its quirks. It's also quite common to see that issue with no logical reason, and the only suggested solution in this case is to re-add account to ASF (and wait 7 days again). Sometimes this issue also fixes itself *magically*, the same way it breaks. However, usually it's just either 7-days trade lock, temporary steam problem, or both. It's best to give it a few days before manually checking what is wrong, unless you have some urge to debug the real cause (and usually you'll be forced to wait anyway, because error message won't make any sense, neither help you in the slightlest).

In any case, ASF can only **try** to send a proper request to Steam in order to accept/send trade. Whether Steam accepts that request, or not, is out of the scope of ASF, and ASF will not magically make it work. There's no bug related to that feature, and there is also nothing to improve, because logic is happening outside of ASF. Therefore, do not ask for fixing stuff that is not broken, and also do not ask why ASF can't accept or send trades - **I don't know, and ASF doesn't know either**. Either deal with it, or fix yourself.

* * *

### Why do I have to put 2FA/SteamGuard code on each login? / `Removed expired login key`

ASF uses login keys (if you kept `UseLoginKeys` enabled) for keeping credentials valid, the same mechanism that Steam uses - 2FA/SteamGuard token is required only once. However, due to Steam network issues and quirks, it's entirely possible that login key is not saved in the network, I've already seen such issues not only with ASF, but with regular steam client as well (a need to input login + password on each run, regardless of "remember me" option).

You could remove `BotName.db` (+ `BotName.bin`, if exists) of affected account and try to link ASF to your account once again, but that doesn't have to succeed. The real ASF-based solution is to import your authenticator as **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** - this way ASF can generate tokens automatically when they're needed, and you don't have to input them manually. Usually the issue magically solves itself after some time, so you can simply wait for that to happen. Of course you can also ask GabeN for solution, because I can't force Steam network to accept our login keys.

As a side note, you can also turn off login keys with `UseLoginKeys` config property set to `false`, but you should do that only if ASF has fully automated way to make initial login. Right now this is possible only with valid `SteamPassword` and **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**, since in this case we don't need to rely on login keys at all, as we have required login credentials (password and 2FA key) available on each login.

* * *

### I'm getting error: `Unable to login to Steam: InvalidPassword or RateLimitExceeded`

This error can mean a lot of things, some of them include:

- Invalid Login/Password combination (obviously)
- Expired login key used by ASF for logging in
- Too many failed login attempts in short period of time (anti-bruteforce)
- Too many login attempts in short period of time (rate-limiting)
- Requirement of captcha to log in (very likely to be caused by two reasons above)
- Any other reason Steam Network might have preventing you from logging in.

In case of anti-bruteforce and rate-limiting, problem will disappear after some time, so just wait and don't attempt to log in in the meantime. If you hit that issue frequently, perhaps it's wise to increase `LoginLimiterDelay` config property of ASF. Excessive program restarts and other intentional/non-intentional login requests definitely won't help with that issue, so try to avoid it if possible.

In case of expired login key - ASF will remove old one and ask for new one on next login (which will require from you putting 2FA token if your account is 2FA-protected. If your account is using ASF 2FA, token will be generated and used automatically). If you get this issue often, it's possible that Steam for some reason decided to ignore our login key save requests, as mentioned in the issue above. You might avoid this issue by not using login keys at all with `UseLoginKeys` config property, but we do not recommend going that way.

And lastly, if you used wrong login + password combination, obviously you need to correct this, or disable bot that is attempting to connect using those credentials. ASF can't guess on its own whether `InvalidPassword` means invalid credentials, or any of the reasons listed above, therefore it'll keep trying until it succeeds.

Keep in mind that ASF has its own built-in system to react accordingly to steam quirks, eventually it will connect and resume its job, therefore it's not required to do anything if the issue is temporary. Restarting ASF in order to magically fix problems will only make things worse (as new ASF won't know previous ASF state of not being able to log in, and try to connect instead of waiting), so avoid doing that unless you know what you're doing.

Finally, as with every Steam request - ASF can only **try** to log in, using your provided credentials. Whether that request will succeed or not is out of the scope and logic of ASF - there is no bug, and nothing can be fixed neither improved in this regard.

* * *

### `System.Threading.Tasks.TaskCanceledException: A task was canceled.`

This warning means that Steam did not answer to ASF request in given time. Usually it's caused by Steam networking hiccups and does not affect ASF in any way. In other cases it's the same as request failing despite of 5 tries. Reporting this issue makes no sense most of the time, as we can't force Steam to respond to our requests.

* * *

### `System.Net.Http.WinHttpException: A security error occurred`

This error happens when ASF can't establish secure connection with given server, almost exclusively because of SSL certificate mistrust.

In almost all cases this error is caused by **wrong date/time on your machine**. Every SSL certificate has issued date and expiry date. If your date is invalid and out of those two bounds then the certificate can't be trusted as potential MITM attack and ASF refuses to make a connection.

Obvious solution is to set the date on your machine appropriately. It's highly recommended to use automatic date synchronization, such as native synchronization available on Windows, or `ntpd` on Linux.

If you made sure that the date on your machine is appropriate and the error doesn't want to go away, then assuming it's not a temporary issue that should go away soon, SSL certificates that your system trusts might be out-of-date or invalid. In this case you should ensure that your machine can establish secure connections, for example by checking if you can access `https://github.com` with any browser of your choice, or CLI tool such as `curl`. If you confirmed that this works properly, feel free to post issue on our Steam group.

* * *

### ASF is being detected as a malware by my AntiVirus! What's going on?

**Ensure that you downloaded ASF from trusted source**. The only official and trusted source is **[ASF releases](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** page on GitHub (and this is also the source for ASF auto-updates) - **any other source is untrusted by definition and might contain malware added by other people** - you should not trust any other download location by definition, and ensure that your ASF always comes from us.

If you confirmed that ASF is downloaded from trusted source, then very likely it's simply a false positive. This **happened in the past**, **is happening right now**, and **will happen in the future**. We're already used to that and you shouldn't notify us about new false positives - notify developers of your AV instead, since it's not ASF issue in the first place.

If you're worried about actual safety when using ASF, then I suggest scanning ASF with many different AVs for actual detection ratio, for example through **[VirusTotal](https://www.virustotal.com)** (or any other web service of your choice like this).

If the AV that you're using falsely detects ASF as a malware, then **it's a good idea to send this file sample back to developers of your AV, so they can analyze it and improve their detection engine**, as clearly it's not working as good as you think it does. There is no issue in ASF code, and there is also nothing to fix for us, since we're not distributing malware in the first place, therefore it doesn't make any sense to report those false-positives to us. We highly recommend to send ASF sample for further analysis like stated above, but if you don't want to bother with it, then you can always add ASF to some kind of AV exceptions, disable your AV or simply use another one. Sadly, we're used to AVs being stupid, as every once in a while some AV detects ASF as a virus, which usually lasts very short and is being patched up quickly by the devs, but like we pointed out above - **it happened**, **happens** and **will happen** all the time. ASF doesn't include any malicious code, you can review ASF code and even compile from source yourself. We're not hackers to obfuscate ASF code in order to hide from AV heuristics and false positives, so do not expect from us to fix what is not broken - there is no "virus" for us to fix.