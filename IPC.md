# IPC

Starting with version 3.0, ASF offers http-based inter-process communication that can be used to communicate with the process. This is offered as an alternative to already existing steam chat communication.

IPC is always executed with `SteamOwnerID` permissions, which is `0` by default. In order to use it, you should set `SteamOwnerID` to the proper non-zero value. Default value will make IPC work, but not authorizing any client to execute any command. For more info about `SteamOwnerID`, visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)**.

---

## Server

To start IPC, ASF must be started with `--server` parameter. For example in OS-specific package on Windows:
```
ArchiSteamFarm.exe --server
```

Or in generic package on any platform:
```
dotnet ArchiSteamFarm.dll -- --server
```

If parameter was passed correctly, you should notice that IPC service is active:
```
INFO|ASF|StartServer() Starting IPC server on http://127.0.0.1:1242/IPC...
INFO|ASF|StartServer() IPC server ready!
```

ASF is now listening on `http://127.0.0.1:1242/IPC` for incoming IPC connections (or whatever `IPCHost` and `IPCPort` you specified in the config).

---

## Client

Communication with IPC server provided by ASF can be done via any web browser, including CLI utilities such as `curl`. Currently ASF supports only one type of communication - sending a command. In order to send a command to IPC, you should send `http://127.0.0.1:1242/IPC?command=` `GET` request, including your command in `command` argument. For example, **[http://127.0.0.1:1242/IPC?command=version](http://127.0.0.1:1242/IPC?command=version)**.

ASF expects that every command will have a structure of `<Command> (BotName) (ExtraArgs)`. Commands don't have to be prefixed by `!`, ASF prefixes them for you if needed (useful on Unix). ExtraArgs are optional, required by some commands (e.g. `redeem` one).

ASF in IPC mode supports all commands that are available, you can review them in **[commands](https://github.com/JustArchi/ArchiSteamFarm/wiki/Commands)** section.

---

## FAQ

**Q:** Why should I consider using IPC in ASF?

**A:** You should consider using IPC in ASF only if you have a strong reason. If you don't know what it is about, most likely you don't have one, and you can safely ignore this page along with the content. IPC stands for inter-process communication, which allows you to control the ASF process during execution, e.g. through a PHP script or similar. By using IPC you're able to control how ASF behaves, which might be useful if you're planning on integrating it further. If you're not running ASF on the server, and you're not planning on integrating it further with your own scripts/applications, it should be easier for you to communicate with ASF through steam chat with one of the bots. However, you can use IPC too, if you consider it useful/easier for you.

---

**Q:** Is this secure?

**A:** ASF by default listens only on `127.0.0.1` address, which means that accessing ASF IPC from any other machine but your own is impossible. Therefore, it's as secure as IPC can be. If you decide to change default `127.0.0.1` bind address to something else, such as `0.0.0.0`, then you're supposed to set proper firewall rules **yourself** in order to allow only authorized IPs to access ASF port. In addition to that, server must include properly set non-zero `SteamOwnerID`, otherwise it'll refuse to execute any command, as an extra security measure.