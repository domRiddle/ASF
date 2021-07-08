# Argumentos de linha de comando

O ASF inclui suporte para vários argumentos de linha de comando que podem afetar o tempo de execução do programa. Eles podem ser usados por usuários avançados para especificar como o programa deve ser executado. Comparando com o caminho usual do arquivo de configuração `ASF.json`, argumentos de linha de comando são usados para inicialização do núcleo (por exemplo, `--path`), configurações específicas de plataforma (por exemplo, `--system-required`) ou dados confidenciais (por exemplo, `--cryptkey`).

---

## Uso

O uso depende do seu sistema operacional e da versão do ASF.

Genérico:

```shell
dotnet ArchiSteamFarm.dll --argumento --segundoArgumento
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argumento --segundoArgumento
```

Linux/macOS

```shell
./ArchiSteamFarm --argumento --segundoArgumento
```

Argumentos de linha de comando também são suportados em códigos auxiliares genéricos como `ArchiSteamFarm.cmd` ou `ArchiSteamFarm.sh`. Além disso, ao usar códigos auxiliares você também pode usar a propriedade de ambiente `ASF_ARGS`, como indicado em nossa seção **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-pt-BR#argumentos-de-linha-de-comando)**.

Se seu argumento inclui espaços, não se esqueça de colocar entre aspas. Esses dois estão errados:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Não funciona!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Não funciona!
```

No entanto, esses dois estão completamente corretos:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Argumentos

`--cryptkey <key>` ou `--cryptkey=<key>` - inicializará o ASF com um valor de chave de criptografia `<key>` personalizado. Essa opção afeta a **[segurança](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-pt-BR)** e fará com que o ASF use a chave personalizada `<key>` que você fornecer ao invés da que está codificada no executável. Uma vez que essa propriedade afeta a chave de criptografia padrão (para fins de criptografia), bem como o sal (para fins de hashing), tenha em mente que tudo o que for criptografado/"hasheado" com essa chave exigirá que ela seja ativada toda vez que o ASF for executado.

Devido à natureza desta propriedade, também é possível definir a cryptkey declarando a variável de ambiente `ASF_CRYPTKEY`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

---

`--ignore-unsupported-environment` - fará com que o ASF ignore a deteção de ambiente não suportado, o que normalmente é sinalizado como erro e força o fechamento do ASF. Agora o ambiente não suportado será classificado como estar rodando a compilação .NET Framework na plataforma que pode rodar tal compilação .NET Core. Já que nós suportamos compilações `generic-netf` apenas em cenários muito limitados (com **[Mono](https://www.mono-project.com)**), usá-lo em outros casos (como no `win-x64`, por exemplo) não é suportado. Acesse a seção **[compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR)** para mais informações.

---

`--network-group <group>` ou `--network-group=<group>` - fará com que o ASF inicie seus limitadores com um grupo personalizado adicional com o valor `<group>`. Esta opção afeta a execução do ASF em **[múltiplas instâncias](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#m%C3%BAltiplas-inst%C3%A2ncias)**, sinalizando que determinada instância depende apenas do compartilhamento do mesmo grupo de rede, e não do resto. Normalmente você vai querer usar essa propriedade somente se você estiver roteando pedidos do ASF através de um mecanismo personalizado (por exemplo, endereços IP diferentes) e você deseja definir grupos de rede você mesmo, sem depender do ASF para fazer isso automaticamente (o que atualmente leva em conta apenas o `WebProxy`). Tenha em mente que ao usar um grupo de rede personalizada este identificador é exclusivo dentro da máquina local, e o ASF não levará em conta nenhum outro detalhe, como o valor do `WebProxy`, permitindo que você, por exemplo, inicie duas instâncias com diferentes valores de `WebProxy` que ainda são dependentes um do outro.

Devido à natureza desta propriedade, também é possível definir o valor declarando a variável de ambiente `ASF_NETWORK_GROUP`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

---

`--no-config-migrate` - por padrão o ASF vai migrar automaticamente sua configuração para sintaxe mais recente. A migração inclui a conversão de propriedades obsoletas para as mais recentes, exeto as propriedades com valores padrão (já que elas não têm efeito), além de limpar o arquivo de uma forma geral (corrigindo indentação e coisas do tipo). Essa é quase sempre uma boa ideia, mas você pode ter uma situação em particular onde você prefira que o ASF não substitua os arquivos de configuração automaticamente. Por exemplo, você pode querer usar `chmod 400` eu seus arquivos de configuração (permissão de leitura apenas para o proprietário) ou colocar `chattr +i` sobre ele, negando acesso de escrita para todos como uma medidad e segurança. Recomendamos manter a migração de configuração ativa, mas se você tem algum motivo para desativá-la, você pode usar esta configuração.

---

`--no-config-watch` - por padrão o ASF seta um `FileSystemWatcher` sobre a pasta `config` para monitorar quaisquer alteração nos arquivos, podento então se adaptar e essas mudanças. Por exemplo, isso inclui parar os bots caso alguma configuração seja apagada, reiniciar o bot quando a configuração for alterada, ou carregar os códigos de produto para o **[BGR](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Background-games-redeemer)** se você salvá-las na pasta `config`. Essa opção permite que você desative esse comportamento, fazendo com que o ASF ignore completamente todas as mudanças na pasta `config`, exigindo que você faça tais ações manualmente se considerar necessário. Recomendamos manter os eventos de configuração ativos, mas se você tem algum motivo para desativá-los, você pode usar esta configuração para esse fim.

---

`--no-restart` - Esta opção é usada principalmente por nossos contêineres do **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-pt-BR)** e forçam o `AutoRestart` para `false`. Unless you have a particular need, you should instead configure `AutoRestart` property directly in your config. This switch is here so our docker script won't need to touch your global config in order to adapt it to its own environment. Claro, se você estiver executando o ASF dentro de um código, você também pode fazer uso desta opção (caso contrário é melhor usar a propriedade de configuração global).

---

`--path <path>` or `--path=<path>` - o ASF sempre navega até a sua própria pasta na inicialização. Ao especificar esse argumento, o ASF navegará até a pasta selecionada ao inicializar, o que permite que você use caminhos personalizados para várias partes do aplicativo (incluindo as pastas `config`, `plugins` e `www`, assim como o arquivo `NLog.config`) sem a necessidade de duplicar o executável no mesmo lugar. Isso pode ser especialmente útil se você quiser separar os arquivos executáveis dos arquivos de configuração, como nos pacotes do Linux; desta forma, você pode usar um arquivo executável (atualizado) com várias configurações diferentes. O caminho pode tanto ser relativo de acordo com o local atual do executável do ASF, ou absoluto. Tenha em mente que este comando aponta para uma nova "pasta principal do ASF", ou seja, a pasta que tem a mesma estrutura original do ASF, com a pasta config dentro.

Devido à natureza desta propriedade, também é possível definir o caminho esperado declarando a variável de ambiente `ASF_PATH`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

Se você está pensando em usar esse argumento de linha de comando para executar várias instâncias do ASF, Recomendamos ler nossa **[página de compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#m%C3%BAltiplas-inst%C3%A2ncias)** desta maneira.

Exemplos:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Caminho absoluto
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # O caminho relativo também funciona
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # O mesmo que as variáveis de ambiente
```

