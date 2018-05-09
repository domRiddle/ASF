# Фоновая активация ключей

Фоновая активация ключей - встроенная в ASF функция, позволяющая ввести определённый набор ключей Steam (вместе с их именами) для активации в фоновом режиме. Это особенно полезно, если у вас большое количество ключей и вы гарантированно столкнётесь со **[статусом](https://github.com/JustArchi/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** `RateLimited`, до того, как вы активируете всю партию.

Фоновая активация ключей нацелена на использование для одного бота, что означает, что функция не использует `RedeemingPreferences`. Эта функция может быть использована вместе (или вместо) **[команды](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** `redeem`, если понадобится.

* * *

## Импорт

Импортировать возможно с помощью двух способов - используя файлы или IPC.

### Файл

ASF увидит в папке `config` файл с названием `BotName.keys`, где `BotName` - имя бота. У файла есть определённая структура в виде названия игры и ключа от неё, отделённые символом табуляции и заканчивающаяся новой строкой. Если используется несколько символов табуляции, к примеру, в названии игры, то считается только последний, при этом предыдущие считаются как часть названия игры, и конвертируются в пробелы. Например:

    POSTAL 2    ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A Week of Circus Terror POIUY-KJHGD-QWERT
    

ASF импортирует данный файл, при запуске бота или позже при выполнении. После успешного считывания вашего файла и возможного пропуска неудачных попыток,все правильно распознанные игры будут добавлены в очередь и `BotName.keys` будет удалён с `config` директории.

### IPC

Кроме использования файла с ключами, упомянутого выше, ASF так же использует `GamesToRedeemInBackground` **[API endpoint](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#post-apigamestoredeeminbackgroundbotname)**, который может быть запущен с помощью любого IPC инструмента, включая наш IPC GUI. Использование IPC может быть более мощным, так как Вы можете настроить подходящее для Вас считывание самостоятельно, например использование кастомного ограничителя вместо принудительного символа табуляции.

* * *

## Очередь

Когда игра успешно импортирована, она добавляется в очередь. ASF автоматически проходит фоновую очередь пока бот подключен к сети Steam и очередь не пуста. A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

Если во время процесса наш аккаунт получает `RateLimited` статус, очередь будет временно приостановлена на час для ожидания исчезновения кулдауна. Потом процесс продолжится с места, где он остановился, пока очередь не исчезнет.

* * *

## Пример

Предположим у Вас есть список со 100 ключей. Сначала Вам нужно создать новый `BotName.keys.new` файл в ASF директории `config`. Для того чтобы указать ASF что файл не нужно использовать сразу после создания(так как сразу после создания он пустой,пока что неготовый к использованию),мы добавляем расширение `new`.

Теперь Вы можете открыть наш новый файл и вставить список со 100 ключей, при необходимости указывая формат. После поправок наш `BotName.keys.new` файл будет в точности иметь 100(или 101,с последней новой строкой)строк,имея структуру строки `GameName\tcd-key\n`,где `\t` это символ табуляции,а `\n` - новая строка.

Теперь Вы готовы сменить имя файла с `BotName.keys.new` на `BotName.keys`,для того чтобы указать ASF что файл готов к использованию. Сразу после того как Вы это сделали, ASF начнёт автоматически импортировать файл(без перезагрузки) и потом удалит его, подтверждая то, что все наши игры были считаны и добавлены в очередь.

Вместо использования `BotName.keys` файла,вы можете использовать IPC API или даже комбинировать оба метода,если хотите.

After some time, `BotName.keys.used` and `BotName.keys.unused` files might get generated. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Примечания

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.