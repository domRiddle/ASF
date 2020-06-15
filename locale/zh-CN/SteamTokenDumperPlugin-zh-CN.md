# Steam Token 转存插件

`SteamTokenDumperPlugin` 是由我们开发的 ASF 官方&#8203;**[插件](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-zh-CN)**，自 ASF V4.2.2.2 版本开始可用，您可以通过此插件为 **[SteamDB](https://steamdb.info)** 项目作出贡献，分享您的 Steam 帐户有权访问的 Package Token、App Token 以及 Depot Key。 关于所收集信息的进一步说明，以及为什么 SteamDB 需要这些信息，可以在 SteamDB 的 **[Token Dumper](https://steamdb.info/tokendumper)** 页面查看。 如上所述，所提交的数据不包含任何敏感信息，也不会有安全或隐私风险。

---

## 启用插件

ASF 在发布时会一并打包 `SteamTokenDumperPlugin`，但插件本身默认是禁用的。 您可以按照 JSON 格式在 ASF 全局配置内设置 `SteamTokenDumperPluginEnabled` 为 `true`：

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

在 ASF 程序启动时，此插件会通过标准的 ASF 日志机制通知您插件是否已成功启用。 您也可以通过我们的在线配置文件生成器启用此插件。

---

## 技术细节

启用后，此插件将会收集 ASF 内正在运行的机器人有权访问的 Package Token、App Token 和 Depot Key。 数据收集模块包括被动与主动例程，以尽量减少数据收集造成的额外开销。

为了符合预期的使用场景，除了上述的数据收集例程以外，还有提交例程负责确定哪些数据需要定期被提交到 SteamDB。 自 ASF 启动最多 `1` 小时后，此例程将会启动，之后则每 `24` 小时重复一次。 此插件将尽全力减少需要发送的数据量，因此，某些收集到的数据会被认为无需提交而跳过（例如 Token 未发生变化的 App 更新）。

此插件使用一个持久化缓存数据库，位于 `config/SteamDB.cache`，其用途类似 `config/ASF.db` 之于 ASF。 此文件用于记录收集到和被提交的数据，以尽可能减少下次 ASF 启动时的工作量。 删除此文件会导致此过程完全从零开始，这种情况尽可能避免。

---

## 数据

ASF 会在请求中包含贡献者的 `SteamID`，即您已在 ASF 内设置好的 `SteamOwnerID` 属性，但如果您没设置过此属性，我们就会选择拥有许可数最多的机器人的 Steam ID 来发送。 通告的贡献者可能会因为持续贡献在 SteamDB 获得一些额外的增益（例如网站上的捐赠者排名），但这完全由 SteamDB 来决定。

无论如何，SteamDB 工作人员愿意提前感谢您的帮助。 这些提交的数据使 SteamDB 能够继续运营，特别是跟踪 Package、App 与 Depot 信息，如果没有您的帮助，这些功能将不复存在。