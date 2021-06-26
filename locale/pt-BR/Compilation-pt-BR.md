# Compilação

Compilação é o processo de criação de arquivo executável. É isso que você quer fazer se você quiser adicionar suas próprias mudanças ao ASF, ou se você, por alguma razão não confia em arquivos executáveis fornecidos em **[lançamentos](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** oficiais. Se você é um usuário e não um desenvolvedor, é mais provável que você queira usar binários pré-compilados, mas se você quiser usar os seus próprios, ou aprender algo novo, continue a leitura.

O ASF pode ser compilado em qualquer plataforma suportada atualmente, desde que você tenha todas as ferramentas necessárias.

---

## .NET SDK

Regardless of platform, you need full .NET SDK (not just runtime) in order to compile ASF. Installation instructions can be found on **[.NET download page](https://dotnet.microsoft.com/download)**. You need to install appropriate .NET SDK version for your OS. Após a instalação bem sucedida, o comando `dotnet` deverá estar funcional e operante. Você pode verificar se ele funciona com `dotnet --info`. Also ensure that your .NET SDK matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**.

---

## Compilação

Assuming you have .NET SDK operative and in appropriate version, simply navigate to source ASF directory (cloned or downloaded and unpacked ASF repo) and execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/generic"
```

Se você estiver usando Linux/macOS, você pode usar o código `cc.sh`, que terá o mesmo resultado de uma maneira um pouco mais complexa.

Se a compilação obteve sucesso você poderá encontrar a `source` da sua versão do ASF na pasta `out/generic`. Essa compilação é a mesma que a `genérica` do ASF, mas ela força o valor de `UpdateChannel` e `UpdatePeriod` para `0`, o que é o correto para versões auto compiladas.

### SO específico

You can also generate OS-specific .NET package if you have a specific need. In general you shouldn't do that because you've just compiled `generic` flavour that you can run with your already-installed .NET runtime that you've used for the compilation in the first place, but just in case you want to:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net5.0" -o "out/linux-x64" -r "linux-x64"
```

Claro, troque `linux-x64` pela arquitetura de SO que você quer atender, tal como `win-x64`. Essa compilação também terá as atualizações desabilitadas.

### .NET framework

Em casos muito raros, quando você quiser compilar um pacote `generic-netf`, você pode mudar a estrutura desejada de `net5.0` para `net48`. Keep in mind that you'll need appropriate **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** developer pack for compiling `netf` variant, in addition to .NET SDK, so the below will work only on Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

In case of being unable to install .NET Framework or even .NET SDK itself (e.g. because of building on `linux-x86` with `mono`), you can call `msbuild` directly. Você também precisará especificar o `ASFNetFramework` manualmente, já que o ASF desativa por padrão a compilação `netf` em plataformas não-Windows:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

---

## Desenvolvimento

If you'd like to edit ASF code, you can use any .NET compatible IDE for that purpose, although even that is optional, since you can as well edit with a notepad and compile with `dotnet` command described above. Mesmo assim, para o Windows nós recomendamos **[o Visual Studio mais recente](https://visualstudio.microsoft.com/downloads)** (a versão comunidade é mais que o suficiente, e é gratuita).

Se, em vez disso, você quiser trabalhar com o código ASF no Linux/Mac OS X, recomendamos o **[Visual Studio Code mais recente](https://code.visualstudio.com/download)**. Ele não é tão rico quanto o Visual Studio clássico, mas serve.

Claro que as opções acima são apenas recomendações, você pode usar qual quiser pois tudo se resume ao comando `dotnet build`. Nós usamos o **[Rider da Jetbrains](https://www.jetbrains.com/rider)** para o desenvolvimento do ASF, embora não seja uma solução gratuita.

---

## Marcadores

Não é garantido que a ramificação `main` esteja em um estado que propicie uma compilação bem sucedida ou uma execução sem falhas do ASF, uma vez que é uma ramificação em desenvolvimento, confirme especificado em nosso **[ciclo de lançamentos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Se você deseja compilar ou referenciar o ASF desde a fonte, então você deve usar a **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** apropriada para tal, o que garante ao menos uma compilação bem sucedida, e muito provavelmente uma execução sem erros (se a compilação foi marcada como versão estável). Para verificar a "saúde" atual da árvore, você pode usar nossas integrações contínuas - ** [GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** ou **[AppVeyor](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)**.

---

## Versões oficiais

Official ASF releases are compiled by **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** on Windows, with latest .NET SDK that matches ASF **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)**. Depois de passar nos testes, todos os pacotes são implantados no lançamento, assim como no GitHub. Isto também garante transparência, pois o GitHub sempre usa uma fonte pública oficial para todas as compilações, e você pode comparar as somas de verificação (checksums) dos artefatos do GitHub com os ativos lançados no GitHub. Os desenvolvedores do ASF não compilam ou publicam as compilações por conta própria, exceto para o processo de desenvolvimento privado e depuração.