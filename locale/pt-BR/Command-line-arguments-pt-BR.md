# Argumentos de linha de comando

O ASF inclui suporte para vários argumentos de linha de comando que podem afetar o tempo de execução do programa. Eles podem ser usados por usuários avançados para especificar como o programa deve ser executado. Comparando com o caminho usual do arquivo de configuração `ASF.json`, argumentos de linha de comando são usados para inicialização do núcleo (por exemplo, `--path`), configurações específicas de plataforma (por exemplo, `--system-required`) ou dados confidenciais (por exemplo, `--cryptkey`).

* * *

## Uso

O uso depende do seu sistema operacional e da versão do ASF.

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

`--cryptkey <key>` ou `--cryptkey=<key>` - inicializará o ASF com um valor de chave de criptografia `<key>` personalizado. Essa opção afeta a **[segurança](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-pt-BR)** e fará com que o ASF use sua chave personalizada `<key>` fornecida ao invés da que está codificada no executável. Tenha em mente que as senhas criptografadas com esta chave serão requeridas toda vez que o ASF for executado.

Devido à natureza desta propriedade, também é possível definir a cryptkey declarando a variável de ambiente `ASF_CRYPTKEY`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

* * *

`--network-group <group>` ou `--network-group=<group>` - fará com que o ASF inicie seus limitadores com um grupo personalizado adicional com o valor `<group>`. Esta opção afeta a execução do ASF em **[multiplas instâncias](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#m%C3%BAltiplas-inst%C3%A2ncias)**, sinalizando que determinada instância depende apenas do compartilhamento do mesmo grupo de rede, e não do resto. Normalmente você vai querer usar essa propriedade somente se você estiver roteando pedidos do ASF através de um mecanismo personalizado (por exemplo, endereços IP diferentes) e você deseja definir grupos de rede você mesmo, sem depender do ASF para fazer isso automaticamente (o que atualmente leva em conta apenas o `WebProxy`). Tenha em mente que ao usar um grupo de rede personalizada este identificador é exclusivo dentro da máquina local, e o ASF não levará em conta nenhum outro detalhe, como o valor do `WebProxy`, permitindo que você, por exemplo, inicie duas instâncias com diferentes valores de `WebProxy` que ainda são dependentes um do outro.

Devido à natureza desta propriedade, também é possível definir o valor declarando a variável de ambiente `ASF_NETWORK_GROUP`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

* * *

`--no-restart` - Esta opção é usada principalmente por nossos contêineres do **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-pt-BR)** e forçam o `AutoRestart` para `false`. A menos que você tenha uma necessidade específica, você deve configurar a propriedade `AutoRestart` diretamente no seu adquivo de configuração. Essa opção está aqui para que nosso código docker não precise mexer em sua configuração global para adaptá-la ao seu próprio ambiente. Claro, se você estiver executando o ASF dentro de um código, você também pode fazer uso desta opção (caso contrário é melhor usar a propriedade de configuração global).

* * *

`--path <path>` or `--path=<path>` - o ASF sempre navega até a sua própria pasta na inicialização. Ao especificar esse argumento, o ASF navegará até a pasta selecionada ao inicializar, o que permite que você use caminhos personalizados para várias partes do aplicativo (incluindo as pastas `config`, `plugins` e `www`, assim como o arquivo `NLog.config`) sem a necessidade de duplicar o executável no mesmo lugar. Isso pode ser especialmente útil se você quiser separar os arquivos executáveis dos arquivos de configuração, como nos pacotes do Linux; desta forma, você pode usar um arquivo executável (atualizado) com várias configurações diferentes. O caminho pode tanto ser relativo de acordo com o local atual do executável do ASF, ou absoluto. Tenha em mente que este comando aponta para uma nova "pasta principal do ASF", ou seja, a pasta que tem a mesma estrutura original do ASF, com a pasta config dentro.

Devido à natureza desta propriedade, também é possível definir o caminho esperado declarando a variável de ambiente `ASF_PATH`, que pode ser mais apropriada para pessoas que gostariam de evitar dados confidenciais nos argumentos do processo.

Se você está pensando em usar esse argumento de linha de comando para executar várias instâncias do ASF, Recomendamos ler nossa **[página de compatibilidade](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-pt-BR#m%C3%BAltiplas-inst%C3%A2ncias)** desta maneira.

Exemplos:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Caminho absoluto
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # O caminho relativo também funciona
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # O mesmo que as variáveis de ambiente
```

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
    

* * *

`--process-required` - declarar este opção desabilitará o comportamento padrão do ASF de desligar quando nenhum bot estiver em execução. O fato dele não se desligar é especialmente útil em combinação com o **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-pt-BR)**, onde a maioria dos usuários vai querer que os serviços web continuem sendo executados independente da quantidade de bots que estejam habilitados. Se você estiver usando a opção IPC ou precisa que o processo ASF seja executado o tempo todo até que você mesmo o feche, esta é a opção certa.

Se você não pretende usar o IPC, esta opção será um tanto inútil já que você pode simplesmente iniciar o processo novamente quando necessário (ao contrário do servidor web do ASF, onde você precisa que ele esteja sempre preparado para receber comandos).

* * *

`--system-required` - declarar esse opção fará com que o ASF tente sinalizar para o sistema operacional que o processo precisa que o sistema continue rodando o tempo todo. Atualmente essa opção tem efeito apenas no Windows e ele previne que seu sistema entre no modo de suspensão enquanto o processo está sendo executado. Isto pode ser especialmente útil quando você deixar seu PC ou notebook coletando durante a noite, pois o ASF será capaz de manter seu sistema ativo enquanto ele está em coleta, então quando o ASF terminar ele vai se desligar como de costume e permitir que seu sistema entre modo de suspensão novamente, economizando energia imediatamente após o termino da coleta.

Tenha em mente que para o ASF se desligar sozinho você precisa de outras configurações - especialmente evitar `--process-required` e garantir que todos os seus bots estão com `ShutdownOnFarmingFinished` habilitado. Claro, o desligamento automático é apenas uma possibilidade para este recurso e não uma exigência, já que você também pode usar esta opção juntamente com `--process-required` fazendo seu sistema ficar ativo infinitamente após iniciar o ASF.