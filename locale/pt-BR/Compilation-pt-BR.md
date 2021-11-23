# Compilação

Compilação é o processo de criação de arquivo executável. É isso que você quer fazer se você quiser adicionar suas próprias mudanças ao ASF, ou se você, por alguma razão não confia em arquivos executáveis fornecidos em **[lançamentos](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** oficiais. Se você é um usuário e não um desenvolvedor, é mais provável que você queira usar binários pré-compilados, mas se você quiser usar os seus próprios, ou aprender algo novo, continue a leitura.

O ASF pode ser compilado em qualquer plataforma suportada atualmente, desde que você tenha todas as ferramentas necessárias.

---

## SDK .NET

Independente da plataforma, você precisa do SDK completo do .NET Core (e não apenas o tempo de execução) em para compilar o ASF. Instruções de instalação podem ser encontradas na **[página de instalação do .NET Core](https://dotnet.microsoft.com/download)**. Você precisa instalar a versão apropriada do SDK do .NET Core para seu sistema operacional. Após a instalação bem sucedida, o comando `dotnet` deverá estar funcional e operante. Você pode verificar se ele funciona com `dotnet --info`. Certifique-se também de que o seu SDK do .NET Core corresponde aos **[requisitos de tempo de execução](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#requisitos-do-tempo-de-execu%C3%A7%C3%A3o)** do ASF.

---

## Compilação

Assumindo que você tenha o SDK .NET na versão apropriada, simplesmente navegue para o diretório raiz do ASF (copiado ou baixado e descompactado do repositório do ASF) e execute:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/generic"
```

Se você estiver usando Linux/macOS, você pode usar o código `cc.sh`, que terá o mesmo resultado de uma maneira um pouco mais complexa.

Se a compilação obteve sucesso você poderá encontrar a `source` da sua versão do ASF na pasta `out/generic`. Essa compilação é a mesma que a `genérica` do ASF, mas ela força o valor de `UpdateChannel` e `UpdatePeriod` para `0`, o que é o correto para versões auto compiladas.

### SO específico

Você também pode gerar um pacote .NET específico para OS se você tiver uma necessidade particular. Em geral, você não deverá fazer isso, pois você já compilou o tipo `genérico` que você pode rodar em seu já instalado tempo de execução .NET, que você usou para a compilação, mas caso você queira:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/linux-x64" -r "linux-x64"
```

Claro, troque `linux-x64` pela arquitetura de SO que você quer atender, tal como `win-x64`. Essa compilação também terá as atualizações desabilitadas.

### .NET framework

In a very rare case when you'd want to build `generic-netf` package, you can change target framework from `net6.0` to `net48`. Tenha em mente que você vai precisar do pacote de desenvolvedor **[.NET Framework](https://dotnet.microsoft.com/download/visual-studio-sdks)** apropriado para compilar a variante `netf`, além do SDK do .NET, então a instrução abaixo funcionará apenas no Windows:

```shell
dotnet publish ArchiSteamFarm -c "Release" -f "net48" -o "out/generic-netf"
```

No caso de você não conseguir instalar o .NET Framework ou mesmo o próprio SDK do .NET (p. ex., por estar compilando no `linux-x86` com `mono`), você pode chamar `msbuild` diretamente. Você também precisará especificar o `ASFNetFramework` manualmente, já que o ASF desativa por padrão a compilação `netf` em plataformas não-Windows:

```shell
msbuild /m /r /t:Publish /p:Configuration=Release /p:TargetFramework=net48 /p:PublishDir=out/generic-netf /p:ASFNetFramework=true ArchiSteamFarm
```

### ASF-ui

While the above steps are everything that is required to have a fully working build of ASF, you may *also* be interested in building **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)**, our graphical web interface. From ASF side, all you need to do is dropping ASF-ui build output in standard `ASF-ui/dist` location, then building ASF with it (again, if needed).

ASF-ui is part of ASF's source tree as a **[git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)**, ensure that you've cloned the repo with `git clone --recursive`, as otherwise you'll not have the required files. You'll also need a working NPM, **[Node.js](https://nodejs.org)** comes with it. If you're using Linux/OS X, we recommend our `cc.sh` script, which will automatically cover building and shipping ASF-ui (if possible, that is, if you're meeting the requirements we've just mentioned).

In addition to the `cc.sh` script, we also attach the simplified build instructions below, refer to **[ASF-ui repo](https://github.com/JustArchiNET/ASF-ui)** for additional documentation. From ASF's source tree location, so as above, execute the following commands:

```shell
rm -rf "ASF-ui/dist" # ASF-ui doesn't clean itself after old build

npm ci --prefix ASF-ui
npm run-script deploy --prefix ASF-ui

rm -rf "out/generic/www" # Ensure that our build output is clean of the old files
dotnet publish ArchiSteamFarm -c "Release" -f "net6.0" -o "out/generic" # Or accordingly to what you need as per the above
```

You should now be able to find the ASF-ui files in your `out/generic/www` folder. ASF will be able to serve those files to your browser.

Alternatively, you can simply build ASF-ui, whether manually or with the help of our repo, then copy the build output over to `${OUT}/www` folder manually, where `${OUT}` is the output folder of ASF that you've specified with `-o` parameter. This is exactly what ASF is doing as part of the build process, it copies `ASF-ui/dist` (if exists) over to `${OUT}/www`, nothing fancy.

---

## Desenvolvimento

Se você quiser editar o código do ASF, você pode usar qualquer IDE compatível com o .NET, embora até mesmo isso seja opcional, uma vez que você pode editar em um bloco de notas e compilar com o comando `dotnet` descrito acima. Mesmo assim, para o Windows nós recomendamos **[o Visual Studio mais recente](https://visualstudio.microsoft.com/downloads)** (a versão comunidade é mais que o suficiente, e é gratuita).

Se, em vez disso, você quiser trabalhar com o código ASF no Linux/Mac OS X, recomendamos o **[Visual Studio Code mais recente](https://code.visualstudio.com/download)**. Ele não é tão rico quanto o Visual Studio clássico, mas é bom o suficiente.

Claro que todas as sugestões acima são apenas recomendações, você pode usar o que quiser, tudo se resume ao comando `dotnet build` de qualquer maneira. Nós usamos o **[Rider da Jetbrains](https://www.jetbrains.com/rider)** para o desenvolvimento do ASF, embora não seja uma solução gratuita.

---

## Marcadores

Não é garantido que a ramificação `main` esteja em um estado que propicie uma compilação bem sucedida ou uma execução sem falhas do ASF, uma vez que é uma ramificação em desenvolvimento, confirme especificado em nosso **[ciclo de lançamentos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle)**. Se você deseja compilar ou referenciar o ASF desde a fonte, então você deve usar a **[tag](https://github.com/JustArchiNET/ArchiSteamFarm/tags)** apropriada para tal, o que garante ao menos uma compilação bem sucedida, e muito provavelmente uma execução sem erros (se a compilação foi marcada como versão estável). In order to check the current "health" of the tree, you can use our CI - **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/ci.yml?query=branch%3Amain)**.

---

## Versões oficiais

Os lançamentos oficiais do ASF são compilados pelo **[GitHub](https://github.com/JustArchiNET/ArchiSteamFarm/actions)** no Windows, com o SDK .NET mais recente que corresponde com os **[requisitos de tempo de execução](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#requisitos-do-tempo-de-execução)** do ASF. Depois de passar nos testes, todos os pacotes são implantados no lançamento, assim como no GitHub. Isto também garante transparência, pois o GitHub sempre usa uma fonte pública oficial para todas as compilações, e você pode comparar as somas de verificação (checksums) dos artefatos do GitHub com os ativos lançados no GitHub. Os desenvolvedores do ASF não compilam ou publicam as compilações por conta própria, exceto para o processo de desenvolvimento privado e depuração.