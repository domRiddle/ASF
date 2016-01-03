# WCF

Starting with version 1.3+, ASF now offers WCF (Windows Communication Foundation) inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

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
[*] NOTICE: StartServer() <WCF> Starting WCF server...
[*] NOTICE: StartServer() <WCF> WCF server ready!
```

ASF is now listening on ```http://localhost:1242/ASF``` for incoming WCF connections.

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

ASF expects that every command will have a structure of ```<Command> <BotName> (ExtraArgs)```. Commands should not be prefixed by ```!```, ASF prefixes them for you. ExtraArgs are optional, required by some commands (e.g. ```redeem``` one).

ASF in WCF mode supports all bot-targeted commands, such as ```!stop <BOT>```, ```!start <BOT>``` or ```!redeem <BOT> <KEY>```. You can view all available commands **[here](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)**.

If you want to execute more than one command, consider launching them at the same time (as shown in example above), because launching process and the client is quite costly. ASF will execute all of those commands synchronously in given order.

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
[*] INFO: HandleCommand() <WCF> Received command: "!stop archi"
[*] INFO: Stop() <archi> Stopping...
[*] INFO: HandleCommand() <WCF> Answered to command: "stop archi" with: "Done!"
[*] INFO: OnDisconnected() <archi> Disconnected from Steam!
```

Client results:
```
[*] NOTICE: ParseArgs() <WCF> Command sent: "!stop archi"
[*] NOTICE: ParseArgs() <WCF> Response received: "Done!"
```

---

## FAQ

**Q:** Why should I consider using WCF in ASF?

**A:** You should consider using WCF in ASF only if you have a strong reason. If you don't know what it is about, most likely you don't have one, and you can safely ignore this page along with the content. WCF provides inter-process communication, which allows you to control the ASF process during execution, e.g. through executing client via PHP script or similar. By using WCF you're able to control how ASF behaves, which might be useful if you're planning on integrating it further.

**Q:** Is this secure?

**A:** ASF by default listens only on ```localhost``` address, which means that accessing ASF WCF from any other machine but your own is impossible.

---

## Troubleshooting

**Problem:**
```
[!] WARNING: StartServer() <WCF> WCF service could not be started because of AddressAccessDeniedException
[!] WARNING: StartServer() <WCF> If you want to use WCF service provided by ASF, consider starting ASF as administrator, or giving proper permissions
```

**Solution:**

**Windows:** Starting with Windows Vista, user cannot create listening sockets by himself anymore. To solve this issue you'll need to start ASF.exe as administrator.

**Mono:** Make sure that your OS policy allows your user to create listening sockets. This should be the case on all Linux distributions and OS X by default, but if you have more strict security policy, you may need to ask ```root``` for permission.