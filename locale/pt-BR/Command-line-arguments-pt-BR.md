# Argumentos da linha de comando

ASF inclui suporte para vários argumentos de linha de comando que podem afetar o tempo de execução do programa. Isso podem ser usados por usuários avançados para especificar como o programa deve executar. Em comparação com o caminho padrão do arquivo de configuração `ASF.json`, argumentos de linha de comando são usados para inicialização do núcleo (por exemplo, `--path`), configurações específicas de plataforma (por exemplo, `--system-required`) ou dados sensíveis (por exemplo, `--cryptkey`).

* * *

## Uso

O uso depende do seu sistema operacional e gosto pessoal para o ASF.

Genérico:

```shell
dotnet ArchiSteamFarm.dll --argument --otherOne
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argument --otherOne
```

Linux/OS X:

```shell
./ArchiSteamFarm --argument --otherOne
```

Argumentos de linha de comando também são suportados em códigos auxiliares genéricos como `ArchiSteamFarm.cmd` ou `ArchiSteamFarm.sh`. Além disso, ao usar codigos de ajuda, você também pode usar a propriedade de ambiente `ASF_ARGS`, como declarou em nossa seção de **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**.

Se seu argumento inclui espaços, não se esqueça de indicar isso. Esses dois estão errados:

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
```

No entanto, esses dois estão completamente corretos:

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## Argumentos

`--cryptkey <key>` ou `--cryptkey=<key>` - começará o ASF com o valor de chave de criptografia`<key>` personalizado. Essa opção afeta a **[segurança](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** e causará ASF usar sua chave personalizada `<key>` fornecido em vez de um hardcoded padrão para o executável. Tenha em mente que as senhas criptografadas com esta chave será requerida ela a cada execução do ASF.

* * *

`--no-restart` - Esta opção é usada principalmente por nossos contêineres do **[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** e forçar `AutoRestart` para `false`. A menos que você tiver uma necessidade específica, você deve configurar a propriedade `AutoRestart` diretamente no seu adquiro de configuração. Essa opção está aqui assim que nosso código de Docker não precisará mexer em sua configuração global para adaptá-la ao seu próprio ambiente. Claro, se você estiver executando o ASF dentro de um código, você pode também fazer uso desta opção (caso contrário é melhor com a propriedade de configuração global).

* * *

`--path <path>` or `--path=<path>` - ASF sempre navega para seu próprio diretório na inicialização. By specifying this argument, ASF will navigate to given directory after initialization, which allows you to use custom path for `config` (and optionally also `www`) directory without a need of duplicating binary in the same place. Pode ser especialmente útil se você gostaria de separar o binário da configuração atual, como é feito no Linux(como pacotes) desta forma, você pode usar um binário (atualizado) com várias configurações diferentes. O caminho pode ser relativo de acordo com o local atual do binário do ASF, ou absoluto. Quando executando várias instâncias do mesmo binário, tenha em mente que você normalmente deve desativar atualizações automáticas, pois não existe nenhuma sincronização entre eles. Também tenha em mente que este comando aponta para novo "local do ASF" - o diretório que tem a mesma estrutura original ASF, com diretório `config` dentro.

Exemplo:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           └── config
    └── ...
    

* * *

`--process-required` - declarar este opção desabilitará o comportamento padrão ASF de desligar quando bots não estão sendo executados. Nenhum comportamento de autodesligamento é especialmente útil em combinação com o **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**, onde a maioria dos usuários esperaria seu serviço da web para ser executado independentemente da quantidade de bots que estão habilitados. Se você estiver usando a opção de IPC o processo ASF e preciso ser executado o tempo todo, até você fechá-lo você mesmo, esta é a opção certa.

Se você não pretende executar IPC, esta opção será um tanto inúteis para você, como você só pode iniciar o processo novamente, quando necessário (em oposição ao servidor de web do ASF onde você exige que esteja preparado para receber comandos sempre).

* * *

`--system-required` - declaração este opção causará que o ASF tente sinalizar para o sistema operacional oque o processo requer quando for executado. Atualmente essa opção tem efeito apenas em máquinas Windows onde ele vai proibir o seu sistema de entrar em modo de suspensão enquanto o processo está sendo executado. Isto pode ser provado especialmente útil quando em coleta em seu PC ou laptop durante a noite, como ASF será capaz de manter seu sistema acordado enquanto ele está em coleta, em seguida, ASF é feito, ele vai desligamento em si, como habitual, fazer seu sistema pode para entrar em modo de suspensão novamente, portanto, economia de energia imediatamente uma vez em coleta está terminado.

Tenha em mente que para autodesligamento adequado de ASF você precisa de outras configurações - especialmente evitando `--process-required` e garantindo que todos os teus bots estão a seguir `ShutdownOnFarmingFinished`. Claro, autodesligamento é apenas uma possibilidade para este recurso, não uma exigência, desde que você também pode usar esta opção juntamente com por exemplo `--process-required`, efetivamente tornando seu sistema acordado infinitamente após iniciar ASF.