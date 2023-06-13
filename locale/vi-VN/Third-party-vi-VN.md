# Bên thứ ba

Mục này chứa một vài phần mở rộng của bên thứ ba được viết dành riêng (hoặc hầu hết) dành cho việc hoạt động cùng ASF. Chúng bao gồm từ các trình cắm ASF, thông qua các ứng dụng web đơn giản và thư viện nội bộ để tích hợp, đến các bot đầy đủ tính năng cho các nền tảng khác. Nếu bạn muốn thêm thứ gì đó vào danh sách, hãy cho chúng tôi biết trên Discord hoặc trên nhóm Steam của chúng tôi.

Xin lưu ý rằng các chương trình bên dưới **không** được duy trì bởi các nhà phát triển ASF và do đó **chúng tôi không đảm bảo về bất kỳ chương trình nào**, đặc biệt là về bảo mật, an toàn hoặc tuân thủ Điều khoản Dịch vụ Steam. Danh sách này được duy trì chỉ để tham khảo. Bạn phải luôn đảm bảo rằng tiện ích của bên thứ ba mà bạn sắp sử dụng đủ an toàn và hợp pháp đối với bạn, vì bạn đang tự chịu rủi ro khi sử dụng tất cả các tiện ích đó.

---

## Phần mở rộng của ASF

### **[Ryzhehvost](https://github.com/Ryzhehvost)**

- **[ASF-Achievement-Manager](https://github.com/Ryzhehvost/ASF-Achievement-Manager)**, phần bổ trợ dành cho ASF cho phép bạn quản lý thành tựu Steam.
- **[BirthdayPlugin](https://github.com/Ryzhehvost/BirthdayPlugin)**, phần bổ trợ dành cho ASF để nhận lời chúc mừng sinh nhật.
- **[BoosterCreator](https://github.com/Ryzhehvost/BoosterCreator)**, phần bổ trợ dành cho ASF bổ sung chức năng tạo các gói bổ sung.
- **[Case-Insensitive-ASF](https://github.com/Ryzhehvost/Case-Insensitive-ASF)**, phần bổ trợ dành cho ASF để đặt tên bot không phân biệt chữ hoa chữ thường.
- **[Commandless-Redeem](https://github.com/Ryzhehvost/Commandless-Redeem)**, phần bổ trợ dành cho ASF để triển khai lại việc kích hoạt mã mà không cần lệnh.
- **[ItemDispenser](https://github.com/Ryzhehvost/ItemDispenser)**, phần bổ trợ để ASF tự động chấp nhận yêu cầu trao đổi đối với (các) loại mặt hàng nhất định.
- **[Selective-Loot-and-Transfer-Plugin](https://github.com/Ryzhehvost/Selective-Loot-and-Transfer-Plugin)**, phần bổ trợ dành cho ASF cung cấp lệnh `transfer` nâng cao để chuyển các vật phẩm trên Steam.

### **[Vita](https://github.com/ezhevita)**

- **[FriendAccepter](https://github.com/ezhevita/FriendAccepter)**, plugin for ASF to automatically accept all friend invites.
- **[GameRemover](https://github.com/ezhevita/GameRemover)**, plugin for ASF implementing a command to remove Steam licenses for selected bot instances.
- **[GetEmail](https://github.com/ezhevita/GetEmail)**, plugin for ASF implementing a command to fetch e-mail address of given bot instances directly from Steam.
- **[ResetAPIKey](https://github.com/ezhevita/ResetAPIKey)**, plugin for ASF implementing a command to reset API key for selected bot instances.
- **[SteamKitProxyInjection](https://github.com/ezhevita/SteamKitProxyInjection)**, plugin for ASF allowing proxifying WebSocket connections.

### Khác

- **[ASFEnhance](https://github.com/chr233/ASFEnhance)**, plugin for ASF enhancing it with various new features, especially commands.

---

## Integrations

- **[ASFBot](https://github.com/dmcallejo/ASFBot)**, telegram bot written in python with ASF integration.
- **[ASF STM userscript](https://greasyfork.org/en/scripts/404754-asf-stm)**, for those who want to send automated trade offers to bots on our **[ASF STM listing](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Remote-communication#public-asf-stm-listing)** through web browser, without using `MatchActively` feature provided by ASF.
- **[telegram-asf](https://github.com/deluxghost/telegram-asf)**, another (minimal) telegram bot written in python featuring ASF integration.

---

## Libraries

- **[ASF-IPC](https://github.com/deluxghost/ASF_IPC)**, python library for further integration with ASF's IPC interface.

---

## Packaging

- **[AUR repo #1](https://aur.archlinux.org/packages/asf)**, allowing you to easily install ASF on arch linux.
- **[AUR repo #2](https://aur.archlinux.org/packages/archisteamfarm-bin)**, allowing you to easily install ASF on arch linux.
- **[Homebrew](https://formulae.brew.sh/formula/archi-steam-farm)**, allowing you to easily install ASF on macOS.
- **[Nix](https://search.nixos.org/packages?channel=unstable&show=ArchiSteamFarm&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, allowing you to easily install ASF on distros with Nix.
- **[NixOS](https://search.nixos.org/options?channel=unstable&from=0&size=50&sort=relevance&type=packages&query=ArchiSteamFarm)**, allowing you to configure and install ASF with NixOS.

---

## Tools

- **[Keys extractor](https://ske.xpixv.com)**, allows you to copy-paste keys in various formats and create `redeem` command for ASF. Check **[GitHub repo](https://github.com/PixvIO/SKE)** for more details.
- **[ASF Mass Config Editor](https://github.com/genesix-eu/ASF_MCE)**, which allows to manage multiple ASF configs more easily.

---

## Want to find more?

We recommend **[ArchiSteamFarm](https://github.com/topics/archisteamfarm)** topic on GitHub for all projects that integrate with ASF.