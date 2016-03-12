# Setting up

This is detailed instruction dedicated for both less advanced, and more advanced users who would like to use ASF. By reading it, you'll learn how to configure and use ASF.

First step is obviously downloading latest stable release of ASF, which is located **[here](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. Find **ASF.zip** link on the bottom of the page to start download process. ASF comes in zipped archive format, therefore prior to launching it you should unpack the archive, using any tool you want to, I suggest **[7-zip](http://www.7-zip.org/)** or **[WinRAR](http://www.win-rar.com/download.html)**.

***

After unpacking archive you should notice executable file **ASF.exe**, and **config** directory. Prior to launching ASF you need to **[configure](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** it. Configuration is really easy process, as long as you read whole documentation carefully and follow included tips.

***

After you're done with configuration part, you should start **ASF.exe** executable, by double clicking it.

If you're using Windows OS, make sure you have **[latest .NET framework](https://www.microsoft.com/en-us/download/details.aspx?id=49981)** installed. If you're not using Windows OS, you should install **[latest Mono](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)**, then you can start ASF by executing ```mono ASF.exe``` from terminal/shell.

***

If you did everything correctly, you should notice that ASF starts working and logs in to steam using credentials you provided in the config. If your account needs extra steps to unlock, such as **SteamGuard** or **TwoFactorAuthentication**, ASF will ask you for extra code, that you should type in the console. This has to be done only once, similar like in steam client.

Congratulations, you've just finished basic steps and your ASF is functional! Remember that ASF doesn't require and doesn't interfere in any way with Steam client, which means that you can be logged in to Steam client as your primary account, and launch ASF at the same time, for any number of accounts, including your main one (if needed).

***

## Need more help?

Head over to the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)** :+1: 

### Legacy versions

For legacy ASF versions, including V0.X and V1.X, you should visit **[Setting up (Legacy)](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-(Legacy))**