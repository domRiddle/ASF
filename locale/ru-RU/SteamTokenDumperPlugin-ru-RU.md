# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` это официальный **[плагин](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-ru-RU)** для ASF, доступный начиная с версии ASF V4.2.2.2, разработанный нами, который позволяет вам внести свой вклад в работу прокета **[SteamDB](https://steamdb.info)** путём передачи токенов пакетов, токенов приложений и ключей хранилища к которым имеет доступ ваша учетная запись Steam. Подробную информацию о собираемых данных, и о том почему SteamDB нуждается в них вы можете найти на странице **[Token Dumper](https://steamdb.info/tokendumper)** в SteamDB. Передаваемые данные не включают в себя никакой потенциально конфиденциальной информации, и не представляют риска для безопасности/приватности, как указано в описании выше.

---

## Активация плагина

ASF уже включает в себя плагин `SteamTokenDumperPlugin`, однако сам плагин по умолчанию отключен. Вы можете включить плагин, установив параметр `SteamTokenDumperPluginEnabled` в глобальной конфигурации ASF равным `true`, в соответствии с форматом JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

При запуске ASF плагин сообщит что он был успешно включен через стандартный механизм журналирования ASF. Вы также можете включить плагин через наш cетевой генератор конфигураций.

---

## Технические подробности

После активации, плагин будет использовать ботов, запущенных в вашем ASF, для сбора информации в виде токенов пакетов, токенов приложений и ключей хранилища к которым ваши боты имеют доступ. Модуль сбора информации включает в себя пассивные и активные процедуры, которые должны минимизировать дополнительную нагрузку, вызванную сбором данных.

Для выполнения поставленных задач, в дополнение к процедурам сбора информации, описанным выше, также инициализируется процедура передачи информации, ответственная за определение того, какие именно данные следует передавать в SteamDB на периодическом основании. Эта процедура будет запускаться примерно через `1` после запуска ASF, а затем будет повторяться каждые `24` часа. Плагин будет делать всё возможное, чтобы минимизировать количество данных, которые необходимо передать, поэтому возможно что часть данных, собранных плагином, будет оценена как бесполезная к передаче, и потому проигнорирована (например, обновление приложения, которое не меняет токен доступа).

Плагин использует базу данных постоянного хранения, расположенную в файле `config/SteamTokenDumper.cache`, она выполняет роль, аналогичную `config/ASF.db` в ASF. Этот файл используется чтобы записывать собранную и переданную информацию, и минимизировать количество работы, которую необходимо выполнить в разных запусках ASF. Удаление этого файла приведёт к тому, что процесс запускается с самого начала, чего по возможности следует избегать.

---

## Данные

ASF включает в запрос `steamID` участника, который определяется как `SteamOwnerID`, который вы установили в ASF, либо, если его нет, то как идентификатор Steam бота, у которого больше всего лицензий. Указанный участник может получить дополнительные привилегии от SteamDB за помощь в работе (как например, статус "donator" на сайте), но это остаётся полностью на усмотрение SteamDB.

В любом случае, администрация SteamDB заранее благодарит вас за вашу помощь. Переданные данные позволяют SteamDB работать, в частности, отслеживать информацию о пакетах, приложениях и хранилищах, что было бы невозможно без вашей помощи.

---

## Advanced config

Starting with ASF V5.0.7.0, our plugin supports advanced config which might come useful for people that would like to tweak the internals to their preference.

The advanced config has the following structure located within `ASF.json`:

```json
{
  "SteamTokenDumperPlugin": {
    "Enabled": false,
    "SecretAppIDs": [],
    "SecretDepotIDs": [],
    "SecretPackageIDs": [],
    "SkipAutoGrantPackages": false
  }
}
```

Все параметры описаны ниже:

### `Enabled`

