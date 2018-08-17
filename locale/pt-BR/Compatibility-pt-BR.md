# Compatibilidade

O ASF é um aplicativo C# que roda na plataforma .NET Core. Isso que significa que o ASF não é compilado diretamente no **[código de máquina](https://en.wikipedia.org/wiki/Machine_code)** que roda no seu CPU, mas na **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)** que requer um tempo de execução compatível com a mesma para executá-la.

Esta abordagem tem uma quantidade gigantesca de vantagens, como a CIL é independente de plataforma o ASF pode ser executado nativamente em vários Sistemas Operacionais, especialmente no Windows, Linux e OS X. Não apenas a emulação é desnecessária, como também há suporte para quaisquer otimizações relacionadas a plataforma ou hardware, tais como instruções CPU SSE. Graças a isso, o ASF pode alcançar desempenho e otimização superiores, enquanto continua a oferecer compatibilidade e confiabilidade perfeitos.

Isto também significa que o ASF **não tem nenhum requisito específico quanto a OS**, porque ele exige um **tempo de execução** funcional na OS e não a OS em si. Enquanto o tempo de execução estiver executando o código ASF corretamente, não importa se o sistema operacional for Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii ou a sua torradeira - desde que rode o **[.NET Core](https://github.com/dotnet/core-setup#daily-builds)**, roda o **[ASF](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**.

No entanto, independentemente de onde você executar o ASF, você deve garantir que sua plataforma de destino tenha os requisitos para o **[.NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados. Essas são bibliotecas de baixo nível necessárias para a funcionalidade adequada do tempo de execução e são o primordiais para o funcionamento do ASF. É bem possível que você já tenha algumas delas (ou mesmo todas) já instaladas.

* * *

## Empacotamento do ASF

O ASF está disponível em 2 tipos diferentes - pacote genérico e pacote específico para OS. Funcionalmente ambos os pacotes são exatamente o mesmo, e ambos são capazes de se atualizarem automaticamente. A única diferença entre eles é se o pacote **genérico** do ASF acompanha ou não o tempo de execução**específico da OS** para rodar o mesmo.

* * *

### Genérico

O pacote genérico é independente de plataforma de compilação e não inclui nenhum código de máquina específico. Esta instalação requer que você tenha o temo de execução .NET Core, **na versão apropriada**, já instalada no seu sistema operacional. Nós todos sabemos como é complicado manter dependências atualizadas, portanto este pacote existe principalmente para pessoas que **já usam o** .NET Core e não querem duplicar seu tempo de execução unicamente para o ASF, uma vez que eles podem fazer uso do que eles já tem instalado. O pacote genérico também permite que você rode o ASF **em qualquer lugar onde você possa obter uma implementação funcional do tempo de execução .NET Core**, independentemente de existir ou não uma compilação do ASF específica para a OS.

Não se recomenda usar o tipo genérico se você é um usuário casual ou mesmo avançado que quer apenas fazer o ASF funcionar e não entrar em detalhes técnicos do .NET Core. Em outras palavras - se você sabe o que ele é, você pode usá-lo, caso contrário é muito melhor usar o pacote específico para a sua OS, que será explicado abaixo.

#### Pacote do .NET framework

Além do pacote genérico mencionado acima, há também pacote `netf-genérico` que é construído em cima do .NET Framework (e não do .NET Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET Core `generic` package can't as of today.

In general you should **avoid this package as much as possible**, as majority of operating systems and setups are perfectly (and much better) supported with `generic` package mentioned above. In fact, this package makes sense to be used only on platforms that lack working .NET Core runtime, while having working Mono implementation. An example of such platform would be `linux-x86` that didn't receive working .NET Core runtime as of today.

As the time goes on with more platforms being supported by .NET Core and less compatibility between .NET Framework and .NET Core, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET Core package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET Core versions, and it'll be only less functional as the time goes on. We offer support for this package only on machines that can't use `generic` variant above (e.g. `linux-x86`), and only with up-to-date runtime (e.g. latest Mono).

* * *

### OS-specific

OS-specific package, apart from managed code included in generic package, also includes native code for given platform. In other words, OS-specific package **already includes proper .NET Core runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. OS-specific package, as you can guess from the name, is OS-specific and every OS requires its own version - for example Windows requires PE32+ `ArchiSteamFarm.exe` binary while Linux works with Unix ELF `ArchiSteamFarm` binary. As you might know, those two types are not compatible with each other.

ASF is currently available in following OS-specific variants:

- `win-x64` works on 64-bit Windows OSes. This includes Windows 7 (SP1+), 8.1, 10, Server 2008 R2 (SP1+), 2012, 2012 R2, 2016, as well as future versions.
- `linux-arm` works on 32-bit ARM-based (ARMv7+) Linux OSes. This includes especially Raspberry Pi 2 & 3 with all glibc-based Linux OSes available for them, in current and future versions. This variant will not work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1.
- `linux-x64` works on 64-bit glibc-based Linux OSes. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-x64` works on 64-bit OS X OSes. This includes 10.12, as well as future versions.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET Core runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET Core runtime. This is important to note - ASF requires .NET Core runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET Core SDK in `win-x86` version and run generic ASF just fine. We simply can't target every OS-architecture combination that exists and is used by somebody, so we have to draw a line somewhere. x86 is a good example of that line, as it's obsolete architecture since at least 2004.

For a complete list of all supported platforms and OSes by .NET Core 2.1, visit **[release notes](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md)**.

* * *

## Runtime requirements

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET Core runtime or SDK**, as OS-specific builds require only native OS dependencies (prereqs) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET Core runtime supports platform required by ASF.

ASF as a program is targetting **.NET Core 2.1** (`netcoreapp2.1`) right now, but it might target newer platform in the future. `netcoreapp2.1` is supported since 2.1.300 SDK (2.1.0 runtime), although we recommend using **[latest SDK](https://www.microsoft.com/net/download)** available for your machine.

If in doubt, check what our **[continuous integration uses](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** for compiling and deploying ASF releases on GitHub. You can find `dotnet --info` output on top of each build.

* * *

## Issues and solutions

### Debian Jessie upgrade

If you upgraded from Debian 8 Jessie (or older) to Debian 9 Stretch, ensure that you **don't** have `libssl1.0.0` package, for example with `apt-get purge libssl1.0.0`. Otherwise, you might run into a segfault. This package is obsolete and doesn't exist by definition, neither is possible to install on clean Debian 9 setups, the only way to run into this issue is upgrading from Debian 8 or older - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. If you have some other packages depending on that outdated libssl version then you should either upgrade them, or get rid of them - not only because of this issue, but also because they're based on obsolete library in the first place.

### Invalid machine certs with ASF V3.2+

Under linux (only), you might get specific issue related to SSL certificates provided by your machine. This will show as an exception below:

```csharp
System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception. ---> Interop+Crypto+OpenSslCryptographicException: error:2006D002:BIO routines:BIO_new_file:system lib
   at Interop.Crypto.CheckValidOpenSslHandle(SafeHandle handle)
```

This issue is most likely caused by some of the certificates not being possible to be read. This could be because of insufficient permissions (as a test you can check if ASF works under `root` user) or other issues populating invalid certificate files.

The current workaround depends on the root issue. On clean setups (e.g. Debian), this should never happen as there should be no invalid/sensitive certificates in the store, so the issue considers only people that are most likely having other certs for other purposes (e.g. VPN or websites) that ASF has no access to read. You can consider either giving proper permissions to ASF user (such as ensuring that all SSL certificates in `/etc/pki` and `/etc/ssl` have at least `644` global read permission), running ASF under `root`, or moving sensitive certificates somewhere else so ASF does not attempt to read them during initialization.

This issue is supposed to be fixed with next .NET Core 2.1.1 servicing release, therefore the workarounds presented above shouldn't be required anymore in near future.

Reference: **[dotnet/corefx #29942](https://github.com/dotnet/corefx/issues/29942)**.

### .NET Core runtime picking wrong `libcurl.so` library

If you have both `libcurl.so.3` and `libcurl.so.4` on your system then .NET Core might decide to pick second one, which will lead to ASF crash the moment it'll try to initialize its http client.

```csharp
OnUnhandledException() System.TypeInitializationException: The type initializer for 'System.Net.Http.CurlHandler' threw an exception. ---> System.TypeInitializationException: The type initializer for 'Http' threw an exception. ---> System.TypeInitializationException: The type initializer for 'HttpInitializer' threw an exception. ---> System.DllNotFoundException: Unable to load DLL 'System.Net.Http.Native': The specified module or one of its dependencies could not be found.
```

If you stumble upon the issue above, then you might need to manually tell .NET Core runtime to pick up proper library in this case. Locate `libcurl.so.3` on your system and add it to `LD_PRELOAD` before starting ASF:

```shell
LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm
```

This should hopefully solve the issue, assuming your `libcurl.so.3` is working properly.