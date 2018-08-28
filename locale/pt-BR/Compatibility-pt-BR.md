# Compatibilidade

O ASF é um aplicativo C# que roda na plataforma .NET Core. Isso significa que o ASF não é compilado diretamente no **[código de máquina](https://en.wikipedia.org/wiki/Machine_code)** que roda no seu CPU, mas na **[CIL](https://en.wikipedia.org/wiki/Common_Intermediate_Language)**, que requer um tempo de execução compatível com a mesma para executá-la.

Esta abordagem tem uma quantidade gigantesca de vantagens, como a CIL é independente de plataforma o ASF pode ser executado nativamente em vários SOs, especialmente no Windows, Linux e OS X. Não apenas a emulação é desnecessária, como também há suporte para quaisquer otimizações relacionadas a plataforma ou hardware, tais como instruções CPU SSE. Graças a isso, o ASF pode alcançar desempenho e otimização superiores, enquanto continua a oferecer compatibilidade e confiabilidade perfeitos.

Isto também significa que o ASF **não tem nenhum requisito específico quanto a SO**, porque ele exige um **tempo de execução** funcional no SO e não o SO em si. Enquanto o tempo de execução estiver executando o código ASF corretamente, não importa se o SO for Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii ou a sua torradeira - uma vez que rode o **[.NET Core](https://github.com/dotnet/core-setup#daily-builds)**, ele roda o **[ASF](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**.

No entanto, independentemente de onde você executar o ASF, você deve garantir que sua plataforma de destino tenha os requisitos para o **[.NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados. Essas são bibliotecas de baixo nível necessárias para a funcionalidade adequada do tempo de execução e são o primordiais para o funcionamento do ASF. É bem possível que você já tenha algumas delas (ou mesmo todas) já instaladas.

* * *

## Empacotamento do ASF

O ASF está disponível em 2 tipos diferentes - pacote genérico e pacote específico para SO. Funcionalmente ambos os pacotes são exatamente o mesmo, e ambos são capazes de se atualizarem automaticamente. A única diferença entre eles é se o pacote **genérico** do ASF acompanha ou não o tempo de execução **específico do SO** para rodar o mesmo.

* * *

### Genérico

O pacote genérico não depende de plataforma de compilação e não inclui nenhum código de máquina específico. Esta instalação requer que você tenha o tempo de execução .NET Core, **na versão apropriada**, já instalada no seu SO. Nós todos sabemos como é complicado manter dependências atualizadas, portanto este pacote existe principalmente para pessoas que **já usam o** .NET Core e não querem duplicar seu tempo de execução unicamente para o ASF, uma vez que eles podem fazer uso do que eles já tem instalado. O pacote genérico também permite que você rode o ASF **em qualquer lugar onde você possa obter uma implementação funcional do tempo de execução .NET Core**, independente de existir ou não uma compilação do ASF específica para a SO.

Não se recomenda usar o tipo genérico se você é um usuário casual ou mesmo avançado que quer apenas fazer o ASF funcionar e não entrar em detalhes técnicos do .NET Core. Em outras palavras - se você sabe o que ele é, você pode usá-lo, caso contrário é muito melhor usar o pacote específico para o seu SO, que será explicado abaixo.

#### Pacote do .NET Framework

Além do pacote genérico mencionado acima, há também um pacote `netf-genérico` que é construído em cima do .NET Framework (e não do .NET Core). Esse pacote é uma variante herdada que fornece compatibilidades que sabíamos faltar nos tempos do ASF V2, e pode rodar, p. ex., com o **[Mono](https://www.mono-project.com)**, enquanto o pacote `genérico` .NET Core já não pode.

Em geral, você deve **evitar este pacote o quanto for possível**, já que a maioria dos sistemas operacionais e configurações são perfeitamente (e muito mais) compatíveis com o pacote `genérico` mencionado acima. Na verdade, só faz sentido usar esse pacote em plataformas que não tenham o tempo de execução .NET Core, e tenham uma implementação funcional do Mono. Um exemplo de tal plataforma seria o `linux-x86` que ainda não recebeu uma versão funcional do tempo de execução .NET Core.

Conforme o tempo passa, com mais plataformas sendo suportadas pelo .NET Core, e com compatibilidade entre o .NET Framework e o .NET Core diminuindo, o pacote `genérico-netf` será inteiramente substituído pelo `genérico`. Por favor, evite usá-lo se você pode usar qualquer pacote do .NET Core em seu lugar, já que exite um monte de funcionalidades e compatibilidades faltantes no pacote `genérico-netf` em comparação com as versões .NET Core, e ele será cada vez menos funcional com o passar do tempo. Oferecemos suporte para este pacote apenas em máquinas que não podem usar a variante `genérica` acima (p. ex., o `linux-x86`) e somente com o tempo de execução atualizado (p. ex., Mono mais recente).

* * *

### SO específico

O pacote para SO específico, além do código gerenciado incluso no pacote genérico, também inclui código nativo para dada plataforma. Em outras palavras, o pacote para SO específico **já inclui o tempo de execução .NET Core apropriado**, que permite que você ignore inteiramente a bagunça toda da instalação e abra o ASF diretamente. O pacote para SO específico, como você pode adivinhar pelo nome, é específico para cada sistema operacional e cada um requer sua própria versão - por exemplo, o Windows requer o binário PE32 + `ArchiSteamFarm.exe`, enquanto o Linux trabalha com o binário Unix ELF `ArchiSteamFarm`. Como você deve saber, esses dois tipos não são compatíveis uns com os outros.

ASF está atualmente disponível nas seguintes variantes específicas de SO:

- `win-x64` funciona em SOs Windows 64-bit. Isso inclui Windows 7 (SP1 +), 8.1, 10, Server 2008 R2 (SP1 +), 2012, 2012 R2, 2016, bem como futuras versões.
- `Linux-arm` funciona em SOs Linux 32 bits baseados em ARM (ARMv7 +). Isso inclui especialmente os Raspberry Pi 2 & 3 com todos os SOs linux baseados em glibc disponíveis para eles, nas versões atuais e futuras. Essa variante não funcionará em arquiteturas ARM antigas, tais como ARMv6, encontrada nos Raspberry Pi 0 & 1.
- `Linux-x64` funciona em SOs Linux 64 bits baseados em glibc. Isso inclui Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES e muitos outros, incluindo seus derivados, nas versões atuais e futuras.
- `osx-x64` funciona em SOs OS X 64-bit. Isso inclui 10.12, bem como futuras versões.

Claro, mesmo que não haja um pacote de SO específico para a sua combinação de arquitetura SO, você sempre pode instalar o tempo de execução .NET Core por sua conta e rodar o pacote genérico do ASF, que também é uma das principais razões para ele existir em primeiro lugar. O pacote genérico do ASF independe de plataforma e será executado em qualquer plataforma que tenha o tempo de execução .NET Core funcional. É importante notar - o ASF requer o tempo de execução .NET Core, não um SO ou arquitetura específicos. Por exemplo, se você estiver rodando o Windows de 32 bits, apesar de não haver versão do ASF dedicado `win-x86`, você pode instalar o .NET Core SDK na versão `win-x86` e rodar o ASF genérico tranquilamente. Nós simplesmente não podemos atender a todas as combinações de arquitetura de SO que existam e são usadas por alguém, então temos que traçar um limite em algum lugar. x86 é um bom exemplo desse limite, já que é uma arquitetura obsoleta desde pelo menos 2004.

Para uma lista completa de todas as plataformas e SOs suportados pelo .NET Core 2.1, visite as **[notas de lançamento](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1-supported-os.md)**.

* * *

## Requisitos de tempo de execução

Se você estiver usando o pacote para SO específico, então você não precisa se preocupar com requisitos de tempo de execução, porque o ASF sempre acompanha o tempo de execução necessário e atualizado que funcionará corretamente, enquanto você tiver os **[pré-requisitos do .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados e atualizados. Em outras palavras, **você não precisa instalar o tempo de execução ou SDK .NET Core**, já que as compilações específicas do SO exigem apenas as dependências nativas (pré-requisitos) do sistema e nada mais.

No entanto, se você está tentando executar o pacote **genérico** do ASF, você deve garantir o que o seu tempo de execução .NET Core ofereça suporte a plataforma requerida pelo ASF.

O ASF, como programa, está visando o **.NET Core 2.1** (`netcoreapp2.1`) agora, mas ele deve visar plataformas novas no futuro. `netcoreapp2.1` é suportado desde o SDK 2.1.300 (tempo de execução 2.1.0), embora nós recomendemos usar o **[SDK mais recente](https://www.microsoft.com/net/download)** disponível para a sua máquina.

Em caso de dúvida, verifique o que nossa **[integração contínua usa](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** para compilar e implantar as versões do ASF liberadas no GitHub. Você pode encontrar a saída `dotnet --info` no topo de cada compilação.

* * *

## Problemas e soluções

### Atualização Debian Jessie

Se você atualizou do Debian 8 Jessie (ou mais antigo) para o Debian 9 Stretch, certifique-se de que você **não** tem o pacote `libssl1.0.0`, por exemplo com `apt-get purge libssl1.0.0`. Caso contrário, você pode se deparar com uma falha de segmentação. Este pacote é obsoleto e não existe por definição, também não é possível instalá-lo em instalações limpas do Debian 9, a única maneira de se deparar com esta questão é atualizando do Debian 8 ou mais antigo - **[dotnet/corefx #8951](https://github.com/dotnet/corefx/issues/8951#issuecomment-314455190)**. Se você tem outros pacotes que dependem dessa versão desatualizada da libssl, então você deve atualizá-los ou se livrar deles - não só por causa deste problema, mas também porque eles são baseados em uma biblioteca obsoleta.

### Certificados de máquina inválidos com ASF v 3.2 +

No linux (apenas), você pode ter problemas específicos relacionados com certificados SSL fornecidos por sua máquina. Isso se mostrará como a exceção abaixo:

```csharp
System.Net.Http.HttpRequestException: A conecção SSL não pode ser estabelecida, veja a exceção interna. ---> Interop+Crypto+OpenSslCryptographicException: error:2006D002:BIO routines:BIO_new_file:system lib
   em Interop.Crypto.CheckValidOpenSslHandle(SafeHandle handle)
```

Esta questão é provavelmente causada por algum dos certificados que não pode ser lido. Isto pode acontecer devido a permissões insuficientes (como um teste, que você pode verificar se a ASF funciona sob o usuário `root`) ou outras questões acerca de arquivos de certificado inválidos.

A solução atual depende da questão do root. Em instalações limpas (p. ex. do Debian), isso nunca deve acontecer já que não deve haver nenhum certificado inválido/sensível na loja, então a questão considera apenas pessoas que provavelmente tem outros certificados para outros fins (por exemplo, VPN ou sites) que o ASF não tem acesso para ler. Você pode considerar dar permissões adequadas ao usuário ASF (tais como garantir que todos os certificados SSL em `/etc/pki` e `etc/ssl` tenham pelo menos permissão de leitura global `644`), rodar o ASF na `raiz`, ou mover certificados sensíveis para algum lugar onde o ASF não tenta lê-los durante a inicialização.

Supõe-se que essa questão seja consertada com o próximo lançamento de manutenção do .NET Core 2.1.1, portanto, as soluções alternativas apresentadas acima não deverão mais serem necessárias num futuro próximo.

Referência: **[dotnet/corefx #29942](https://github.com/dotnet/corefx/issues/29942)**.

### Tempo de execução .NET Core pegando a biblioteca `libcurl.so` errada

Se você tiver tanto a `libcurl.so.3` quanto a `libcurl.so.4` no seu sistema, então o .NET Core pode decidir escolher segunda, o que levará ao travamento do ASF no momento em que ele tentar inicializar o seu cliente http.

```csharp
OnUnhandledException() System.TypeInitializationException: O inicializador padrão para 'System.Net.Http.CurlHandler' emitiu uma exceção. ---> System.TypeInitializationException: O inicializador padrão para 'Http' emitiu uma exceção. ---> System.TypeInitializationException: O inicializador padrão para 'HttpInitializer' emitiu uma exceção. ---> System.DllNotFoundException: Não pode ler a DLL 'System.Net.Http.Native': O módulo específico ou uma de suas dependências não pode ser encontrado.
```

Se você se deparar com a questão acima, então precisa informar manualmente ao tempo de execução .NET Core qual a biblioteca apropriada. Localize `libcurl.so.3` no seu sistema e adicione-o ao `LD_PRELOAD` antes de iniciar o ASF:

```shell
LD_PRELOAD=/usr/lib/libcurl.so.3 ./ArchiSteamFarm
```

Isso deve resolver o problema, supondo que sua `libcurl.so.3` esteja funcionando corretamente.