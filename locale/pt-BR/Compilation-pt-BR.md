# Compilação

Compilação é o processo de criação de arquivo executável. This is what you want to do if you want to add your own changes to ASF, or if you for whatever reason don't trust executable files provided in official **[releases](https://github.com/JustArchiNET/ArchiSteamFarm/releases)**. Se você é um usuário e não um desenvolvedor, é mais provável que você queira usar binários pré-compilados, mas se você quiser usar os seus próprios, ou aprender algo novo, continue a leitura.

O ASF pode ser compilado em qualquer plataforma suportada atualmente, desde que você tenha todas as ferramentas necessárias.

* * *

## .NET Core SDK

Independente da plataforma, você precisa do SDK completo do .NET Core (e não apenas o tempo de execução) em para compilar o ASF. Instruções de instalação podem ser encontradas na **[página de instalação do .NET Core](https://www.microsoft.com/net/download)**. Você precisa instalar a versão apropriada do SDK do .NET Core para seu sistema operacional. Após a instalação bem sucedida, o comando `dotnet` deverá estar funcional e operante. Você pode verificar se ele funciona com `dotnet --info`. Also ensure that your .NET Core SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

Exemplo de `dotnet--info` no Windows:

    .NET Core SDK (reflecting any global.json):
     Version:   2.1.300
     Commit:    adab45bf0c
    
    Runtime Environment:
     OS Name:     Windows
     OS Version:  10.0.17134
     OS Platform: Windows
     RID:         win10-x64
     Base Path:   C:\Program Files\dotnet\sdk\2.1.300\
    
    Host (useful for support):
      Version: 2.1.0
      Commit:  caa7b7e2ba
    
    .NET Core SDKs installed:
      2.1.300 [C:\Program Files\dotnet\sdk]
    
    .NET Core runtimes installed:
      Microsoft.AspNetCore.All 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.All]
      Microsoft.AspNetCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
      Microsoft.NETCore.App 2.1.0 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
    

* * *

## Compilação

Supondo que você tenha o SDK do .NET Core operacional e na versão apropriada, basta navegar até o diretório do ASF e executar:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/generic" "/p:LinkDuringPublish=false"
```

Se você estiver usando Linux/OS X, você pode usar o código `cc.sh`, que fará o mesmo de uma maneira um pouco mais complexa.

Se a compilação terminou com êxito, você pode encontrar seu ASF `fonte` na pasta `ArchiSteamFarm/out/generic`. Essa compilação é a mesma que a `genérica` do ASF, mas ela força o valor de `UpdateChannel` e `UpdatePeriod` para `0`.

### SO específico

Você também pode gerar um pacote .NET Core específico para OS se você tiver uma necessidade particular. Em geral, você não deverá fazer isso, pois você já compilou o tipo `genérico` que você pode rodar em seu já instalado tempo de execução .NET Core, que você usou para a compilação, mas caso você queira:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "netcoreapp2.1" -o "out/linux-x64" -r "linux-x64" "/p:CrossGenDuringPublish=false"
```

Claro, troque `linux-x64` pela arquitetura de SO que você quer atender, tal como `win-x64`. Essa compilação também terá as atualizações desabilitadas.

### .NET framework

Em casos muito raros, quando você quiser compilar um pacote `generic-netf`, você pode mudar a estrutura desejada de `netcoreapp2.1` para `net472`. Tenha em mente que você vai precisar do pacote de desenvolvedor **[.NET Framework](https://www.microsoft.com/net/download/visual-studio-sdks)** apropriado para compilar a variante `netf`, além do SDK do .NET Core.

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net472" -o "out/generic-netf"
```

Em casos ainda mais raros, se você não pode instalar o .NET Framework ou mesmo o SDK do .NET Core em si (p. ex., por estar compilando no `linux-x86` com `mono`), você pode chamar `msbuild` diretamente:

```shell
msbuild /m /p:Configuration=Release /p:PublishDir=out/generic-netf /p:TargetFramework=net472 /r /t:Publish ArchiSteamFarm
```

* * *

## Desenvolvimento

Se você gostaria de editar o código do ASF, você pode usar qualquer IDE compatível com o .NET Core, embora até mesmo isso seja opcional, uma vez que você pode editar em um bloco de notas e compilar com o comando `dotnet` descrito acima. Ainda assim, para Windows, recomendamos **[o Visual Studio mais recente](https://www.visualstudio.com/downloads)** (a versão gratuita community é mais que o suficiente). Também sugerimos usá-lo juntamente com **[ReSharper](https://www.jetbrains.com/resharper)**, embora este não seja um produto gratuito.

Se, em vez disso, você quiser trabalhar com o código ASF no Linux/Mac OS X, recomendamos o **[Visual Studio Code mais recente](https://code.visualstudio.com/download)**. Ele não é tão rico quanto o Visual Studio clássico, mas é bom o suficiente.

Claro que todas as sugestões acima são apenas recomendações, você pode usar o que quiser, tudo se resume ao comando `dotnet build` de qualquer maneira. Nós utilizamos o Visual Studio + ReSharper para o desenvolvimento do ASF, com uma pequena parte de `ferramentas` de terceiros que você pode encontrar no repositório.

* * *

## Marcadores

`master` branch is not guaranteed to be in a state that allows successful compilation or flawless ASF execution in the first place, since it's development branch just like stated in our **[release cycle](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. If you want to compile ASF from source, then you should use appropriate **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** for that purpose, which guarantees at least successful compilation, and very likely also flawless execution (if build was marked as stable release). In order to check the current "health" of the tree, you can use our CIs - **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** or **[Travis](https://travis-ci.com/JustArchiNET/ArchiSteamFarm)**.

* * *

## Versões oficiais

Official ASF releases are compiled by **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** on Windows, with latest .NET Core SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. Depois de passar nos testes, todos os pacotes são implantados no GitHub. Isto também garante transparência, uma vez que o AppVeyor sempre usa uma fonte pública oficial para todas as compilações, e você pode comparar as somas de verificação dos artefatos do AppVeyor com os ativos no GitHub. Os desenvolvedores do ASF não compilam ou publicam as compilações manualmente, exceto para o processo de desenvolvimento privado, incluindo depuração.