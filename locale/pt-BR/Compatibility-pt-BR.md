# Compatibilidade

O ASF é um aplicativo C# que roda na plataforma .NET Core. Isso significa que o ASF não é compilado diretamente em **[código de máquina](https://pt.wikipedia.org/wiki/C%C3%B3digo_de_m%C3%A1quina)** que roda no seu CPU, mas em **[CIL](https://pt.wikipedia.org/wiki/Common_Intermediate_Language)**, que requer um tempo de execução compatível para a execução.

Esta abordagem tem uma quantidade gigantesca de vantagens, como a CIL é independente de plataforma, o ASF pode ser executado nativamente em vários Sistemas Operacionais, especialmente no Windows, Linux e OS X. Não apenas é desnecessário qualquer emulação como também há suporte para quaisquer otimizações relacionadas a plataforma ou hardware, tais como instruções CPU SSE. Graças a isso, o ASF pode alcançar desempenho e otimização superiores, enquanto continua a oferecer compatibilidade e confiabilidade perfeitos.

Isto também significa que o ASF **não tem nenhum requisito específico quanto ao Sistema Operacional**, porque ele exige um **tempo de execução** funcional no Sistema Operacional e não o Sistema Operacional em si. Desde que o tempo de execução execute o código do ASF corretamente, não importa se o Sistema Operacional é Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii ou a sua torradeira: uma vez que rode o **[.NET Core](https://github.com/dotnet/core-setup#daily-builds)**, roda o **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**.

No entanto, independente de onde você executar o ASF, você deve garantir que sua plataforma de destino tenha os requisitos para o **[.NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados. Essas são bibliotecas de baixo nível necessárias para a funcionalidade adequada do tempo de execução e são primordiais para o funcionamento do ASF. É bem possível que você já tenha algumas delas (ou mesmo todas) já instaladas.

* * *

## Múltiplas instâncias

O ASF permite a execução de múltiplas instâncias do processo no mesmo computador. Essas instâncias podem ser totalmente individuais ou derivadas do mesmo executável (nesse caso você deve rodá-las com o **[argumento de linha de comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments)** `--path` diferente para cada uma).

Quando estiver executando várias instâncias do mesmo executável certifique-se que você desativou as atualizações automáticas em todas as configurações, pois não existe nenhuma sincronização entre eles no que tange as atualizações automáticas. Se você quiser continuar com as atualizações automáticas habilitadas nós recomendamos rodar instâncias individuais, mas você ainda pode fazer as atualizações funcionarem, contanto que você possa garantir que todas as outras instâncias do ASF sejam fechadas.

O ASF fará o seu melhor para manter uma quantidade mínima de comunicação entre Sistema Operacional e multi-processos com outras instâncias do ASF. Isso inclui verificar sua pasta de configuração ao invés da pasta de outras instâncias, além de compartilhar limitadores de núcleo de processo configurados com a **[propriedade de configuração global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-pt-BR#configura%C3%A7%C3%A3o-global)** `*LimiterDelay` garantindo que rodar várias instâncias do ASF não cause a possibilidade de ter problemas com limite de tráfego. No que diz respeito a aspectos técnicos, todas as plataformas usam nosso mecanismo dedicado de bloqueios baseado em arquivos personalizados do ASF criados em uma pasta temporária em `C:\Users\<SeuUsuário>\AppData\Local\Temp\ASF` no Windows e `/tmp/ASF` no Unix.

Não é necessário que instâncias do ASF compartilhem as mesmas propriedades `*LimiterDelay`, elas podem usar valores diferentes já que cada ASF adicionará seu próprio atraso configurado para o tempo de liberação após adquirir o bloqueio. Se a configuração `*LimiterDelay` estiver definida como `0`, a instância do ASF vai pular a espera pelo bloqueio de determinado recurso que for compartilhado com outras instâncias (isso poderia manter um bloqueio compartilhado uns com os outros). Quando definido como outro valor, o ASF irá sincronizar corretamente com outras instâncias e esperar pela sua vez e liberar o bloqueio após o atraso configurado, permitindo que outras instâncias continuem.

O ASF leva em conta a configuração `WebProxy` quando decide sobre o escopo compartilhado, o que significa que duas instâncias do ASF usando configurações `WebProxy` diferentes não compartilharão seus limitadores entre si. Isso foi implementado para permitir que as configurações de `WebProxy` operem sem atrasos excessivos, como esperado de diferentes interfaces de rede. Isso deve ser o suficiente para a maioria dos casos, no entanto, se você tiver uma configuração personalizada específica na qual você roteie os pedidos de forma diferente, por exemplo, você pode especificar manualmente o grup de rede atráves do **[argumento de linha de comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-pt-BR)** `--network-group`, o que vai permitir que você declare o grupo do ASF que será sincronizado com essa instância. Tenha em mente que grupos de rede personalizados são exclusividades, o que significa que o ASF não vai mais usar o `WebProxy` para determinar o grupo certo, já que você é responsável pelo agrupamento nesse caso.

* * *

## Empacotamento do ASF

O ASF está disponível em 2 formas diferentes: pacote genérico e pacote específico para Sistema Operacional. Funcionalmente ambos os pacotes são exatamente o mesmo e ambos são capazes de se atualizarem automaticamente. A única diferença entre eles é se o pacote **genérico** do ASF acompanha ou não o tempo de execução **específico do Sistema Operacional** para rodar o mesmo.

* * *

### Genérico

O pacote genérico não depende de plataforma de compilação e não inclui nenhum código de máquina específico. Esta instalação requer que você tenha o tempo de execução .NET Core, **na versão apropriada**, já instalada no seu Sistema Operacional. Nós todos sabemos como é complicado manter dependências atualizadas, portanto este pacote existe principalmente para pessoas que **já usam o** .NET Core e não querem duplicar seu tempo de execução unicamente para o ASF, já que podem fazer uso do que eles já tem instalado. O pacote genérico também permite que você rode o ASF **em qualquer lugar onde você possa obter uma implementação funcional do tempo de execução .NET Core**, independente de existir ou não uma compilação do ASF específica para a SO.

Não se recomenda usar o tipo genérico se você quer apenas fazer o ASF funcionar, sendo um usuário casual ou avançado, e não quer entrar em detalhes técnicos do .NET Core. Em outras palavras: se você sabe o que ele é, você pode usá-lo, caso contrário é muito melhor usar o pacote específico para o seu Sistema Operacional, que será explicado abaixo.

#### Pacote do .NET Framework

Além do pacote genérico mencionado acima, há também um pacote `netf-genérico` que é construído em cima do .NET Framework (e não do .NET Core). Esse pacote é uma variante herdada que fornece compatibilidades que faltavam nos tempos do ASF V2, e pode rodar, por exemplo, com o **[Mono](https://www.mono-project.com)**, enquanto o pacote `genérico` .NET Core já não pode.

Em geral, você deve **evitar este pacote o quanto for possível**, já que a maioria dos sistemas operacionais e configurações são perfeitamente (e muito mais) compatíveis com o pacote `genérico` mencionado acima. Na verdade, só faz sentido usar esse pacote em plataformas que não tenham o tempo de execução .NET Core, e tenham uma implementação funcional do Mono. Exemplos de tais plataformas incluem `linux-x86` (linux i386/i686 32-bit), bem como `linux-armel` (placas ARMv6 encontradas, p. ex., no Raspberry Pi 0 & 1), todas elas que não tem um tempo de execução oficial .NET Core até hoje.

Conforme o tempo passa, com mais plataformas sendo suportadas pelo .NET Core, e com compatibilidade entre o .NET Framework e o .NET Core diminuindo, o pacote `genérico-netf` será inteiramente substituído pelo `genérico`. Por favor, evite usá-lo se você pode usar qualquer pacote do .NET Core em seu lugar, já que exite um monte de funcionalidades e compatibilidades faltantes no pacote `genérico-netf` em comparação com as versões .NET Core, e ele será cada vez menos funcional com o passar do tempo. Oferecemos suporte para este pacote **apenas** em máquinas que não podem usar a variante `genérica` acima (por exemplo, o `linux-x86`) e somente com o tempo de execução atualizado (por exemplo, o Mono mais recente).

* * *

### Sistema Operacional específico

O pacote para Sistema Operacional específico, além do código gerenciado incluso no pacote genérico, também inclui código nativo para dada plataforma. Em outras palavras, o pacote para Sistema Operacional específico **já inclui o tempo de execução .NET Core apropriado**, que permite que você ignore inteiramente a bagunça toda da instalação e abra o ASF diretamente. O pacote para Sistema Operacional específico, como você pode adivinhar pelo nome, é específico para cada sistema operacional e cada um requer sua própria versão; por exemplo, o Windows requer o binário PE32 + `ArchiSteamFarm.exe`, enquanto o Linux trabalha com o binário Unix ELF `ArchiSteamFarm`. Como você deve saber, esses dois tipos não são compatíveis um com o outro.

ASF está atualmente disponível nas seguintes variantes específicas de Sistema Operacional:

- `win-x64` funciona em Sistemas Operacionais Windows 64-bit. Isso inclui Windows 7 (SP1 +), 8.1, 10, Server 2008 R2 (SP1 +), 2012, 2012 R2, 2016, bem como futuras versões.
- `Linux-arm` funciona em Sistemas Operacionais GNU/Linux 32 bits baseados em ARM (ARMv7 +). Isso inclui plataformas como o Raspberry Pi 2 (e mais recentes) com todos os Sistemas Operacionais GNU/Linux disponíveis para eles (como o Raspian), nas versões atuais e futuras. Essa variante não vai funcionar com arquiteturas ARM antigas, como a ARMv6 encontrada no Raspberry Pi 0 & 1, também não vai funcionar com SOs que não implementem o ambiente GNU/Linux requerido (tal qual o Android).
- `linux-arm64` funciona em Sistemas Operacionais GNU/Linux 64 bits baseados em ARM (ARMv8). Isso inclui plataformas como o Raspberry Pi 3 (e mais recentes) com todos os Sistemas Operacionais GNU/Linux AArch64 disponíveis para eles (como o Debian), nas versões atuais e futuras. Esta variante não funcionará com Sistemas Operacionais de 32 bits que não tenham bibliotecas de 64 bits disponíveis (como o Raspbian), também não funcionará com Sistemas Operacionais que não implementem o ambiente GNU/Linux necessário (como o Android).
- `Linux-x64` funciona em Sistemas Operacionais GNU/Linux 64 bits. Isso inclui Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES e muitos outros, incluindo seus derivados, nas versões atuais e futuras.
- `osx-x64` funciona em Sistemas Operacionais OS X 64-bit. Isso inclui o 10.13, bem como futuras versões.

Claro, mesmo que não haja um pacote de Sistema Operacional específico para a sua combinação de arquitetura de Sistema, você sempre pode instalar o tempo de execução .NET Core por sua conta e rodar o pacote genérico do ASF, que também é uma das principais razões para ele existir em primeiro lugar. O pacote genérico do ASF não depende de plataforma e será executado em qualquer plataforma que tenha o tempo de execução .NET Core funcional. Há algo importante de se notar: o ASF requer o tempo de execução .NET Core, não um Sistema Operacional ou arquitetura específicos. Por exemplo, se você estiver rodando o Windows de 32 bits, apesar de não haver versão do ASF dedicado `win-x86`, você pode instalar o .NET Core SDK na versão `win-x86` e rodar o ASF genérico tranquilamente. Nós simplesmente não podemos atender a todas as combinações de arquitetura de Sistemas Operacionais que existam e são usadas por alguém, então temos que traçar um limite em algum lugar. O x86 é um bom exemplo desse limite, já que é uma arquitetura obsoleta desde pelo menos 2004.

Para uma lista completa de todas as plataformas e Sistemas Operacionais suportados pelo .NET Core 3.1, visite as **[notas de lançamento](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md)**.

* * *

## Requisitos do tempo de execução

Se você estiver usando um pacote para Sistema Operacional específico você não precisa se preocupar com requisitos de tempo de execução porque o ASF sempre acompanha o tempo de execução necessário e atualizado que funcionará corretamente, enquanto você tiver os **[pré-requisitos do .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados e atualizados. Em outras palavras, **você não precisa instalar o tempo de execução ou SDK .NET Core**, já que as compilações específicas de Sistema Operacional exigem apenas as dependências nativas (pré-requisitos) do sistema e nada mais.

No entanto, se você está tentando executar o pacote **genérico** do ASF, você deve garantir o que o seu tempo de execução .NET Core ofereça suporte a plataforma requerida pelo ASF.

O ASF, como programa, utiliza o **.NET Core 3.1** (`netcoreapp3.1`) agora, mas ele deve utilizar plataformas mais novas no futuro. O `netcoreapp3.1` é suportado desde o SDK 3.1.100 (tempo de execução 3.1.0), porém o ASF é configurado para buscar o **tempo de execução mais recente na hora da compilação**, então você deve garantir que você tem a **[SDK mais recente](https://dotnet.microsoft.com/download)** (ou ao menos o tempo de execução) disponível para o seu computador. A variante genéria do ASF pode se recusar a iniciar se o seu tempo de execução for mais antigo que o utilizado durante a compilação.

Em caso de dúvida, verifique o que nossa **[integração contínua usa](https://ci.appveyor.com/project/JustArchi/ArchiSteamFarm)** para compilar e implantar as versões do ASF liberadas no GitHub. Você pode encontrar a saída `dotnet --info` no topo de cada compilação.