# Statistics

ASF development is supported by 3 major things: donations, users feedback, and statistics. Donations directly influence our willings to work on the project, users feedback is always nice to read (especially positive one), while statistics are providing us with the information how our software is used, and by how many people - this way we can know what to improve, as well as being able to see actual users using our application. ASF in default settings has ```Statistics``` global config property enabled. If you want to see new versions coming up, bugs being fixed, and new features getting implemented, you should consider keeping them on, so we can make use of that data in order to provide you with a better software (but not only). This is especially important because ASF is given to you **for free**, and this is the least you can do to say thanks - **tell us that you're using ASF**, because this is what our current statistics are mainly doing.

---

## Current privacy policy

When ```Statistics``` are active, following things will happen:

a) Every account being used in ASF will join our **[steam group](http://steamcommunity.com/groups/ascfarm)**. This is done for three reasons:

* It provides **you** with group announcements, especially new versions, critical issues, steam problems and other things that are important to keep community updated
* It allows **you** to use our technical support, by asking questions, resolving problems, reporting issues or suggesting improvements
* It allows us to see how many actual steam accounts are being used by ASF


b) ASF will periodically send useful data about your accounts to our **[server](https://asf.justarchi.net)**. Actual data consists of following account-related information:

* Your SteamID (in 64-bit form)
* Your Avatar (hash)
* Value specifying if you have valid ```SteamApiKey``` and you're using **[ASF 2FA](https://github.com/JustArchi/ArchiSteamFarm/wiki/Escrow)**
* Value specifying if you've enabled ```SteamTradeMatcher``` in your ```TradingPreferences```
* Value specifying if you've enabled ```MatchEverything``` in your ```TradingPreferences```

ASF will **not** gather any other non-listed-above data without prior important ([!]) notice in the changelog.

---

## Usage of data

Your SteamID, Avatar, automated trading (API key + ASF 2FA) status, SteamTradeMatcher status and MatchEverything status is being used for our **Public ASF STM listing** explained below.

---

### Public ASF STM listing

Our listing is located **[here](https://asf.justarchi.net/STM)**.

Thanks to our listing, every interested ASF and non-ASF user can easily notice bots that are currently active, and send them STM trade offer, which helps both users, **also you**, to get rid of duplicated cards and head further towards badge completion. We wanted to create something like this for a long time, as **everybody** appreciates instant response to trade offers that ASF includes, which can drastically improve efficiency of matching, as well as information about bots availability - until now it was very hard to make a public listing like this, and thanks to ASF it's much easier.

**How it exactly works:**

ASF sends initial data once after logging in, that contains all properties public listing makes use of. Then, every 5 minutes ASF sends one, very tiny "HeartBeat" request that notifies the server that bot is still up.

This allows our website to record which account can be used for matching, as well as if that account is still active. Thanks to that, our website can show all ASF 2FA+STM accounts that were active in **last 15 minutes**, appropriately with green shadow if ```MatchEverything``` is active, and with orange shadow otherwise. Users are sorted according to their last report, most recent ones being on top.

Please note that you will **not** be displayed on the website if you do not have automated trading, or SteamTradeMatcher option enabled. In this case your data is entirely private.

*The entire concept, together with website integration and ASF reporting is still in beta - it can be improved over time.*

---

## Opting out

Participating in statistics is **not mandatory**, although highly encouraged for future of the program. We do not judge you, and if you have inner urge of hiding the fact that you're ASF user (😭) then you can disable statistics **entirely** by switching ```Statistics``` global config property to ```false```. Disabled statistics make entire module non-operative, and will not do any of actions specified in our privacy policy above.

> Please note that disabling statistics might influence our technical support and other things that are offered to you (for free).