Параметр типа `bool` со значением по умолчанию `false`. This property acts the same as `SteamTokenDumperPluginEnabled` root-level property explained above, and can be used instead, dedicated to people that would prefer to have entire plugin-related config in its own structure (so most likely those already using other advanced properties explained below).

---

### `SecretAppIDs`

Параметр типа `ImmutableHashSet<uint>` с пустым значением по умолчанию. This property specifies `appIDs` that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretDepotIDs`

Параметр типа `ImmutableHashSet<uint>` с пустым значением по умолчанию. This property specifies `depotIDs` that the plugin won't resolve, and if they're already resolved, won't submit the key for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SecretPackageIDs`

Параметр типа `ImmutableHashSet<uint>` с пустым значением по умолчанию. This property specifies `packageIDs` (also known as `subIDs`) that the plugin won't resolve, and if they're already resolved, won't submit the token for. This property can be useful for people with access to potentially-sensitive information about unpublished items, especially the developers, publishers or closed beta testers.

---

### `SkipAutoGrantPackages`

Параметр типа `bool` со значением по умолчанию `false`. This property acts very similar to `SecretPackageIDs` and when enabled, will cause packages with `EPaymentMethod` of `AutoGrant` to be skipped during resolve routine explained below. `AutoGrant` payment method is used by **[Steamworks](https://partner.steamgames.com)** to automatically grant packages on developer accounts. While this is not as explicit as other `Secret` options, and therefore doesn't guarantee anything (since you might have other packages than `AutoGrant` that you still don't want to submit), it should be good enough for skipping majority, if not all, of the secret packages.

---

## Further explanation

At the root level, every Steam account owns a set of packages (licenses, subscriptions), which are classified by their `packageID` (also known as `subID`). Every package may contain several apps classified by their `appID`. Every app may then include several depots classified by their `depotID`.

```text
├── sub/124923
│     ├── app/292030
│     │     ├── depot/292031
│     │     ├── depot/378648
│     │     └── ...
│     ├── app/378649
│     └── ...
└── ...
```

Our plugin includes two routines which take into account skipped items - the resolve routine and submission routine.

The resolve routine is responsible for resolving the tree you can see above. By blacklisting the packages/apps/depots in advance, you'll effectively cut the tree in the place of blacklisted branch/leaf without additional need of specifying the remaining parts of it. In our example above, if `124923` package was ignored, whether by `SecretPackageIDs` or `SkipAutoGrantPackages`, and it was the only package you own which linked to the `292030` appID, then appID `292030` wouldn't get resolved either, and by definition, if there were no other resolved apps which linked to the `292031` and `378648` depots, then they wouldn't get resolved either. However, keep in mind that if the plugin has already resolved the tree, then effectively this will only stop given item from being updated (e.g. new apps added), it will not make the plugin "forget" about the existing items that were already resolved (e.g. apps found in that package before you decided to blacklist it).

The submission routine is responsible for submitting package tokens, app tokens and depot keys of already resolved items (by the resolve routine above). Here your blacklist has immediate effect, as even if the plugin has already resolved the info, the submission routine will not actually submit it over to SteamDB if you have it blacklisted, regardless if it has been resolved or not. Keep in mind however that we're not talking about the tree anymore at this point, the submission routine does not know whether the information about the app comes from this or that package, so it only skips information about particular, blacklisted items, regardless of the relation they're in with other.

For majority of the developers and publishers, it should be enough to enable `SkipAutoGrantPackages`, potentially empowered with `SecretPackageIDs` only, as it effectively cuts the tree at the beginning branch and guarantees that the apps and depots included further will not get submitted as long as there is no other package linking to the same app. If you want to be double sure, in addition to that you can also use `SecretAppIDs`, which will skip the resolve of the app even if there are some other licenses you didn't blacklist linking to it. Using `SecretDepotIDs` should not be needed, unless you have a particular, specific need (such as skipping only a particular depot while still submitting info about packages and apps), or if you want to add yet another layer to be triple safe.