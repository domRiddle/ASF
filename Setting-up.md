# Setting up

This is detailed instruction dedicated for both less advanced, and more advanced users who would like to use ASF. By reading it, you'll learn how to configure and use ASF.

**[Here](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** you can find latest stable release of ASF. Find **ASF.zip** link on the bottom to download ASF. ASF comes in zipped archive format, therefore prior to launching it you should unpack the archive, using any tool you want to, I suggest **[7-zip](http://www.7-zip.org/)**.

After unpacking archive you should notice executable file **ASF.exe**, and **config** directory, with **example.xml** config file inside. Go to the config directory and copy paste **example.xml** to new file. You should rename newly created file to anything you wish e.g. **primary.xml**, **1.xml**, or **nickname.xml**. File should have no spaces in the name. After renaming, open it using any editor you want, I suggest **[notepad++](https://notepad-plus-plus.org/)**.

Config file is using **[XML](https://en.wikipedia.org/wiki/XML)** language structure, although don't worry, you don't need to learn it, it's very easy and straightforward. In the file you can notice many so called **config properties**, which are used for setting up bot that will use that config file.

First property that you'll find is here:
```
<Enabled type="bool" value="false"/>
```

Property has 3 fields - **name**, **type** and **value**. In above example, **Enabled** is the name, **bool** is the type and **false** is a value. You as the user should edit only **value** field, and make sure that the value you use is **valid** for given **type** of the property.

Types and their valid values are included on the top of the config. For reference, I'll include some of them:
```
bool - Boolean value that can be only "true" or "false"
string - Any sequence of characters, unless stated otherwise (keep in mind escape table above), with special treatment of "null" value
byte - 8-bit unsigned integer, used mostly for small numbers (0-255)
uint - 32-bit unsigned integer, used mostly for steam appID
ulong - 64-bit unsigned (long) integer, used mostly for representing steamID64
HashSet(uint) - Comma-separated list of unique 32-bit unsigned integers
```

So as you can see, **Enabled** property above has **bool** type, so you can use only **true** and **false** values. First step is to switch Enabled property to true, so you should edit above line, so it looks like this:
```
<Enabled type="bool" value="true"/>
```

Every property is properly **documented** to explain to you what is it's purpose. Comments are dedicated to **you**, so you can read and understand the meaning of the property. Documentation is provided as a **comment** which is contained in ```<!-- -->``` tags.

Continue reading config file, and you'll notice second property to configure:
```
<!-- This is your steam login, the one you use for logging in to steam -->
<!-- You can use "null" if you wish to enter login on every startup -->
<SteamLogin type="string" value="null"/>
```

As you can read from the **comment**, this property is used for your steam login. The property is named **SteamLogin**, has type of **string** and value of **null**. You should edit value to match your steam login, as you should already know from the comment. So if you use e.g. **pablo32** steam login, you should edit above property so it looks like this:
```
<SteamLogin type="string" value="pablo32"/>
```

The last property you need to edit is **SteamPassword** property. At this point you should already know how to read and understand config properties, so I hope you can fill it properly without my helpful hand :+1:.

**Enabled**, **SteamLogin** and **SteamPassword** are 3 **required** properties. It means that after editing them, your bot is configured properly enough to allow you launch ASF and use it. However, config includes also many **other properties**, which can be configured to suit your needs. Those properties are optional, but I highly suggest to go through them, so you can customize ASF to your needs. Some of those optional properties are actually quite important to maximize performance, such as **CardDropsRestricted** property which matters for choosing optimal cards farming algorithm.

***

In any case, after you're done with configuring your config, you should start **ASF.exe** executable, by double clicking it.

If you're not using Windows OS, you should install **[latest Mono](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)** in order to be able to launch ASF. You can then start ASF by executing ```mono ASF.exe``` from terminal/shell.

***

If you did everything correctly, you should notice that ASF starts working and logs in to steam using credentials you provided in the config. If your account needs extra steps to unlock, such as **SteamGuard** or **TwoFactorAuthentication**, ASF will ask you for extra code, that you should type in the console. This has to be done only once, similar like in steam client.

Congratulations, you've just finished configuration of your first account in ASF! If you need to add another account to ASF, such as your alt, simply copy either your already configured **nickname.xml** or **example.xml** to a new file, and configure it the same way you did with your previous account. You can have as many configs as you want, ASF is coded to handle many accounts at once. Also, if you didn't yet read all available config properties, it's wise to get familiar with them, because ASF offers really many neat features that can be configured.

***

Remember that ASF doesn't require and doesn't interfere in any way with Steam client, which means that you can be logged in to Steam client as your primary account, and launch ASF at the same time, for any number of accounts, including your main one (if needed).

***

## Compatibility

During development of ASF, it's top priority to assure compatibility between older configs and new ASF, therefore **unless changelog states otherwise, old configs will work properly with new versions of ASF**. Newly introduced **config properties**, which are missing in your old config, will have their **default values**, so if I decide to add e.g. two new config properties, and you use new ASF version with your old config, it will result in the same as you'd declare those two new properties in your config with their default (unchanged) values.

***

## Minimalistic config

As you should notice, apart from **example.xml** config file, containing all currently available config properties, there is also **minimal.xml** config file, that contains only absolutely required fields.

By reading **Compatibility** part above, you should know already that missing fields will have their default values used. Therefore, you **don't need to declare all those properties in your configs**, you can instead use **minimal.xml** as your config base, and instead of **editing all properties to your needs**, you need only to **add those properties from example.xml, which you want to change**. This way your configs won't be cluttered with comments and properties which you have no interest in changing. This can be useful for advanced users having many accounts, so your configs will be short and simple, and the less properties you declare, the more compatible will be the config, because there's smaller chance that ASF will introduce **breaking change** that would require from you to rebase your configs.

So in short, you can base your bots either on **example.xml** (with editing all properties to your needs), or **minimal.xml** (with adding only those properties which you want to change). It's up to you which approach you prefer.

***

## Need more help?

Head over to the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)** :+1: 