```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── TargetDirectory
│           ├── config
│           ├── logs (gerado)
│           ├── plugins (opicional)
│           ├── www (opicional)
│           ├── log.txt (gerado)
│           └── NLog.config (opicional)
└── ...
```

---

`--process-required` - declarar este opção desabilitará o comportamento padrão do ASF de desligar quando nenhum bot estiver em execução. O fato dele não se desligar é especialmente útil em combinação com o **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-pt-BR)**, onde a maioria dos usuários vai querer que os serviços web continuem sendo executados independente da quantidade de bots que estejam habilitados. Se você estiver usando a opção IPC ou precisa que o processo ASF seja executado o tempo todo até que você mesmo o feche, esta é a opção certa.

Se você não pretende usar o IPC, esta opção será um tanto inútil já que você pode simplesmente iniciar o processo novamente quando necessário (ao contrário do servidor web do ASF, onde você precisa que ele esteja sempre preparado para receber comandos).

---

`--system-required` - declarar esse opção fará com que o ASF tente sinalizar para o sistema operacional que o processo precisa que o sistema continue rodando o tempo todo. Atualmente essa opção tem efeito apenas no Windows e ele previne que seu sistema entre no modo de suspensão enquanto o processo está sendo executado. This can be proven especially useful when farming on your PC or laptop during night, as ASF will be able to keep your system awake while it's farming, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once farming is finished.

Tenha em mente que para o ASF se desligar sozinho você precisa de outras configurações - especialmente evitar `--process-required` e garantir que todos os seus bots estão com `ShutdownOnFarmingFinished` habilitado. Claro, o desligamento automático é apenas uma possibilidade para este recurso e não uma exigência, já que você também pode usar esta opção juntamente com `--process-required` fazendo seu sistema ficar ativo infinitamente após iniciar o ASF.