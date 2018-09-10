# Конфигурация

Эта статься посвящена конфигурированию ASF. Здесь содержится полная документация по содержимому папки `config`, прозволяющая настроить ASF под ваши нужды.

- **[Введение](#Введение)**
- **[Сетевой генератор конфигураций](#Сетевой-генератор-конфигураций)**
- **[Ручная настройка](#Ручная-настройка)**
- **[Файл глобальной конфигурации](#Файл-глобальной-конфигурации)**
- **[Конфигурация бота](#Конфигурация-бота)**
- **[Файловая структура](#Файловая-структура)**
- **[Типы параметров в JSON](#Типы-параметров-в-json)**
- **[Совместимость типов](#Совместимость-типов)**
- **[Совместимость конфигураций](#Совместимость-конфигураций)**
- **[Автоматическая перезагрузка](#Автоматическая-перезагрузка)**

* * *

## Введение

Конфигурация ASF разделена на две основные части - глобальная конфигурация (для всего процесса), и конфигурация каждого бота. У каждого бота есть собственный файл конфигурации бота с именем `BotName.json` (где `BotName` это имя, которое вы дали боту), а для конфигурации процесса ASF используется один файл с именем `ASF.json`.

Бот - это один аккаунт Steam используемый ASF. Для нормальной работы ASF требует чтобы был определён хотя бы **один** бот. Жёсткого ограничения на количество ботов нет, поэтому вы можете использовать столько ботов (аккаунтов Steam) сколько захотите.

ASF использует формат **[JSON](https://ru.wikipedia.org/wiki/JSON)** для сохранения настроек в файлах конфигурации. Это дружественный к пользователю, читаемый и довольно универсальный формат, в котором вы можете конфигурировать программу. Но не волнуйтесь, вам не обязательно знать JSON чтобы настроить ASF. Я упомянул это просто на случай если вы захотите массово создавать конфигурации ASF каким-нибудь скриптом.

Конфигурирование может быть проделано вручную - путём создания соответствующих файлов конфигруации в формате JSON, или с использованием нашего **[сетевого генератора конфигураций](https://justarchi.github.io/ArchiSteamFarm)**, что будет намного легче и удобнее. Если вы не продвинутый пользователь, я рекомендую вам использовать генератор конфигураций, описание которого приведено ниже.

**[Вернуться к началу](#Конфигурация)**

* * *

## Сетевой генератор конфигураций

Цель сетевого генератора конфигураций - предоставить вам удобный интерфейс для создания конфигурационных файлов ASF. Сетевой генератор конфигураций работает на 100% на стороне клиента, это значит что никакие данные, которые вы в нём вводите, никуда не пересылаются, и обрабатываются только на вашей машине. Это обеспечивает безопасность и надёжность, поскольку он может работать даже **[оффлайн](https://github.com/JustArchi/ArchiSteamFarm/tree/master/docs)** если вы скачаете все файлы и запустите `index.html` в своём любимом браузере.

Сетевой генератор конфигураций проверен в Chrome и Firefox, но он должен работать во всех популярных браузерах с включенным javascript.

Пользоваться им очень просто - выбираете, хотите ли вы создать файл конфигурации `ASF` или `Бот` выбрав соответствующую закладку, проверяете что выбранная в генераторе версия соответствует вашей версии ASF, вводите все необходимые поля и нажимаете кнопку "Скачать". Скачанный файл кладёте в папку `config` вашего ASF, при необходимости перезаписывая существующий. Повторяете всю процедуру в случае будущих модификаций, и используете остаток этой статьи для объяснения всех доступных параметров конфигурации.

**[Вернуться к началу](#Конфигурация)**

* * *

## Ручная настройка

Я настоятельно рекомендую использовать сетевой генератор конфигураций, но если по какой-то причине вы не хотите - вы можете создавать конфигурационные файлы сами. Ознакомьтесь с файлом `example.json` в качестве примера правильной структуры, вы можете скопировать этот файл и использовать как базу для вашего нового бота. Поскольку вы не пользуетесь программой, убедитесь что ваш файл **[валидный с точки зрения JSON](https://jsonlint.com)**, поскольку ASF не сможет его загрузить если его не удаётся разобрать. Чтобы узнать правильную структуру JSON для всех полей, ознакомьтесь с разделом "**[Типы параметров в JSON](#Типы-параметров-в-json)**" и приведенной ниже документацией.

**[Вернуться к началу](#Конфигурация)**

* * *

## Файл глобальной конфигурации

Файл глобальной конфигурации называется `ASF.json` и имеет следующую структуру:

```json
{
    "AutoRestart": true,
    "Blacklist": [],
    "CommandPrefix": "!",
    "ConfirmationsLimiterDelay": 10,
    "ConnectionTimeout": 60,
    "CurrentCulture": null,
    "Debug": false,
    "FarmingDelay": 15,
    "GiftsLimiterDelay": 1,
    "Headless": false,
    "IdleFarmingPeriod": 8,
    "InventoryLimiterDelay": 3,
    "IPC": false,
    "IPCPassword": null,
    "LoginLimiterDelay": 10,
    "MaxFarmingTime": 10,
    "MaxTradeHoldDuration": 15,
    "OptimizationMode": 0,
    "Statistics": true,
    "SteamMessagePrefix": "/me ",
    "SteamOwnerID": 0,
    "SteamProtocols": 5,
    "UpdateChannel": 1,
    "UpdatePeriod": 24,
    "WebLimiterDelay": 200,
    "WebProxy": null,
    "WebProxyPassword": null,
    "WebProxyUsername": null
}
```

**Совет:** Если вы не собираетесь менять этих настроек, вы можете просто оставить всё по-умолчанию, а значит вы можете закрыть `ASF.json` и перейти к конфигурированию бота.

* * *

Все параметры описаны ниже:

`AutoRestart` - параметр типа `bool` со значением по-умолчанию `true`. Этот параметр определяет, разрешено ли ASF перезапускаться при необходимости. Существуют события, которые могут потребовать перезапуска ASF, такие как обновление (раз в `UpdatePeriod` или по команде `update`), редактирование файла `ASF.json`, команда `restart` и тому подобное. Перезапуск обычно включает в себя две части - создание нового процесса и завершение текущего. Для большинства пользователей подойдёт оставить этому параметро значение по-умолчанию `true`, однако если вы запускаете ASF через собственный скрипт и/или `dotnet`, возможно вы захотите иметь полный контроль над запуском процесса, и избежать ситуации когда новый (перезапущеный) процесс ASF работает где-то в фоне, а не в основном процессе скрипта, который завершится вместе со старым процессом ASF. Особенно важно то, что новый процесс не будет прямым потомком вашего скрипта, из-за чего вы не сможете например использовать его стандартный поток вывода.

В этом случае вам поможет этот параметр, вы можете установить его в `false`. Однако помните, что в этом случае **вы** несёте ответственность за перезапуск процесса. Это особенно важно поскольку в этом случае ASF просто выйдет вместо создания нового процесса (например, после обновления), поэтому если вы не добавили свою логику для этого случая он просто перестанет работать пока вы его не запустите снова. ASF всегда выходит с правильным кодом выхода, указывающим на удачу (ноль) или неудачу (не ноль), поэтому вы можете добавить правильную логику в своём скрипте чтобы избежать перезапуска ASF в случае отказа, или как минимум создавать резервную копию `log.txt` для дальнейшего анализа. Также помните, что команда `restart` выполнит перезапуск ASF независимо от значения этого параметра, поскольку этот параметр задаёт поведение по-умолчанию, а команда `restart` производит принудительный перезапуск процесса. Если у вас нет причин отключать эту функцию, рекомендуется оставить её включенной.

* * *

`Blacklist` - параметр типа `ImmutableHashSet<uint>` с пустым значением по-умолчанию. Как подсказывает название, этот глобальный параметр определяет какие appID (игры) будут полностью игнорироваться в автоматическом режиме фарма ASF. К сожалению Steam любит отмечать значки летней/зимней распрожажи как "доступные для выпадения карточек", из-за чего ASF пытается их фармить как будто это настоящая игра. Если бы не было черного списка, ASF просто "зависал" бы пытаясь фармить эти игры, которые на самом деле не игры, и бесконечно бы ждал пока из них выпадут карточки, чего никогда не произойдёт. Черный список ASF позволяет отметить эти значки как недоступные для фарма, что позволяет просто тихо игнорировать их при определении следующей игры для фарма, и избегать таких ловушек.

По умолчанию ASF включает в себя два черных списка - `GlobalBlacklist`, который жёстко записан в ASF при программировании, и обычный `Blacklist`, который здесь описан. `GlobalBlacklist` обновляется вместе с ASF и обычно включает в себя все "плохие" appID на момент релиза, поэтому если вы пользуетесь последней версией вам не нужно беспокоиться о поддержании своего `Blacklist`, описанного здесь. Основная цель этого параметра - позволить вам заносить в черный список новые, не известные на момент релиза ASF appID, которые не следует фармить. Записанный в код `GlobalBlacklist` обновляется как можно быстрее, поэтому нет необходимости обновлять `Blacklist` если вы пользуетесь последней версие ASF, но без этого параметра вы были бы вынуждены обновлять ASF чтобы поддерживать его "работоспособным" когда Valve выпускает новый значок распродажи - а я не хочу заставлять вас пользоваться последней версие ASF, и поэтому этот параметр позволяет вам "исправить" ASF самостоятельно, если по какой-то причине вы не хотите или не можете обновить `GlobalBlacklist` вместе с новым релизом, но хотите чтобы ASF продолжил работать. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

Если вы ищете черный список, персональный для каждого бота, посмотрите на **[команды](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `ib`, `ibadd` and `ibrm`.

* * *

`CommandPrefix` - параметр типа `string` со значением по-умолчанию `!`. Этот параметр задаёт **чувствительный к регистру** префикс используемый для **[команд](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** ASF. Другими словами, это то, с чего должна начинаться каждая команда ASF чтобы ASF вас послушался. Этому параметру можно присвоить значение `null` или пустую строку, чтобы заставить ASF не использовать префикс, в этом случае команды вводятся просто своими идентификаторами. Однако, это может привести к снижению производительности ASF, поскольку ASF оптимизирован пропускать команды, если они не начинаются с `CommandPrefix` - если вы намеренно решите не пользоваться им, ASF будет вынужден читать все сообщения и отвечать на них, даже если это не команды. Поэтому рекомендуется использовать какой-нибудь `CommandPrefix`, как например `/`, если вам не нравится значение по-умолчанию `!`. Для единообразия, `CommandPrefix` влияет на весь процесс ASF. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`ConfirmationsLimiterDelay` - параметр типа `byte` со значением по-умолчанию `10`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF обеспечивает, чтобы как минимум `ConfirmationsLimiterDelay` секунд прошло между двумя последовательными запросами на подтверждение двухфакторной аутентификации (далее 2ФА) - они используются для **[2ФА ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication-ru-RU)** во время, например, команды `2faok`, а также при необходимости различных действия связанных с обменами. Значение по-умолчанию было установлено исходя из результатов наших тестов, и его не стоит снижать если вы не хотите возникновения проблем. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`ConnectionTimeout` - параметр типа `byte` со значением по-умолчанию `60`. Этот параметр определяет таймауты для различных сетевых действий, производимых ASF, в секундах. В частности, `ConnectionTimeout` определяет таймаут в секундах для HTTP и IPC запросов, `ConnectionTimeout / 10` определяет максимальное число пропущенных тактов сторожевого таймера, а `ConnectionTimeout / 30` определяет время в минутах, которое мы выделяем на начальный запрос подключения к сети Steam. Значение по умолчанию `60` должно подходить большинству пользователей, однако, если у вас очень медленное сетевое подключение или ПК, возможно вы захотите увеличить это значение (например до `90`). Имейте в виду, что большие значения не починят волшебным образом медленные или недоступные сервера Steam, поэтому нет смысла бесконечно ждать чего-то, что никогда не случится, и проще попробовать заново позже. Установка этому параметру слишком большого значения приведёт к излишним задержка обнаружения проблем с сетью, а также приведёт к общему падению производительности. Установка этому параметру слишком маленького значения также уменьшит общую стабильность и производительность, поскольку ASF будет прерывать нормальные запросы, которые ещё обрабатываются. Поэтому устанавливать этому параметру значение меньше значения по-умолчанию в целом не даёт преимуществ, поскольку сервера Steam время от времени работают медленно, и поэтому требуют больше времени для обработки запросов ASF. Значение по-умолчанию - это попытка уравновесить предположение о стабильности сетевого подключения и сомнения в способности сети Steam обработать запрос за отведенное время. Если вы хотите обнаруживать проблемы с сетью как можно раньше и позволить ASF переподключаться/реагировать как можно быстрее, значение по умолчанию вам подойдёт (можно сделать его немножко ниже, чтобы ASF стал менее терпеливым). Если же вы замечаете, что у ASF часто происходят проблемы с сетью, такие как ошибки обменов, потери тактов сторожевого таймера, или разрывы связи со Steam, возможно имеет смысл увеличить это значение если вы уверены что это связано с самим Steam, а **не** с вашей сетью, поскольку увеличение таймаутов делает ASF более "терпеливым" и не пытаться сразу же переподключаться. Возможно также имеет смысл увеличить это значение если у вас довольно медленный интернет, который требует больше времени для передачи данных. Вкратце, значение по-умолчанию подходит для большинства случаев, но вы можете захотеть увеличить его при необходимости. Но значительно увеличивать относительно значения по-умолчанию также не имеет много смысла, поскольку большие таймауты не починят волшебным образом недоступные сервера Steam. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`CurrentCulture` - параметр типа `string` со значением по-умолчанию `null`. По-умолчанию ASF пытается использовать язык вашей операционной системы, и использовать переведенные на этот язык строки, если они доступны. Это возможно благодаря нашему сообществу, которое старается **[локализовать](https://github.com/JustArchi/ArchiSteamFarm/wiki/Localization-ru-RU)** ASF на большинство популярных языков. Если по какой-то причине вы не хотите использовать язык вашей ОС, вы можете использовать этот параметр чтобы выбрать любой существующий язык, который бы вы хотели использовать. Полный список языков вы можете найти в **[MSDN](https://msdn.microsoft.com/en-us/library/cc233982.aspx)** в колонке `Language tag`. Рады сообщить, что ASF воспринимает как точные указания на языки, как например `en-GB`, так и нейтральные, такие как `en`. Указание языка может также влиять на другие региональные настройки, такие как формат валюты, даты и тому подобное. Обратите внимание, что вам могут понадобиться дополнительные шрифты/языковые пакеты для правильного отображения символов некоторых языков, если вы выбрали язык отличный от языка вашей системы. Обычно вам может понадобиться этот параметр если вы хотите пользоваться ASF на английском языке вместо своего родного.

* * *

`Debug` - параметр типа `bool` со значением по-умолчанию `false`. Этот параметр определяет, должен ли ASF работать в режиме отладки. В режиме отладки, ASF использует специальную папку `debug` рядом с папкой `config`, в которой сохраняются все обмены между ASF и серверами Steam. Отладочная информация может помочь обнаруживать сложные проблемы, связанные с сетью и общей работой ASF. В дополнение к этому, некоторые подпрограммы будут выводить гораздо больше информации, так например `WebBrowser` будет указывать конкретную причину, почему запросы завершаются неудачно - такая информация записывается в обычный журнал ASF. **Вам не следует использовать ASF в режиме отладки, если вас об этом не попросил разработчик**. Работа в отладочном режиме **уменьшает производительность**, **отрицательно влияет на стабильность** и **выводит гораздо больше информации в разных местах**, поэтому этот режим должен использоваться **исключительно** намеренно, кратковременно, для отладки конкретной проблемы, воспроизведения проблемы или получения дополнительной информации и сбое обмена, и тому подобное, но **не** для нормальной работы программы. Вы увидите **огромное количество** новых ошибок, проблем, и исключений - подумайте, обладаете ли вы достаточными знаниями об ASF, Steam и его глюках прежде чем вы решите анализировать эти данные самостоятельно, поскольку часть этих сообщений может не соответствовать истине.

**ПРЕДУПРЕЖДЕНИЕ:** активация этого режима включает журналирование **потенциально конфиденциальной** информации, такой как логины и пароли, используемые для входа в Steam (из-за журналирования сетевых запросов). Эти данные записываются как в папку `debug`, так и в обычный `log.txt` (который в этом режиме будет гораздо более подробным). Вам не следует выкладывать отладочную информацию, сгенерированную ASF, в публичных местах, разработчики ASF всегда будут просить передать её на e-mail, или выложить в другое безопасное место. Мы не сохраняем и не используем эти персональные данные, они записываются как часть процедуры отладки, поскольку их присутствие может быть связано с имеющейся у вас проблемой. Мы бы предпочли чтобы вы не изменяли журнал ASF, но если вы беспокоитесь, вы можете соответственно отредактировать свои персональные данные.

> Редактирование включает в себя замену персональных данных, например на звездочки. Вам стоит избегать полного удаления строк, содержащих персональные данные, поскольку само их наличие может быть важным, и поэтому их надо сохранить.

* * *

`FarmingDelay` - параметр типа `byte` со значением по-умолчанию `15`. Для работы ASF требуется периодически проверять каждые `FarmingDelay` минут, не выпали ли уже все карточки из текущей игры. Слишком маленькое значение в этом параметру приведёт к избыточному количеству запросов к steam, а слишком большое может привести к тому что ASF будет "фармить игру на `FarmingDelay` дольше чем требуется. Значение по-умолчанию должно прекрасно подойти большинству пользователей, однако если у вас много ботов вы можете захотеть увеличить это значение до чего-то вроде `30` минут чтобы ограничить количество запросов к сети Steam. К счастью, ASF использует механизм обработки событий и проверяет страницу значков в Steam каждый раз когда появляется уведомление о новых предметов, поэтому в общем случае **нам вообще не нужно проверять это через фиксированные промежутки времени**, но поскольку мы не полностью доверяем сети Steam - мы всё же проверяем страницу со значками, чтобы убедиться за за последние `FarmingDelay` минут не выпало карточек (на случай если Steam не пришлёт уведомление о новый предметах, или что-то подобное). Если предположить что сесть Steam работает правильно, уменьшение этого значения **никак не повлияет на эффективность фарма**, но **существенно увеличит нагрузку на сеть** - поэтому рекомендуется только увеличивать его (при необходимости) относительно значения по-умолчанию в `15` минут. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`GiftsLimiterDelay` - параметр типа `byte` со значением по-умолчанию `1`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF обеспечивает чтобы между последовательными запросами на обработку(активацию) подарков/ключей/лицензий прошло как минимум `GiftsLimiterDelay` секунд. В дополнение к этому этот параметр также будет использоваться как глобальное ограничение на получение списка игр, как например по **[команде](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `owns`. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`Headless` - параметр типа `bool` со значением по-умолчанию `false`. Этот параметр определяет, должен ли ASF работать в безынтерфейсном режиме. В безынтерфейсном режиме предполагается что ASF запущен на сервере или в другой не-интерактивной среде, и поэтому запросы на чтение данных из консоли (таких как коды аутентификации, коды SteamGuard, пароли и тому подобное) не будут выдаваться. Это аналогично переводу консоли ASF в режим "только для чтения". Безынтерфейсный режим полезен в основном для пользователей, запускающих ASF на серверах, поскольку вместо запроса на, например, код аутентификации, ASF будет просто прерывать операцию и останавливать "проблемный" аккаунт. Если вы не пользуетесь ASF на сервере, и не убедились что ASF сможет работать без ввода дополнительных данных, вам стоит оставить этот параметр выключенным. В безынтерфейсном режиме не будет работать никакое взаимодействие с пользователем, и бот просто не запустится если для его работы нужен **любой** ввод значений. Это полезно на серверах, чтобы ASF просто прекращал попытки входа в аккаунт если для этого нужны дополнительные данные, а не ждал (бесконечно) пока пользователь их введёт. Также, если этот параметр включен, вы сможете пользоваться **[командой](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands-ru-RU)** `input`, которая служит заменой стандартного ввода с консоли. Если вы не уверены как настроить этот параметр - оставьте ему значение по-умолчанию `false`.

Если вы используете ASF на сервере, возможно вы захотите использовать этот параметр совместно с **[аргументом командной строки](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments-ru-RU)** `--process-required`.

* * *

`IdleFarmingPeriod` - параметр типа `byte` со значением по-умолчанию `8`. Когда ASF нечего фармить, он будет проверять каждые `IdleFarmingPeriod` часов, не появилось ли на аккаунте новых игр для фарма. Эта функция не нужна когда мы говорим о добавлении новых игр на аккаунт, поскольку алгоритм ASF достаточно умен чтобы автоматически проверять в этом случае страницу со значками. `IdleFarmingPeriod` в основном для тех случаев, когда в старую игру, которае есть у нас на аккаунте, добавили коллекционные карточки. В этом случае нет никакого события, поэтому ASF приходится периодически проверять страницу со значками чтобы предусмотреть такой вариант. Значение `0` выключает эту функцию. Смотрите также: `ShutdownOnFarmingFinished`.

* * *

`InventoryLimiterDelay` - параметр типа `byte` со значением по-умолчанию `3`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF обеспечивает чтобы между двумя последовательными запросами к инвентарю (они используются только чтобы получить список предметов в вашем инвентаре) прошло как минимум `InventoryLimiterDelay` секунд. Значение по-умолчанию `3` основано на испытаниях проведенных со 100 ботами, и должно быть достаточным для большинства (если не всех) пользователей. Однако вы можете захотеть уменьшить это значение, или даже сделать его равным `0` если у вас очень небольшое количество ботов, в этом случае ASF будет игнорировать задержку и опрашивать инвентари гораздо быстрее. Но помните, слишком низкое значение **приведёт** к временному бано по IP, и тогда вы вообще не сможете получить доступ к инвентарю. Вам может также понадобиться увеличить это значение если у вас много ботов который производят много запросов к инвентарю, хотя возможно в этом случае вам стоит вместо этого уменьшить количество запросов. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`IPC` - параметр типа `bool` со значением по-умолчанию `false`. Этот параметр определяет, должен ли **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC-ru-RU)** сервер ASF запускаться при запуске программы. IPC allows for inter-process communication by booting a local HTTP server. Если вы не собираетесь использовать IPC сервер ASF, нет смысла включать этот параметр.

* * *

`IPCPassword` - параметр типа `string` со значением по-умолчанию `null`. Этот параметр задаёт обязательный пароль для любого запроса через IPC и служит дополнительной мерой безопасности. Когда этому параметру присвоено не пустое значение, все IPC запросы будут требовать дополнительный параметр `password` со значением заданным тут. Значение по-умолчанию `null` снимает необходимость в пароле, позволяя ASF обрабатывать все запросы. В дополнение к этому, включение этого параметра также включает встроенный в IPC механизм предотвращения перебора, который будет временно банить любой `IP` -адрес посылающий слишком много неавторизованных запросов за короткий интервал времени. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`LoginLimiterDelay` - `byte` type with default value of `10`. Сеть Steam в общем случае включает в себя различные ограничения на частоту одинаковых запросов, поэтому нам приходится добавлять дополнительные задержки, чтобы не сработали эти ограничения, поскольку такое срабатывание сделает сервисы Steam временно недоступными. ASF will ensure that there will be at least `LoginLimiterDelay` seconds in between of two consecutive connection attempts. Default value of `10` was set based on connecting over 100 bot instances, and should satisfy most (if not all) of the users. You may however want to increase/decrease it, or even change to `0` if you have very low amount of bots, so ASF will ignore the delay and connect to Steam much faster. Be warned though, as setting it too low while having too many bots **will** result in Steam temporarily banning your IP, and that will prevent you from logging in **at all**, with `InvalidPassword/RateLimitExceeded` error - and that also includes your normal Steam client, not only ASF. Likewise, if you're running excessive number of bots, especially together with other Steam clients/tools using the same IP address, most likely you'll need to increase this value in order to spread logins across longer period of time.

As a side note, this value is also used as load-balancing buffer in all ASF-scheduled actions, such as trades in `SendTradePeriod`. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`MaxFarmingTime` - `byte` type with default value of `10`. As you should know, Steam is not always working properly, sometimes weird situations can happen such as steam not being recording our playtime, despite of in fact playing a game. ASF will allow farming a single game in solo mode for maximum of `MaxFarmingTime` hours, and consider it fully farmed after that period. This is required to not freeze farming process in case of weird situations happening, but also if for some reason Steam released a new badge that would stop ASF from progressing further (see: `Blacklist`). Default value of `10` hours should be enough for dropping all steam cards from one game. Setting this property too low can result in valid games being skipped (and yes, there are valid games taking even up to 9 hours to farm), while setting it too high can result in farming process being frozen. Please note that this property affects only a single game in a single farming session (so after going through entire queue ASF will return to that title), also it's not based on total playtime but internal ASF farming time. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`MaxTradeHoldDuration` - `byte` type with default value of `15`. This property defines maximum duration of trade hold in days that we're willing to accept - ASF will reject trades that are being held for more than `MaxTradeHoldDuration` days. This option makes sense only for bots with `TradingPreferences` of `SteamTradeMatcher`, as it doesn't affect `Master`/`SteamOwnerID` trades, neither donations. Trade holds are annoying for everyone, and nobody really wants to deal with them. ASF is supposed to work on liberal rules and help everyone, regardless if on trade hold or not - that's why this option is set to `15` by default. However, if you'd instead prefer to reject all trades affected by trade holds, you can specify `0` here. Please consider the fact that cards with short lifespan are not affected by this option and automatically rejected for people with trade holds, as described in **[trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section, so there is no need to globally reject everybody only because of that. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`OptimizationMode` - `byte` type with default value of `0`. This property defines optimization mode which ASF will prefer during runtime. Currently ASF supports two modes - `0` which is called `MaxPerformance`, and `1` which is called `MinMemoryUsage`. By default ASF prefers to run as many things in parallel (concurrently) as possible, which enhances performance by load-balancing work across all CPU cores, multiple CPU threads, multiple sockets and multiple threadpool tasks. For example, ASF will ask for your first badge page when checking for games to idle, and then once request arrived, ASF will read from it how many badge pages you actually have, then request each other one concurrently. This is what you should want **almost always**, as the overhead in most cases is minimal and benefits from asynchronous ASF code can be seen even on the oldest hardware with a single CPU core and heavily limited power. However, with many tasks being processed in parallel, ASF runtime is responsible for their maintenance, e.g. keeping sockets open, threads alive and tasks being processed, which can result in increased memory usage from time to time, and if you're extremely constrained by available memory, you might want to switch this property to `1` (`MinMemoryUsage`) in order to force ASF into using as little tasks as possible, and typically running possible-to-parallel asynchronous code in a synchronous manner. You should consider switching this property only if you previously read **[low-memory setup](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)** and you intentionally want to sacrifice gigantic performance boost, for a very small memory overhead decrease. Usually this option is **much worse** than what you can achieve with other possible ways, such as by limiting your ASF usage or tuning runtime's garbage collector, as explained in **[low-memory setup](https://github.com/JustArchi/ArchiSteamFarm/wiki/Low-memory-setup)**. Therefore, you should use `MinMemoryUsage` as a **last resort**, right before runtime recompilation, if you couldn't achieve satisfying results with other (much better) options. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`Statistics` - `bool` type with default value of `true`. This property defines if ASF should have statistics enabled. Detailed explanation what exactly this option does is available in **[statistics](https://github.com/JustArchi/ArchiSteamFarm/wiki/Statistics)** section. Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`SteamMessagePrefix` - `string` type with default value of `"/me "`. This property defines a prefix that will be prepended to all Steam messages being sent by ASF. By default ASF uses `"/me "` prefix in order to distinguish bot messages more easily by showing them in different color on Steam chat. Another worthy mention is `"/pre "` prefix which achieves similar result, but uses different formatting. You can also set this property to empty string or `null` in order to disable using prefix entirely and output all ASF messages in a traditional way. It's nice to note that this property affects Steam messages only - responses returned through other channels (such as IPC) are not affected. Unless you want to customize standard ASF behaviour, it's a good idea to leave it at default.

* * *

`SteamOwnerID` - `ulong` type with default value of `0`. This property defines Steam ID in 64-bit form of ASF process owner, and is very similar to `Master` permission of given bot instance (but global instead). You almost always want to set this property to ID of your own main Steam account. `Master` permission includes full control over his bot instance, but global commands such as `exit`, `restart` or `update` are reserved for `SteamOwnerID` only. This is convenient, as you might want to run bots for your friends, while not allowing them to control ASF process, such as exiting it via `exit` command. Default value of `0` specifies that there is no owner of ASF process, which means that nobody will be able to issue global ASF commands. Keep in mind that **[IPC](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC)** works with `SteamOwnerID`, so if you want to use it you must provide a valid value here.

* * *

`SteamProtocols` - `byte flags` type with default value of `5`. This property defines Steam protocols that ASF will use when connecting to Steam servers, which are defined as below:

| Значение | Имя       | Описание                                                                                         |
| -------- | --------- | ------------------------------------------------------------------------------------------------ |
| 0        | None      | Отсутствие протокола                                                                             |
| 1        | TCP       | **[Transmission Control Protocol](https://ru.wikipedia.org/wiki/Transmission_Control_Protocol)** |
| 2        | UDP       | **[User Datagram Protocol](https://ru.wikipedia.org/wiki/UDP)**                                  |
| 4        | WebSocket | **[WebSocket](https://ru.wikipedia.org/wiki/WebSocket)**                                         |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option, and that option is invalid by itself.

By default ASF should use all available Steam protocols as a measure for fighting with downtimes and other similar Steam issues. Typically you want to change this property if you want to limit ASF into using only one or two specific protocols instead of all available ones. Such measure could be needed if you're e.g. enabling only TCP traffic on your firewall and you do not want ASF to try connecting via UDP. However, unless you're debugging particular problem or issue, you almost always want to ensure that ASF is free to use any protocol that is currently supported and not just one or two. Если у вас нет **веских** причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

Right now, default settings do not include UDP protocol due to issue **[#882](https://github.com/JustArchi/ArchiSteamFarm/issues/882)**.

* * *

`UpdateChannel` - `byte` type with default value of `1`. This property defines update channel which is being used, either for auto-updates (if `UpdatePeriod` is greater than `0`), or update notifications (otherwise). Currently ASF supports three update channels - `0` which is called `None`, `1`, which is called `Stable`, and `2`, which is called `Experimental`. `Stable` channel is the default release channel, which should be used by majority of users. `Experimental` channel in addition to `Stable` releases, also includes **pre-releases** dedicated for advanced users and other developers in order to test new features, confirm bugfixes or give feedback about planned enhancements. **Experimental versions often contain unpatched bugs, work-in-progress features or rewritten implementations**. If you don't consider yourself advanced user, please stay with default `1` (Stable) update channel. `Experimental` channel is dedicated to users who know how to report bugs, deal with issues and give feedback - no technical support will be given. Check out ASF **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)** if you'd like to learn more. You can also set `UpdateChannel` to `0` (`None`), if you want to completely remove all version checks. Setting `UpdateChannel` to `0` will entirely disable entire functionality related to updates, including `update` command. Using `None` channel is **strongly discouraged** due to exposing yourself to all sort of problems (mentioned in `UpdatePeriod` description below).

**Unless you know what you're doing**, we **strongly** recommend to keep it at default.

* * *

`UpdatePeriod` - `byte` type with default value of `24`. This property defines how often ASF should check for auto-updates. Updates are crucial not only to receive new features and critical security patches, but also to receive bugfixes, performance enhancements, stability improvements and more. When a value greater than `0` is set, ASF will automatically download, replace, and restart itself (if `AutoRestart` permits) when new update is available. In order to achieve this, ASF will check every `UpdatePeriod` hours if new update is available on our GitHub repo. A value of `0` disables auto-updates, but still allows you to execute `update` command manually. You might also be interested in setting appropriate `UpdateChannel` that `UpdatePeriod` should follow.

Update process of ASF involves update of entire folder structure that ASF is using, but without touching your own configs or databases located in `config` directory - this means that any extra files unrelated to ASF in its directory might be lost during update. Default value of `24` is a good balance between unnecessary checks, and ASF that is fresh enough.

Unless you have a **strong** reason to disable this feature, you should keep auto-updates enabled within reasonable `UpdatePeriod` **for your own good**. This is not only because we don't support anything but latest stable ASF release, but also because **we give our security guarantee only for latest version**. If you're using outdated ASF version then you're intentionally exposing yourself to all kind of issues, from small bugs, through broken functionality, ending with **[permanent Steam account suspensions](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#did-somebody-get-banned-for-it)**, so we **strongly recommend**, for your own good, to always ensure that your ASF version is up to date. Auto-updates allow us to react quickly to all kind of issues by disabling or patching problematic code before it can escalate - if you opt out of it, you lose all of our security guarantees and risk consequences from running code that could be potentially harmful, not only to Steam network, but also (by definition) to your own Steam account.

* * *

`WebLimiterDelay` - `ushort` type with default value of `200`. This property defines, in miliseconds, the minimum amount of delay between sending two consecutive requests to the same Steam web-service. Such delay is required as **[AkamaiGhost](https://www.akamai.com)** service that Steam uses internally includes rate-limiting based on global number of requests sent across given time period. In normal circumstances akamai block is rather hard to achieve, but under very heavy workloads with a huge ongoing queue of requests, it's possible to trigger it if we keep sending too many requests across too short time period.

Default value was set based on assumption that ASF is the only tool accessing Steam web-services, in particular `steamcommunity.com`, `api.steampowered.com` and `store.steampowered.com`. If you're using other tools sending requests to the same web-services then you should make sure that your tool includes similar functionality of `WebLimiterDelay` and set both to double of default value, which would be `400`. This guarantees that under no circumstance you'll exceed sending more than 1 request per 200 ms.

In general, lowering `WebLimiterDelay` under default value is **strongly discouraged** as it might lead to various IP-related blocks, some of which are possible to be permanent. Default value is good enough for running a single ASF instance on the server, as well as using ASF in normal scenario along with original Steam client. It should be correct for majority of usages, and you should only increase it (never lower it), if - apart from using ASF, you're also using another tool that might send excessive number of requests to the same web-services that ASF is making use of. In short, global number of all requests sent from a single IP to a single Steam domain should never exceed more than 1 request per 200 ms.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxy` - `string` type with default value of `null`. This property defines a web proxy address that will be used for all internal http and https requests sent by ASF's `HttpClient`, especially to services such as `github.com`, `steamcommunity.com` and `store.steampowered.com`. Proxying ASF requests in general has no advantages, but it's exceptionally useful for bypassing various kinds of firewalls, especially the great firewall in China.

This property is defined as uri string:

> A URI string is composed of a scheme (http or https), a host, and an optional port. An example of a complete uri string is `"http://contoso.com:8080"`.

If your proxy requires user authentication, you will also need to set up `WebProxyUsername` and/or `WebProxyPassword`. If there is no such need, setting up this property alone is sufficient.

Right now ASF uses web proxy only for `http` and `https` requests, which **do not** include internal Steam network communication done within ASF's internal Steam client. There are currently no plans for supporting that, mainly due to missing **[SK2](https://github.com/SteamRE/SteamKit)** functionality. If you need/want it to happen, I'd suggest starting from there.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxyPassword` - `string` type with default value of `null`. This property defines password field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

`WebProxyUsername` - `string` type with default value of `null`. This property defines username field used in basic, digest, NTLM, and Kerberos authentication that is supported by a target `WebProxy` machine providing proxy functionality. If your proxy doesn't require user credentials, there is no need for you to input anything here. Using this option makes sense only if `WebProxy` is used as well, as it has no effect otherwise.

Если у вас нет причин редактировать этот параметр, рекомендуется оставить ему значение по-умолчанию.

* * *

**[Вернуться к началу](#Конфигурация)**

* * *

## Конфигурация бота

As you should know already, every bot should have its own config. Example bot config is included in `example.json` file, which should be used for bot configuration. Simply **copy paste** `example.json` to a new file, and remember to name it appropriately, as it will be your bot instance. You should start from configuring your **primary** account, so some good suggestions for filename is `primary.json`, `1.json` or `YourNickname.json`.

**Notice:** There are several names which are reserved and can't be used for bot configs. Those are: **ASF**, **example** and **minimal**. ASF will ignore such configuration files. ASF will also ignore configuration files starting with a dot.

After deciding how you want to name your bot, open its file, and start with configuration. You should notice following structure:

```json
{
    "AcceptGifts": false,
    "AutoSteamSaleEvent": false,
    "BotBehaviour": 0,
    "CustomGamePlayedWhileFarming": null,
    "CustomGamePlayedWhileIdle": null,
    "Enabled": false,
    "FarmingOrders": [],
    "GamesPlayedWhileIdle": [],
    "HoursUntilCardDrops": 3,
    "IdlePriorityQueueOnly": false,
    "IdleRefundableGames": true,
    "LootableTypes": [
        1,
        3,
        5
    ],
    "MatchableTypes": [
        5
    ],
    "OnlineStatus": 1,
    "PasswordFormat": 0,
    "Paused": false,
    "RedeemingPreferences": 0,
    "SendOnFarmingFinished": false,
    "SendTradePeriod": 0,
    "ShutdownOnFarmingFinished": false,
    "SteamLogin": null,
    "SteamMasterClanID": 0,
    "SteamParentalPIN": "0",
    "SteamPassword": null,
    "SteamTradeToken": null,
    "SteamUserPermissions": {},
    "TradingPreferences": 0,
    "UseLoginKeys": true
}
```

**Tip:** In order for bot to work properly, you should edit at least `Enabled`, `SteamLogin` and `SteamPassword` properties. I also suggest to take a look at some fine-tuning such as `HoursUntilCardDrops`, but all of that is optional. ASF configs are quite advanced to allow you tune your bots and ASF however you want, if you don't "require" such advanced setup, you don't really have to go deep into each config property. It's up to you how simple or how complex ASF should be.

* * *

Все параметры описаны ниже:

`AcceptGifts` - `bool` type with default value of `false`. When enabled, ASF will automatically accept and redeem all steam gifts (including wallet gift cards) sent to the bot. This includes also gifts sent from users other than those defined in `SteamUserPermissions`. Keep in mind that gifts sent to e-mail address are not directly forwarded to the client, so ASF won't accept those without your help.

This option is recommended only for alt accounts, as it's very likely that you don't want to automatically redeem all gifts sent to your primary account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

`AutoSteamSaleEvent` - `bool` type with default value of `false`. During Steam summer/winter sale events Steam is known for providing you extra cards for browsing discovery queue each day, as well as voting in the Steam awards. When this option is enabled, ASF will automatically check Steam discovery queue and Steam awards each 6 hours, and clear them if needed. This option is not recommended if you want to do those actions yourself, and typically it should make sense only on bot accounts. Moreover, you need to ensure that your account is at least of level `8` if you expect to receive those cards in the first place. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

Please note that due to constant Valve issues, changes and problems, **we give no guarantee whether this function will work properly**, therefore it's entirely possible that this option **will not work at all**. We do not accept **any** bug reports, neither support requests for this option. It's offered with absolutely no guarantees, you're using it at your own risk.

* * *

`BotBehaviour` - `byte flags` type with default value of `0`. This property defines ASF bot-like behaviour during various events, and is defined as below:

| Значение | Имя                           | Описание                                                                                   |
| -------- | ----------------------------- | ------------------------------------------------------------------------------------------ |
| 0        | None                          | Бот не имеет особых реакций, наименее агрессивный режим, используется по умолчанию         |
| 1        | RejectInvalidFriendInvites    | ASF будет отклонять (а не игнорировать) неверные запросы на добавление в друзья            |
| 2        | RejectInvalidTrades           | ASF будет отклонять (а не игнорировать) неверные предложения обмена                        |
| 4        | RejectInvalidGroupInvites     | ASF будет отклонять (а не игнорировать) неверные приглашения в группы                      |
| 8        | DismissInventoryNotifications | ASF будет автоматически отмечать все уведомления о предметах в инвентаре как просмотренные |
| 16       | MarkReceivedMessagesAsRead    | ASF будет автоматически отмечать все полученные сообщения как прочитанные                  |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

In general you want to modify this property if you expect from ASF to do certain amount of automation related to its activity, as it'd be expected from a bot account, but not a primary account used in ASF. Therefore, changing this property makes sense mainly for alt accounts, although you're free to use selected options for main accounts as well.

Normal (`None`) ASF behaviour is to only automate things that user wants (e.g. cards farming or `SteamTradeMatcher` offers, if set in `TradingPreferences`). This is the least invasive mode, and it's beneficial to majority of users since you remain in full control over your account and you can decide yourself whether to allow certain out-of-scope interactions, or not.

Invalid friend invite is an invite that doesn't come from user with `FamilySharing` permission (defined in `SteamUserPermissions`) or above. ASF in normal mode ignores those invites, as you'd expect, giving you free choice whether to accept them, or not. `RejectInvalidFriendInvites` will cause those invites to be automatically rejected, which will practically disable option for other people to add you to their friend list (as ASF will deny all such requests, apart from people defined in `SteamUserPermissions`). Unless you want to outright deny all friend invites, you shouldn't enable this option.

Invalid trade offer is an offer that isn't accepted through built-in ASF module. More on this matter can be found in **[trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section which explicitly defines what types of trade ASF is willing to accept automatically. Valid trades are also defined by other settings, especially `TradingPreferences`. `RejectInvalidTrades` will cause all invalid trade offers to be rejected, instead of being ignored. Unless you want to outright deny all trade offers that aren't automatically accepted by ASF, you shouldn't enable this option.

Invalid group invite is an invite that doesn't come from `SteamMasterClanID` group. ASF in normal mode ignores those group invites, as you'd expect, allowing you to decide yourself if you want to join particular Steam group or not. `RejectInvalidGroupInvites` will cause all those group invites to be automatically rejected, effectively making it impossible to invite you to any other group than `SteamMasterClanID`. Unless you want to outright deny all group invites, you shouldn't enable this option.

If you're unsure how to configure this option, it's best to leave it at default.

* * *

`CustomGamePlayedWhileFarming` - `string` type with default value of `null`. When ASF is farming, it can display itself as "Playing non-steam game: `CustomGamePlayedWhileFarming`" instead of currently farmed game. This can be useful if you would like to let your friends know that you're farming, yet you don't want to change default `OnlineStatus`. Please note that ASF cannot guarantee the actual display order of Steam network, therefore this is only a suggestion that may, or may not, display properly. Default value of `null` disables this feature.

* * *

`CustomGamePlayedWhileIdle` - `string` type with default value of `null`. Similar to `CustomGamePlayedWhileFarming`, but for the situation when ASF has nothing to do (as account is fully farmed). Default value of `null` disables this feature.

* * *

`Enabled` - `bool` type with default value of `false`. This property defines if bot is enabled. Enabled bot instance (`true`) will automatically start on ASF run, while disabled bot instance (`false`) will need to be started manually. By default every bot is disabled, so you probably want to switch this property to `true` for all of your bots that should be started automatically.

* * *

`FarmingOrders` - `ImmutableHashSet<byte>` type with default value of being empty. This property defines the **preferred** farming order used by ASF for given bot account. Currently there are following farming orders available:

| Значение | Имя                       | Описание                                                                                    |
| -------- | ------------------------- | ------------------------------------------------------------------------------------------- |
| 0        | Unordered                 | Без сортировки, незначительно улучшает производительность ЦПУ                               |
| 1        | AppIDsAscending           | Пытаться сначала фармить игры с самым маленьким `appID`                                     |
| 2        | AppIDsDescending          | Пытаться сначала фармить игры с самым большим `appID`                                       |
| 3        | CardDropsAscending        | Пытаться сначала фармить игры с наименьшим количеством доступных для выпадения карточек     |
| 4        | CardDropsDescending       | Пытаться сначала фармить игры с наибольшим количеством доступных для выпадения карточек     |
| 5        | HoursAscending            | Пытаться сначала фармить игры с наименьшим количеством наигранных часов                     |
| 6        | HoursDescending           | Пытаться сначала фармить игры с наибольшим количеством наигранных часов                     |
| 7        | NamesAscending            | Пытаться фармить игры в алфавитном порядке, начиная с A                                     |
| 8        | NamesDescending           | Пытаться фармить игры в обратном алфавитном порядке, начиная с Z                            |
| 9        | Random                    | Пытаться фармить игры в совершенно случайном порядке (разном для каждого запуска программы) |
| 10       | BadgeLevelsAscending      | Пытаться сначала фармить игры с самым маленьким уровнем значка                              |
| 11       | BadgeLevelsDescending     | Пытаться сначала фармить игры с самым большим уровнем значка                                |
| 12       | RedeemDateTimesAscending  | Пытаться сначала фармить игры, которые добавлены на аккаунт раньше других                   |
| 13       | RedeemDateTimesDescending | Пытаться сначала фармить игры, которые добавлены на аккаунт позже других                    |
| 14       | MarketableAscending       | Пытаться сначала фармить игры, карточки из которых нельзя продать                           |
| 15       | MarketableDescending      | Пытаться сначала фармить игры, карточки из которых можно продать                            |

Since this property is an array, it allows you to use several different settings in your fixed order. For example, you can include values of `15`, `11` and `7` in order to sort by marketable games first, then by those with highest badge level, and finally alphabetically. As you can guess, the order actually matters, as reverse one (`7`, `11` and `15`) achieves something entirely different. Majority of people will probably use just one order out of all of them, but in case you want to, you can also sort further by extra parameters.

Also notice the word "try" in all above descriptions - the actual ASF order is heavily affected by selected **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** and sorting will affect only results that ASF considers same performance-wise. For example, in `Simple` algorithm, selected `FarmingOrders` should be entirely respected in current farming session (as every game has the same performance value), while in `Complex` algorithm actual order is affected by hours first, and then sorted according to chosen `FarmingOrders`. This will lead to different results, as games with existing playtime will have a priority over others, so effectively ASF will prefer games with highest playtime first, and only then sorting everything further by your chosen `FarmingOrders`. Therefore, this config property is only a **suggestion** that ASF will try to respect, as long as it doesn't affect performance negatively (in this case, ASF will always prefer performance over `FarmingOrders`).

There is also idling priority queue that is accessible through `iq` **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If it's used, actual idling order is sorted firstly by performance, then by idling queue, and finally by your `FarmingOrders`.

* * *

`GamesPlayedWhileIdle` - `ImmutableHashSet<uint>` type with default value of being empty. If ASF has nothing to farm it can play your specified steam games (`appID`s) instead. Playing games in such manner increases your "hours played" of those games, but nothing else apart of it. This feature can be enabled at the same time with `CustomGamePlayedWhileIdle` in order to play your selected games while showing custom status in Steam network, but in this case, like in `CustomGamePlayedWhileFarming` case, the actual display order is not guaranteed. Please note that Steam allows ASF to play only up to `32` `appID`s, therefore if you put more games than that, only first `32` will be respected (and extra ones being ignored).

* * *

`HoursUntilCardDrops` - `byte` type with default value of `3`. This property defines if account has card drops restricted, and if yes, for how many initial hours. Restricted card drops means that account is not receiving any card drops from given game until the game is played for at least `HoursUntilCardDrops` hours. Unfortunately there is no magical way to detect that, so ASF relies on you. This property affects **[cards farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)** that will be used. Setting this property properly will maximize profits and minimize time required for cards to be farmed. Remember that there is no obvious answer whether you should use one or another value, since it fully depends on your account. It seems that older accounts which never asked for refund have unrestricted card drops, so they should use a value of `0`, while new accounts and those who did ask for refund have restricted card drops with a value of `3`. Но это только теория, и не стоит полагаться на это как на правило. The default value for this property was set based on "lesser evil" and majority of use cases.

* * *

`IdlePriorityQueueOnly` - `bool` type with default value of `false`. This property defines if ASF should consider for automatic idling only apps that you added yourself to priority idling queue available with `iq` **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. When this option is enabled, ASF will skip all `appIDs` that are missing on the list, effectively allowing you to cherry-pick games for automatic ASF idling. Keep in mind that if you didn't add any games to the queue then ASF will act as if there is nothing to idle on your account. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

`IdleRefundableGames` - `bool` type with default value of `true`. This property defines if ASF is permitted to idle games that are still refundable. A refundable game is a game that we bought in last 2 weeks through Steam Store and we didn't play it for longer than 2 hours yet, as stated **[here](https://store.steampowered.com/steam_refunds)**. By default when this option is set to `true`, ASF ignores Steam refunds entirely and idles everything, as most people expect. However, you can change this option to `false` if you want to ensure that ASF won't idle any of your refundable games too soon, allowing you to evaluate those games yourself and refund if needed without worrying about ASF affecting playtime negatively. Please note that if you disable this option then games you purchased from Steam Store won't be idled by ASF for up to 14 days since redeem date. If you're unsure whether you want this feature enabled or not, keep it with default value of `true`.

* * *

`LootableTypes` - `ImmutableHashSet<byte>` type with default value of `1, 3, 5` steam item types. This property defines ASF behaviour when looting - both manual and automatic. ASF will ensure that only items from `LootableTypes` will be included in a trade offer, therefore this property allows you to choose what you want to receive in a trade offer that is being sent to you.

| Значение | Имя               | Описание                                                                  |
| -------- | ----------------- | ------------------------------------------------------------------------- |
| 0        | Unknown           | Типы предметов, которые не попадают ни в одну из категорий ниже           |
| 1        | BoosterPack       | Наборы карточек                                                           |
| 2        | Emoticon          | Смайлики, используемые в чате Steam                                       |
| 3        | FoilTradingCard   | Аналог `TradingCard`, но для металлических карточек                       |
| 4        | ProfileBackground | Фоны, используемые в вашем профиле Steam                                  |
| 5        | TradingCard       | Коллекционные карточки Steam, используемые для создания значков (обычных) |
| 6        | SteamGems         | Самоцветы и мешки самоцветов, используемые для создания наборов карточек  |

Please note that regardless of the settings above, ASF will only ask for Steam (`appID` of 753) community (`contextID` of 6) items, so all game items, gifts and likewise, are excluded from the trade offer by definition.

Default ASF setting is based on most common usage of the bot, with looting only booster packs, and trading cards (including foils). The property defined here allows you to alter that behaviour in whatever way that satisfies you. Please keep in mind that all types not defined above will show as `Unknown` type, which is especially important when Valve releases some new Steam item, that will be marked as `Unknown` by ASF as well, until it's added here (in the future release). That's why in general it's not recommended to include `Unknown` type in your `LootableTypes`, unless you know what you're doing, and you also understand that ASF will send your entire inventory in a trade offer if Steam Network gets broken again and reports all your items as `Unknown`. My strong suggestion is to not include `Unknown` type in the `LootableTypes`, even if you expect to loot everything.

* * *

`MatchableTypes` - `ImmutableHashSet<byte>` type with default value of `5` Steam item types. This property defines which Steam item types are permitted to be matched when `SteamTradeMatcher` option in `TradingPreferences` is enabled. Types are defined as below:

| Значение | Имя               | Описание                                                                  |
| -------- | ----------------- | ------------------------------------------------------------------------- |
| 0        | Unknown           | Типы предметов, которые не попадают ни в одну из категорий ниже           |
| 1        | BoosterPack       | Наборы карточек                                                           |
| 2        | Emoticon          | Смайлики, используемые в чате Steam                                       |
| 3        | FoilTradingCard   | Аналог `TradingCard`, но для металлических карточек                       |
| 4        | ProfileBackground | Фоны, используемые в вашем профиле Steam                                  |
| 5        | TradingCard       | Коллекционные карточки Steam, используемые для создания значков (обычных) |
| 6        | SteamGems         | Самоцветы и мешки самоцветов, используемые для создания наборов карточек  |

Of course, types that you should use for this property typically include only `2`, `3`, `4` and `5`, as only those types are supported by STM. Please note that **ASF is not a trading bot** and **will NOT care about price or rarity**, which means that if you use it e.g. with `Emoticon` type, then ASF will be happy to trade your 2x rare emoticon for 1x rare 1x common, as that makes progress towards badge (in this case emoticons) completion. Please evaluate twice if you're fine with that. Unless you know what you're doing, you should keep it with default value of `5`.

* * *

`OnlineStatus` - `byte` type with default value of `1`. This property specifies Steam community status that the bot will be announced with after logging in to Steam network. Currently you can choose one of below statuses:

| Значение | Имя            |
| -------- | -------------- |
| 0        | Offline        |
| 1        | Online         |
| 2        | Busy           |
| 3        | Away           |
| 4        | Snooze         |
| 5        | LookingToTrade |
| 6        | LookingToPlay  |
| 7        | Invisible      |

`Offline` status is extremely useful for primary accounts. As you should know, farming a game actually shows your steam status as "Playing game: XXX", which can be misleading to your friends, confusing them that you're playing a game while actually you're only farming it. Using `Offline` status solves that issue - your account will never be shown as "in-game" when you're farming steam cards with ASF. This is possible thanks to the fact that ASF does not have to sign in into Steam Community in order to work properly, so we're in fact playing those games, connected to Steam network, but without announcing our online presence at all. Keep in mind that played games using offline status will still count towards your playtime, and show as "recently played" on your profile.

In addition to that, this feature is also important if you want to receive notifications and unread messages when ASF is running, while not keeping Steam client open at the same time. This is because ASF acts as a Steam client itself, and whether ASF would like it or not, Steam broadcasts all those messages and other events to it. This is not a problem if you have both ASF and your own Steam client running, as both clients receive exactly the same events. However, if just ASF is running, Steam network could mark certain events and messages as "delivered", despite of your traditional Steam client not receiving it due to not being present. Offline status also solves this problem, as ASF is never considered for any community events in this case, so all unread messages and other events will be properly marked as unread when you come back.

It's important to note that ASF running on `Offline` mode will **not** be able to receive commands in usual Steam chat way, as the chat, as well as entire community presence is in fact, entirely offline. A solution to this issue is using `Invisible` mode instead which works in a similar way (not exposing status), but keeps the ability to receive and respond to messages (so also a potential to dismiss notifications and unread messages as stated above). `Invisible` mode makes the most sense on alt accounts that you don't want to expose (status-wise), but still be able to send commands to.

However, there is one catch with `Invisible` mode - it doesn't go well with primary accounts. This is because **any** Steam session that is currently online **exposes** the status, even if ASF itself does not. This is caused by the current limitation/bug of the Steam network that isn't possible to be fixed on ASF side, so if you want to use `Invisible` mode you will also need to ensure that **all** other sessions to the same account use `Invisible` mode as well. This will be the case on alt accounts where ASF is hopefully the only active session, but on primary accounts you'll almost always prefer to show as `Online` to your friends, hiding only ASF activity, and in this case `Invisible` mode will be entirely useless for you (we recommend to use `Offline` mode instead). Hopefully this limitation/bug will be eventually solved in the future by Valve, but I wouldn't expect that to happen anytime soon...

If you're unsure how to set up this property, it's recommended to use a value of `0` (`Offline`) for primary accounts, and default `1` (`Online`) otherwise.

* * *

`PasswordFormat` - `byte` type with default value of `0`. This property defines the format of `SteamPassword` property, and currently supports - `0` for `PlainText`, `1` for `AES` and `2` for `ProtectedDataForCurrentUser`. Please refer to **[Security](https://github.com/JustArchi/ArchiSteamFarm/wiki/Security)** section if you want to learn more, as you'll need to ensure that `SteamPassword` property indeed includes password in matching `PasswordFormat`. In other words, when you change `PasswordFormat` then your `SteamPassword` should be **already** in that format, not just aiming to be. Unless you know what you're doing, you should keep it with default value of `0`.

* * *

`Paused` - `bool` type with default value of `false`. This property defines initial state of `CardsFarmer` module. With default value of `false`, bot will automatically start farming when it's started, either because of `Enabled` or `start` command. Switching this property to `true` should be done only if you want to manually `resume` automatic farming process, for example because you want to use `play` all the time and never use automatic `CardsFarmer` module - this works exactly the same as `pause` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**. If you're unsure whether you want this feature enabled or not, keep it with default value of `false`.

* * *

`RedeemingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when redeeming cd-keys, and is defined as below:

| Значение | Имя              | Описание                                                                                                |
| -------- | ---------------- | ------------------------------------------------------------------------------------------------------- |
| 0        | None             | Нет предпочтений по активации, по-умолчанию                                                             |
| 1        | Forwarding       | Передавать ключи, которые не удалось активировать, другим ботам                                         |
| 2        | Distributing     | Распределять все ключи между этим и другими ботами                                                      |
| 4        | KeepMissingGames | Не передавать ключи для игр, которые (возможно) отсутствуют на аккаунте, оставлять их неиспользованными |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

`Forwarding` will cause bot to forward a key that is not possible to redeem, to another connected and logged on bot that is missing that particular game (if possible to check). The most common situation is forwarding `AlreadyPurchased` game to another bot that is missing that particular game, but this option also covers other scenarios, such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`.

`Distributing` will cause bot to distribute all received keys among itself and other bots. This means that every bot will get a single key from the batch. Typically this is used only when you're redeeming many keys for the same game, and you want to evenly distribute them among your bots, as opposed to redeeming keys for various different games. This feature makes no sense if you're redeeming only one key in a single `redeem` action (as there are no extra keys to be distributed).

`KeepMissingGames` will cause bot to skip `Forwarding` when we can't be sure if key being redeemed is in fact owned by our bot, or not. This basically means that `Forwarding` will apply **only** to `AlreadyPurchased` keys, instead of covering also other cases such as `DoesNotOwnRequiredApp`, `RateLimited` or `RestrictedCountry`. Typically you might want to use this option on primary account, to ensure that keys being redeemed on it won't be forwarded further if your bot for example becomes temporarily `RateLimited`. As you can guess from the description, this field has absolutely no effect if `Forwarding` is not enabled.

Enabling both `Forwarding` and `Distributing` will add distributing feature on top of forwarding one, which makes ASF trying to redeem one key on all bots firstly (forwarding) before moving to the next one (distributing). Typically you want to use this option only when you want `Forwarding`, but with altered behaviour of switching the bot on key being used, instead of always going in-order with every key (which would be `Forwarding` alone). This behaviour can be beneficial if you know that majority or even all of your keys are tied to the same game, because in this situation `Forwarding` alone would try to redeem everything on one bot firstly (which makes sense if your keys are for unique games), and `Forwarding` + `Distributing` will switch the bot on the next key, "distributing" the task of redeeming new key onto another bot than the initial one (which makes sense if keys are for the same game, skipping one pointless attempt per key).

The actual bots order for all of the redeeming scenarios is alphabetical, excluding bots that are unavailable (not connected, stopped or likewise). Please keep in mind that there is per-IP and per-account hourly limit of redeeming tries, and every redeem try that didn't end with `OK` contributes to failed tries. ASF will do its best to minimize number of `AlreadyPurchased` failures, e.g. by trying to avoid forwarding a key to another bot that already owns that particular game, but it's not always guaranteed to work due to how Steam is handling licenses. Using redeeming flags such as `Forwarding` or `Distributing` will always increase your likelyhood to hit `RateLimited`.

Also keep in mind that you can't forward or distribute keys to bots that you do not have access to. This should be obvious, but ensure that you're at least `Operator` of all the bots you want to include in your redeeming process, for example with `status ASF` **[command](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

* * *

`SendOnFarmingFinished` - `bool` type with default value of `false`. When ASF is done with farming given account, it can automatically send steam trade containing everything farmed up to this point to user with `Master` permission, which is very convenient if you don't want to bother with trades yourself. This option works the same as `loot` command, therefore keep in mind that it requires user with `Master` permission set, you might also require valid `SteamTradeToken`, including using an account that is actually eligible for trading. In addition to initiating `loot` after finishing farming, ASF will also initiate `loot` on each new items notification (when not farming), and after completing each trade that results in new items (always) when this option is active. This is especially useful for "forwarding" items received from other people to our account. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. Если вы не уверены как настроить этот параметр - оставьте ему значение по-умолчанию `false`.

* * *

`SendTradePeriod` - `byte` type with default value of `0`. This property works very similar to `SendOnFarmingFinished` property, with one difference - instead of sending trade when farming is done, we can also send it every `SendTradePeriod` hours, regardless of how much we have to farm left. This is useful if you want to `loot` your alt accounts on usual basis instead of waiting for it to finish farming. Default value of `0` disables this feature, if you want your bot to send you trade e.g. every day, you should put `24` here. It's strongly recommended to use this feature together with **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)** being set, as there is no point in sending automatic trades if you need to manually confirm them. If you're not sure how to set this property, leave it with default value of `0`.

* * *

`ShutdownOnFarmingFinished` - `bool` type with default value of `false`. ASF is "occupying" an account for the whole time of process being active. When given account is done with farming, ASF periodically checks it (every `IdleFarmingPeriod` hours), if perhaps some new games with steam cards were added in the meantime, so it can resume farming of that account without a need to restart the process. This is useful for majority of people, as ASF can automatically resume farming when needed. However, you may actually want to stop the process when given account is fully farmed, you can achieve that by setting this property to `true`. When enabled, ASF will proceed with logging off when account is fully farmed, which means that it won't be periodically checked or occupied anymore. You should decide yourself if you prefer ASF to work on given bot instance for the whole time, or if perhaps ASF should stop it when farming process is done. When all accounts are stopped and process is not running in `--process-required` **[mode](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-Line-Arguments)**, ASF will shutdown as well. Если вы не уверены как настроить этот параметр - оставьте ему значение по-умолчанию `false`.

* * *

`SteamLogin` - `string` type with default value of `null`. This property defines your steam login - the one you use for logging in to steam. In addition to defining steam login here, you may also keep default value of `null` if you want to enter your steam login on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamMasterClanID` - `ulong` type with default value of `0`. This property defines the steamID of the steam group that bot should automatically join, including its group chat. You can check steamID of your group by navigating to its **[page](https://steamcommunity.com/groups/ascfarm)**, then adding `/memberslistxml?xml=1` to the end of the link, so the link will look like **[this](https://steamcommunity.com/groups/ascfarm/memberslistxml?xml=1)**. Then you can get steamID of your group from the result, it's in `<groupID64>` tag. In above example it would be `103582791440160998`. In addition to trying to join given group at startup, the bot will also automatically accept group invites to this group, which makes it possible for you to invite your bot manually if your group has private membership. If you don't have any group dedicated for your bots, you should keep this property with default value of `0`.

* * *

`SteamParentalPIN` - `string` type with default value of `0`. This property defines your steam parental PIN. ASF requires an access to resources protected by steam parental, therefore if you use that feature, you need to provide ASF with parental unlock PIN, so it can operate normally. Default value of `0` means that there is no steam parental PIN required to unlock this account, and this is probably what you want if you don't use steam parental functionality. In addition to defining steam parental PIN here, you may also use value of `null` if you want to enter your steam parental PIN on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamPassword` - `string` type with default value of `null`. This property defines your steam password - the one you use for logging in to steam. In addition to defining steam password here, you may also keep default value of `null` if you want to enter your steam password on each ASF startup instead of putting it in the config. This may be useful for you if you don't want to save sensitive data in config file.

* * *

`SteamTradeToken` - `string` type with default value of `null`. When you have your bot on your friend list, then bot can send a trade to you right away without worrying about trade token, therefore you can leave this property at default value of `null`. If you however decide to NOT have your bot on your friend list, then you will need to generate and fill a trade token as the user that this bot is expecting to send trades to. In other words, this property should be filled with trade token of the account that is defined with `Master` permission in `SteamUserPermissions` of **this** bot instance.

In order to find your token, as logged in user with `Master` permission, navigate **[here](https://steamcommunity.com/my/tradeoffers/privacy)** and take a look at your trade URL. The token we're looking for is made out of 8 characters after `&token=` part in your trade URL. You should copy and put those 8 characters here, as `SteamTradeToken`. Do not include whole trading URL, neither `&token=` part, only token itself.

* * *

`SteamUserPermissions` - `ImmutableDictionary<ulong, byte>` type with default value of being empty. This property is a dictionary property which maps given Steam user identified by his 64-bit steam ID, to `byte` number that specifies his permission in ASF instance. Currently available bot permissions in ASF are defined as:

| Значение | Имя           | Описание                                                                                                                                                                                                                     |
| -------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0        | None          | Никаких прав, это зарезервированное значение, которое нужно только чтобы описывать те steam ID, которые отсутствуют в этом массиве - нет необходимости задавать какой-то аккаунт с такими правами                            |
| 1        | FamilySharing | Даёт минимальный доступ для пользователей использующих семейный доступ. Опять же, это скорее зарезервированное значение, поскольку ASF способен автоматически обнаруживать каким steam ID разрешён доступ к вашей библиотеке |
| 2        | Operator      | Даёт основные права доступа к боту, в основном для активации ключей и добавления бесплатных лицензий                                                                                                                         |
| 3        | Master        | Даёт полный доступ к данному боту                                                                                                                                                                                            |

In short, this property allows you to handle permissions for given users. Permissions are important mainly for access to ASF **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**, but also for enabling many ASF features, such as accepting trades. For example you might want to set your own account as `Master`, and give `Operator` access to 2-3 of your friends so they can easily redeem keys for your bot with ASF, while **not** being eligible e.g. for stopping it. Thanks to that you can easily assign permissions to given users and let them use your bot to some specified by you degree.

We recommend to set exactly one user as `Master`, and any amount you wish as `Operator`s and below. While it's technically possible to set multiple `Master`s and ASF will work correctly with them, for example by accepting all of their trades sent to the bot, ASF will use only one of them (with lowest steam ID) for every action that requires a single target, for example a `loot` request, so also properties like `SendOnFarmingFinished` or `SendTradePeriod`. If you perfectly understand those limitations, especially the fact that `loot` request will always send items to the `Master` with lowest steam ID, regardless of the `Master` that actually executed the command, then you can define multiple users with `Master` permission here, but we still recommend a single master scheme - multiple masters scheme is discouraged setup that is not supported.

It's nice to note that there is one more extra `Owner` permission, which is declared as `SteamOwnerID` global config property. You can't assign `Owner` permission to anybody here, as `SteamUserPermissions` property defines only permissions that are related to the bot instance, and not ASF as a process.

* * *

`TradingPreferences` - `byte flags` type with default value of `0`. This property defines ASF behaviour when in trading, and is defined as below:

| Значение | Имя                 | Описание                                                                                                                                                                                                                                 |
| -------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0        | None                | Нет предпочтений при обменах - принимаются только обмены от `Master`                                                                                                                                                                     |
| 1        | AcceptDonations     | Принимаются предложения обменов, в которых бот не отдаёт никаких предметов                                                                                                                                                               |
| 2        | SteamTradeMatcher   | Принимаются предложения обменов дубликатов карточек по правилам **[STM](https://www.steamtradematcher.com)**. Ознакомьтесь с разделом "**[Обмены](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading-ru-RU)**" чтобы узнать больше |
| 4        | MatchEverything     | Требует, чтобы был задан `SteamTradeMatcher`, и в сочетании с ним - также принимаются "плохие" сделки в дополнение к "хорошим" и "нейтральным"                                                                                           |
| 8        | DontAcceptBotTrades | Не принимаются автоматически предложения обмена, отправленные по команде `loot` другими ботами                                                                                                                                           |

Please notice that this property is `flags` field, therefore it's possible to choose any combination of available values. Check out **[flags mapping](#json-mapping)** if you'd like to learn more. Not enabling any of flags results in `None` option.

For further explanation of ASF trading logic, and description of every available flag, please visit **[Trading](https://github.com/JustArchi/ArchiSteamFarm/wiki/Trading)** section.

* * *

`UseLoginKeys` - `bool` type with default value of `true`. This property defines if ASF should use login keys mechanism for this Steam account. Login keys mechanism works very similar to official Steam client's "remember me" option, which makes it possible for ASF to store and use temporary one-time use login key for next logon attempt, effectively skipping a need of providing password, Steam Guard or 2FA code as long as our login key is valid. Login key is stored in `BotName.db` file and updated automatically. This is why you don't need to provide password/SteamGuard/2FA code after logging in successfully with ASF just once.

Login keys are used by default for your convenience, so you don't need to input `SteamPassword`, SteamGuard or 2FA code (when not using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**) on each login. It's also superior alternative since login key can be used only for a single time and does not reveal your original password in any way. Exactly the same method is being used by your original Steam client, which saves your account name and login key for your next logon attempt, effectively being the same as using `SteamLogin` with `UseLoginKeys` and empty `SteamPassword` in ASF.

However, some people might be concerned even about this little detail, therefore this option is available here for you if you'd like to ensure that ASF won't store any kind of token that would allow resuming previous session after being closed, which will result in full authentication being mandatory on each login attempt. Disabling this option will work exactly the same as not checking "remember me" in official Steam client. Unless you know what you're doing, you should keep it with default value of `true`.

**[Вернуться к началу](#Конфигурация)**

* * *

## Файловая структура

ASF is using quite simple file structure.

    ├── config
    │     ├── ASF.json
    │     ├── ASF.db
    │     ├── Bot1.json
    │     ├── Bot1.db
    │     ├── Bot1.bin
    │     ├── Bot2.json
    │     ├── Bot2.db
    │     ├── Bot2.bin
    │     └── ...
    ├── ArchiSteamFarm.dll
    ├── log.txt
    └── ...
    

In order to move ASF to new location, for example another PC, it's enough to move/copy `config` directory alone, and that's the recommended way of doing any form of "ASF backups".

`log.txt` file holds the log generated by your last ASF run. This file doesn't contain any sensitive information, and is extremely useful when it comes to issues, crashes or simply as an information to you what happened in last ASF run. We will very often ask about for file if you run into issues or bugs. ASF automatically manages this file for you, but you can further tweak ASF **[logging](https://github.com/JustArchi/ArchiSteamFarm/wiki/Logging)** module if you're advanced user.

`config` directory is the place that holds configuration for ASF, including all of its bots.

`ASF.json` is a global ASF configuration file. This config is used for specifying how ASF behaves as a process, which affects all of the bots and program itself. You can find global properties there, such as ASF process owner, auto-updates or debugging.

`BotName.json` is a config of given bot instance. This config is used for specifying how given bot instance behaves, therefore those settings are specific to that bot only and not shared across other ones. This allows you to configure bots with various different settings and not necessarily all of them working in exactly the same way.

Apart from config files, ASF also uses `config` directory for storing databases.

`ASF.db` is a global ASF database file. It acts as a global persistent storage and is used for saving various information related to ASF process, such as IPs of local Steam servers. **You should not edit this file**.

`BotName.db` is a database of given bot instance. This file is used for storing crucial data about given bot instance in persistent storage, such as login keys or ASF 2FA. **You should not edit this file**.

`BotName.bin` is a special file of given bot instance, which holds information about Steam sentry hash. Sentry hash is used for authenticating using `SteamGuard` mechanism, very similar to Steam `ssfn` file. **You should not edit this file**.

`BotName.keys` is a special file that can be used for importing keys into **[background games redeemer](https://github.com/JustArchi/ArchiSteamFarm/wiki/Background-games-redeemer)**. It's not mandatory and not generated, but recognized by ASF. This file is automatically deleted after keys are successfully imported.

`BotName.maFile` is a special file that can be used for importing **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Two-factor-authentication)**. It's not mandatory and not generated, but recognized by ASF if your `BotName` does not use ASF 2FA yet. This file is automatically deleted after ASF 2FA is successfully imported.

**[Вернуться к началу](#Конфигурация)**

* * *

## Типы параметров в JSON

Every configuration property has its type. Type of the property defines values that are valid for it. You can only use values that are valid for given type - if you use invalid value, then ASF won't be able to parse your config.

**We strongly recommend to use ConfigGenerator for generating configs** - it handles most of the low-level stuff (such as types validation) for you, so you only need to input proper values, and you also don't need to understand variable types specified below. This section is mainly for people generating/editing configs manually, so they know what values they can use.

Types used by ASF are native C# types, which are specified below:

* * *

`bool` - Boolean type accepting only `true` and `false` values.

Example: `"Enabled": true`

* * *

`byte` - Unsigned byte type, accepting only integers from `0` to `255` (inclusive).

Example: `"ConnectionTimeout": 60`

* * *

`uint` - Unsigned integer type, accepting only integers from `0` to `4294967295` (inclusive).

* * *

`ulong` - Unsigned long integer type, accepting only integers from `0` to `18446744073709551615` (inclusive).

Example: `"SteamMasterClanID": 103582791440160998`

* * *

`string` - String type, accepting any sequence of characters, including empty sequence `""` and `null`. Both empty sequence as well as `null` value is treated the same by ASF, so it's up to your preference which one you want to use.

Examples: `"SteamLogin": null`, `"SteamLogin": ""`, `"SteamLogin": "MyAccountName"`

* * *

`ImmutableHashSet<valueType>` - Immutable collection (set) of unique values in given `valueType`. In JSON, it's defined as array of elements in given `valueType`.

Example for `ImmutableHashSet<uint>`: `"Blacklist": [267420, 303700, 335590]`

* * *

`ImmutableDictionary<keyType, valueType>` - Immutable dictionary (map) that maps a key specified in its `keyType`, to value specified in its `valueType`. In JSON, it's defined as an object with key-value pairs. Keep in mind that `keyType` is always quoted in this case, even if it's value type such as `ulong`.

Example for `ImmutableDictionary<ulong, byte>`: `"SteamUserPermissions": { "76561198174813138": 3, "76561198174813137": 1 }`

* * *

`flags` - Flags attribute combines several different properties into one final value by applying bitwise operations. This allows you to choose any possible combination of various different allowed values at the same time. The final value is constructed as a sum of values of all enabled options.

For example, given following values:

| Имя | Имя  |
| --- | ---- |
| 0   | None |
| 1   | A    |
| 2   | B    |
| 4   | C    |

Using `B + C` would result in value of `6`, using `A + C` would result in value of `5`, using `C` would result in value of `4` and so on. This allows you to create any possible combination of enabled values - if you decided to enable all of them, making `None + A + B + C`, you'd get value of `7`. Also notice that flag with value of `0` is enabled by definition in all other available combinations, therefore very often it's a flag that doesn't enable anything specifically (such as `None`).

So as you can see, in above example we have 3 available flags to switch on/off (`A`, `B`, `C`), and 8 possible values overall (`None -> 0`, `A -> 1`, `B -> 2`, `A+B -> 3`, `C -> 4`, `A+C -> 5`, `B+C -> 6`, `A+B+C -> 7`).

**[Вернуться к началу](#Конфигурация)**

* * *

## Совместимость типов

Due to JavaScript limitations of being unable to properly serialize simple `ulong` fields in JSON when using web-based ConfigGenerator, `ulong` fields will be rendered as strings with `s_` prefix in the resulting config. This includes for example `"SteamOwnerID": 76561198006963719` that will be written by our ConfigGenerator as `"s_SteamOwnerID": "76561198006963719"`. ASF includes proper logic for handling this string mapping automatically, so `s_` entries in your configs are actually valid and correctly generated. If you're generating configs yourself, we recommend to stick with original `ulong` fields if possible, but if you're unable to do so, you can also follow this scheme and encode them as strings with `s_` prefix added to their names. We hope to resolve this JavaScript limitation eventually.

**[Вернуться к началу](#Конфигурация)**

* * *

## Совместимость конфигураций

It's top priority for ASF to remain compatible with older configs. As you should already know, missing config properties are treated the same as they would be defined with their **default values**. Therefore, if new config property gets introduced in new version of ASF, all your configs will remain **compatible** with new version, and ASF will treat that new config property as it'd be defined with its **default value**. You can always add, remove or edit config properties according to your needs. We recommend to limit defined config properties only to those that you want to change, since this way you automatically inherit default values for all other ones, not only keeping your config clean but also increasing compatibility in case we decide to change a default value for property that you don't want to explicitly set yourself. Feel free to check `minimal.json` example configuration file that follows this concept.

**[Вернуться к началу](#Конфигурация)**

* * *

## Автоматическая перезагрузка

Starting with ASF V2.1.6.2+, the program is now aware of configs being modified "on-the-fly" - thanks to that, ASF will automatically:

- Создаёт (и при необходимости запускает) нового бота, если вы создали для него файл конфигурации
- Останавливает (при необходимости) и удаляет старого бота, если вы удалили его файл конфигурации
- Останавливает (и при необходимости запускает) любого бота, если вы изменили его файл конфигурации
- Перезапускает (при необходимости) бота с новым именем, если вы переименовали файл конфигурации

All of the above is transparent and will be done automatically without a need of restarting the program, or killing other (unaffected) bot instances.

In addition to that, ASF will also restart itself (if `AutoRestart` permits) if you modify core ASF `ASF.json` config. Likewise, program will quit if you delete or rename it.

**[Вернуться к началу](#Конфигурация)**