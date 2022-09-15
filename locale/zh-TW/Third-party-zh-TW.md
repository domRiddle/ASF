# 第三方工具

本章節包含專門（或主要）與 ASF 配合使用的各種第三方工具的內容。 它們包括 ASF 外掛程式、簡單的網路應用程式、用於一體化的內部函式庫，以及適用於各種平台的全能 Bot。 若您想在此清單中新增內容，請在 Discord 或我們的 Steam 群組上聯絡我們。

請注意，以下程式**不**由 ASF 開發人員所維護，因此**我們不提供任何保證**，尤其是它們的安全性及是否符合 Steam 服務條款。 此清單僅供參考。 您應始終保證您欲使用的第三方工具對您來說足夠合規，因為您需自行承擔使用它們的風險。

---

## ASF 外掛程式

### **[Ryzhehvost](https://github.com/Ryzhehvost)**

- **[ASF-Achievement-Manager](https://github.com/Ryzhehvost/ASF-Achievement-Manager)**：您可以透過 ASF 來管理 Steam 成就。
- **[BoosterCreator](https://github.com/Ryzhehvost/BoosterCreator)**：使 ASF 具有製作擴充包的功能。
- **[Case-Insensitive-ASF](https://github.com/Ryzhehvost/Case-Insensitive-ASF)**：ASF Bot 的名稱將不再區分大小寫。
- **[Commandless-Redeem](https://github.com/Ryzhehvost/Commandless-Redeem)**：使 ASF 無需指令，即可重新實現序號啟動。
- **[ItemDispenser](https://github.com/Ryzhehvost/ItemDispenser)**：ASF 將會自動接受特定類型物品的交易請求。
- **[Selective-Loot-and-Transfer-Plugin](https://github.com/Ryzhehvost/Selective-Loot-and-Transfer-Plugin)**：使 ASF 提供進階的 `transfer` 指令來轉移 Steam 物品。

### **[Vital7](https://github.com/Vital7)**

- **[FriendAccepter](https://github.com/Vital7/FriendAccepter)**：ASF 將會自動接受所有好友邀請。
- **[GameRemover](https://github.com/Vital7/GameRemover)**：實作 ASF 指令，以刪除指定 Bot 實例的 Steam 授權條款。
- **[GetEmail](https://github.com/Vital7/GetEmail)**：實作 ASF 指令，以直接從 Steam 獲得指定 Bot 實例的電子郵件地址。
- **[ResetAPIKey](https://github.com/Vital7/ResetAPIKey)**：實作 ASF 指令，以重置指定 Bot 實例的 API 金鑰。
- **[SteamKitProxyInjection](https://github.com/Vital7/SteamKitProxyInjection)**：使 ASF 允許代理 WebSocket 連線。

### 其他

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**：加入新功能以增強 ASF，特別是指令。

---

## 整合

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**：用 Python 編寫，整合 ASF 與 Telegram 機器人。
- **[ASF STM 使用者腳本](https://greasyfork.org/zh-TW/scripts/404754-asf-stm)**：使您可以透過瀏覽器向我們的 **[ASF STM 清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#公開-asf-stm-清單)**中的 Bot 發送自動交易報價，而不必使用 ASF 提供的 `MatchActively` 功能。
- **[telegram-asf](https://github.com/deluxghost/telegram-asf)**：另一個（最小版本）用 Python 編寫，整合 ASF 與 Telegram 機器人。

---

## 函式庫

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**：用於進一步整合 ASF 的 IPC 介面的 Python 函式庫。

---

## 封裝

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**：讓您能輕鬆地在 Arch Linux 上安裝 ASF。
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**：讓您能輕鬆地在 Arch Linux 上安裝 ASF。
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**：讓您能輕鬆地在 macOS 上安裝 ASF。
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**：讓您能輕鬆地在帶有 Nix 的發行版本上安裝 ASF。
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**：讓您能輕鬆地在 NixOS 上設定並安裝 ASF。

---

## 工具

- **[Keys extractor](https://ske.xpixv.com)**：允許您複製貼上含有各種格式的序號，並使 ASF 建立 `redeem` 指令。 前往 **[GitHub repo](https://github.com/PixvIO/SKE)** 以了解更多。
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**：讓你能更方便的管理多個 ASF 設定檔。

---

## 想要發現更多嗎？

我們建議您在 GitHub 上搜尋 **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** 主題，來尋找關於 ASF 的所有專案。