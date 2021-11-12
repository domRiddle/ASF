# Compatibilidade

ASF is a C# application that is running on .NET platform. Isso significa que o ASF não é compilado diretamente em **[código de máquina](https://pt.wikipedia.org/wiki/C%C3%B3digo_de_m%C3%A1quina)** que roda no seu CPU, mas em **[CIL](https://pt.wikipedia.org/wiki/Common_Intermediate_Language)**, que requer um tempo de execução compatível para a execução.

Esta abordagem tem uma quantidade gigantesca de vantagens, como a CIL não é dependente de plataforma, o ASF pode ser executado nativamente em vários Sistemas Operacionais, especialmente no Windows, Linux e macOS. Não apenas é desnecessário qualquer emulação como também há suporte para quaisquer otimizações relacionadas a plataforma ou hardware, tais como instruções CPU SSE. Graças a isso, o ASF pode alcançar desempenho e otimização superiores, enquanto continua a oferecer compatibilidade e confiabilidade perfeitos.

Isto também significa que o ASF **não tem nenhum requisito específico quanto ao Sistema Operacional**, porque ele exige um **tempo de execução** funcional no Sistema Operacional e não o Sistema Operacional em si. As long as that runtime is executing ASF code properly, it does not matter whether underlying OS is Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii or your toaster - as long as there is **[.NET for it](https://dotnet.microsoft.com/download/dotnet)**, there is **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** for it.

However, regardless of where you run ASF, you must ensure that your target platform has **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed. Essas são bibliotecas de baixo nível necessárias para a funcionalidade adequada do tempo de execução e são primordiais para o funcionamento do ASF. É bem possível que você já tenha algumas delas (ou mesmo todas) já instaladas.

---

## Empacotamento do ASF

O ASF está disponível em 2 formas diferentes: pacote genérico e pacote específico para Sistema Operacional. Funcionalmente ambos os pacotes são exatamente o mesmo e ambos são capazes de se atualizarem automaticamente. A única diferença entre eles é se o pacote **genérico** do ASF acompanha ou não o tempo de execução **específico do Sistema Operacional** para rodar o mesmo.

---

### Genérico

O pacote genérico não depende de plataforma de compilação e não inclui nenhum código de máquina específico. This setup requires from you to have .NET runtime already installed on your OS **in appropriate version**. We all know how troublesome it is to keep dependencies up-to-date, therefore this package is here mainly for people that **already use** .NET and don't want to duplicate their runtime solely for ASF if they can make use of what they have installed already. Generic package also allows you to run ASF **anywhere where you can obtain working implementation of .NET runtime**, regardless if there exists OS-specific ASF build for it, or not.

It's not recommended to use generic flavour if you're casual or even advanced user that just wants to make ASF work and not dig into .NET technical details. Em outras palavras: se você sabe o que ele é, você pode usá-lo, caso contrário é muito melhor usar o pacote específico para o seu Sistema Operacional, que será explicado abaixo.

#### Pacote do .NET Framework

In addition to generic package mentioned above, there is also `generic-netf` package which is built on top of .NET Framework and not .NET (Core). This package is a legacy variant that provides missing compatibility known from ASF V2 times, and can be run e.g. with **[Mono](https://www.mono-project.com)**, while .NET `generic` package can't as of today.

Em geral, você deve **evitar este pacote o quanto for possível**, já que a maioria dos sistemas operacionais e configurações são perfeitamente (e muito mais) compatíveis com o pacote `genérico` mencionado acima. In fact, this package makes sense to be used only on platforms that lack working .NET runtime, while having working Mono implementation. Examples of such platforms include `linux-x86` (32-bit i386/i686 linux), as well as `linux-armel` (ARMv6 boards found e.g. in Raspberry Pi 0 & 1), all of which do not have official working .NET runtime as of today.

As the time goes on with more platforms being supported by .NET and less compatibility between .NET Framework and .NET, `generic-netf` package will be entirely replaced with `generic` one in the future. Please refrain from using it if you can use any .NET package instead, as `generic-netf` is missing a lot of functionality and compatibility compared to .NET versions, and it'll be only less functional as the time goes on. Oferecemos suporte para este pacote **apenas** em máquinas que não podem usar a variante `genérica` acima (por exemplo, o `linux-x86`) e somente com o tempo de execução atualizado (por exemplo, o Mono mais recente).

---

### Sistema Operacional específico

O pacote para Sistema Operacional específico, além do código gerenciado incluso no pacote genérico, também inclui código nativo para dada plataforma. In other words, OS-specific package **already includes proper .NET runtime inside**, which allows you to entirely skip the whole installation mess and just launch ASF directly. O pacote para Sistema Operacional específico, como você pode adivinhar pelo nome, é específico para cada sistema operacional e cada um requer sua própria versão; por exemplo, o Windows requer o binário PE32 + `ArchiSteamFarm.exe`, enquanto o Linux trabalha com o binário Unix ELF `ArchiSteamFarm`. Como você deve saber, esses dois tipos não são compatíveis um com o outro.

ASF está atualmente disponível nas seguintes variantes específicas de Sistema Operacional:

- `win-x64` compatível com a versão de 64 bits do Windows. This includes Windows 7 (SP1+), 10, 11, Server 2012+ as well as future versions.
- `linux-arm` compatível com a versão de 32 bits do GNU/Linux com base no ARM (ARMv7+). This includes platforms such as Raspberry Pi 2 (and newer) with all GNU/Linux OSes available for them (such as Raspberry Pi OS), in current and future versions. This variant will **not** work with older ARM architectures, such as ARMv6 found in Raspberry Pi 0 & 1, it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-arm64` compatível com a versão de 64 bits do GNU/Linux com base no ARM (ARMv8+). Isso inclui plataformas como Raspberry Pi 3 (e mais recente) com todos os sistemas operacionais GNU/Linux com base no ARM de 64 bits disponíveis (como o Debian), nas versões atuais e futuras. This variant will **not** work with 32-bit OSes that do not have required 64-bit libraries available (such as Raspberry Pi OS), it will also not work with OSes that do not implement required GNU/Linux environment (such as Android).
- `linux-x64` compatível com a versão de 64 bits do GNU/Linux. This includes Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu, OpenSUSE/SLES and many other ones, including their derivatives, in current and future versions.
- `osx-arm64` works on 64-bit ARM-based (Apple silicon) OS X OSes. This includes version 11, as well as future ones.
- `osx-x64` funciona em Sistemas Operacionais OS X 64-bit. This includes version 10.14, as well as future ones.

Of course, even if you don't have OS-specific package available for your OS-architecture combination, you can always install appropriate .NET runtime yourself and run generic ASF flavour, which is also the main reason why it exists in the first place. Generic ASF build is platform-agnostic and will run on any platform that has a working .NET runtime. This is important to note - ASF requires .NET runtime, not some specific OS or architecture. For example, if you're running 32-bit Windows then despite of no dedicated `win-x86` ASF version, you can still install .NET SDK in `win-x86` version and run generic ASF just fine. Nós simplesmente não podemos atender a todas as combinações de arquitetura de Sistemas Operacionais que existam e são usadas por alguém, então temos que traçar um limite em algum lugar. O x86 é um bom exemplo desse limite, já que é uma arquitetura obsoleta desde pelo menos 2004.

For a complete list of all supported platforms and OSes by .NET 6.0, visit **[release notes](https://github.com/dotnet/core/blob/main/release-notes/6.0/supported-os.md)**.

---

## Requisitos do tempo de execução

If you're using OS-specific package then you don't need to worry about runtime requirements, because ASF always ships with required and up-to-date runtime that will work properly as long as you have **[.NET prerequisites](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)** installed and up-to-date. In other words, **you don't need to install .NET runtime or SDK**, as OS-specific builds require only native OS dependencies (prerequisites) and nothing else.

However, if you're trying to run **generic** ASF package then you must ensure that your .NET runtime supports platform required by ASF.

ASF as a program is targeting **.NET 6.0** (`net6.0`) right now, but it may target newer platform in the future. `net6.0` is supported since 6.0.100 SDK (6.0.0 runtime), although ASF is configured to prefer **latest runtime at the moment of compilation**, so you should ensure that you have **[latest SDK](https://dotnet.microsoft.com/download)** (or at least runtime) available for your machine. Generic ASF variant may refuse to launch if your runtime is older than the specified minimum supported one during compilation.

If in doubt, check what our **[continuous integration uses](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** for compiling and deploying ASF releases on GitHub. You can find `dotnet --info` output in every build as part of .NET verification step.