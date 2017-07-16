# Setting up

## This page applies to ASF V3 only. If you're using ASF V2, please visit **[ASF V2](https://github.com/JustArchi/ArchiSteamFarm/wiki/_Setting-up-(ASF-V2))** page instead.

If you arrived here for the first time, welcome! We're very happy to see yet another traveler that is interested in our project, although bear in mind that with great power comes great responsibility - there is a steep learning curve involved here. ASF is not just some toy program with friendly GUI, one click installer and fancy window, it's a very sophisticated and complex Steam tool that can do almost anything, as long as **you care enough** to learn how to configure and use it. As long as you care, we've got you covered with our wiki and usual wall of text. And if you don't care, just use **[Idle master](http://www.steamidlemaster.com/)** or something, we won't do homework for you.

If you're still here then it means that you endured our text above, which is nice. Unless you skipped it, then you're going to have a bad time... Anyway, ASF is a console app, which means that program itself doesn't have a friendly GUI that you're in general used to. This means that ASF is a standalone executable file that doesn't need installation, and actually works out of the box, but requires configuration prior to launching it.

***

In general, you should:
- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in appropriate format.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm` if you're on Linux/OS X)
- **[Configure ASF](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF and become a happy user.

First step is ensuring that your OS can even launch ASF properly. ASF is written in C#, based on .NET Core and might require native libraries that are not available on your platform yes. Depending on whether you use Windows, Linux or OS X, you will have different requirements, although all of them are listed in **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** that you should follow. Simply follow those instructions, there is a chance that you already have all required libraries, but you should double check.

Next step is downloading **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. ASF is available in many formats, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit c*[Compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)**.

Once you get your package and extract the zip file, you'll have a bunch of files, folders and everything. Ignore all of that for now, since it's configuration time. Visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** page and follow instructions - you should at least generate one bot config file that you put in `config` directory.

All done? Great, simply launch `ArchiSteamFarm` binary, put extra details if asked (such as your 2FA code), and have fun. Reading more about ASF while it's doing its job is a great thing to consider.

***

## Need more help?

Head over to the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)**.