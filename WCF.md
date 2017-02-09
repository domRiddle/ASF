# WCF

Starting with version 1.3+, ASF now offers WCF (Windows Communication Foundation) inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

WCF is always executed with ```SteamOwnerID``` permissions, which is ```0``` by default. In order to use it, you should set ```SteamOwnerID``` to the proper non-zero value. Default value will make WCF work, but not authorizing any client to execute any command. For more info about ```SteamOwnerID```, visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.

---

## Server

To start WCF, ASF must be started with ```--server``` parameter.

On Windows:
```
ASF.exe --server
```

On Linux and other Mono-powered OSes:
```
mono ASF.exe --server
```

If parameter was passed correctly, you should notice that WCF service is active:
```
INFO|ASF|StartServer() Starting WCF server on net.tcp://127.0.0.1:1242/ASF ...
INFO|ASF|StartServer() WCF server ready!
```

ASF is now listening on ```net.tcp://127.0.0.1:1242/ASF``` for incoming WCF connections (or whatever ```WCFBinding```, ```WCFHost``` and ```WCFPort``` you specified in the config).

---

## Client

To use ASF as a client, ASF must be started with ```--client``` parameter, and commands that should be executed.

On Windows:
```
ASF.exe --client "start example" "stop example"
```

On Linux and other Mono-powered OSes:
```
mono ASF.exe --client "start example" "stop example"
```

ASF expects that every command will have a structure of ```<Command> (BotName) (ExtraArgs)```. Commands don't have to be prefixed by ```!```, ASF prefixes them for you if needed (useful on Unix). ExtraArgs are optional, required by some commands (e.g. ```redeem``` one).

ASF in WCF mode supports all commands, such as ```!stop <BOT>```, ```!start <BOT>``` or ```!redeem <BOT> <KEY>```. You can view all available commands **[here](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

Remember to **quote** the command, as shown in the example above. Quotes are very important for ASF to understand what you want it to do. For example, executing:

```
ASF.exe --client "status archi"
```

Will result in intended behaviour - executing command ```!status archi```. However, executing following:

```
ASF.exe --client status archi
```

Will result in **not intended** behaviour of executing two commands - ```!status``` and ```!archi```.

If you want to execute more than one command, consider launching them at the same time (as shown in start stop example above), because launching process and the client is quite costly. ASF will execute all of those commands synchronously in given order.

Keep in mind that ASF will exit after executing last command while in ```Client``` mode.

---

## Examples

Commands:
```
mono ASF.exe --server
mono ASF.exe --client "stop archi"
```

Server results:
```
INFO|ASF|HandleCommand() Received command: "stop archi"
INFO|archi|Stop() Stopping...
INFO|ASF|HandleCommand() Answered to command: "stop archi" with: "Done!"
INFO|archi|OnDisconnected() Disconnected from Steam!
```

Client results:
```
INFO|ASF|ParseArgs() Command sent: "stop archi"
INFO|ASF|ParseArgs() Response received: "Done!"
```

---

## FAQ

**Q:** Why should I consider using WCF in ASF?

**A:** You should consider using WCF in ASF only if you have a strong reason. If you don't know what it is about, most likely you don't have one, and you can safely ignore this page along with the content. WCF provides inter-process communication, which allows you to control the ASF process during execution, e.g. through executing client via PHP script or similar. By using WCF you're able to control how ASF behaves, which might be useful if you're planning on integrating it further. If you're not running ASF on the server, and you're not planning on integrating it further with your own scripts/applications, it should be easier for you to communicate with ASF through steam chat with one of the bots. However, you can use WCF too, if you consider it useful/easier for you.

---

**Q:** Is this secure?

**A:** ASF by default listens only on ```127.0.0.1``` address, which means that accessing ASF WCF from any other machine but your own is impossible. Therefore, it's as secure as WCF can be. If you decide to change default ```127.0.0.1``` bind address to something else, such as ```0.0.0.0```, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF port. In addition to that, server must include properly set non-zero ```SteamOwnerID```, otherwise it'll refuse to execute any command, as an extra security measure.

Also keep in mind that listening on ```127.0.0.1``` is possible only in ```NetTcp``` ```WCFBinding```. If you're using any ```Http``` binding then ASF is listening on ```0.0.0.0```, even if you specified ```127.0.0.1``` as your ```WCFHost```.

---

## Troubleshooting

**Problem:**
```
System.ServiceModel.AddressAccessDeniedException
```
```
ERROR|ASF|StartServer() WCF service could not be started because of AddressAccessDeniedException!
ERROR|ASF|StartServer() If you want to use WCF service provided by ASF, consider starting ASF as administrator, or giving proper permissions!
```

**Solution:**

**Windows:** Starting with Windows Vista, user cannot create listening sockets by himself anymore. To solve this issue you'll need to start ASF.exe as administrator. Client doesn't have to be started by administrator, but it won't cause any problem if it does.

**Mono:** Make sure that your OS policy allows your user to create listening sockets. This should be the case on all Linux distributions and OS X by default, but if you have more strict security policy, you may need to ask ```root``` for permission. Such access is required if you for some reason decide to use port from ```0-1024``` range instead of recommended port above 1024.