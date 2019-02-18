# Autenticador em duas etapas

Há um tempo atrás a Valve introduziu um sistema conhecido como "Escrow" que requer uma autenticação extra para várias atividades relacionadas a conta. Você pode ler mais sobre isso **[aqui](https://support.steampowered.com/kb_article.php?ref=1284-WTKB-4729)** e **[aqui](https://support.steampowered.com/kb_article.php?ref=8078-TPHC-6195)**. É crucial primeiro entender o sistema 2FA antes de tentar entender a lógica por trás do ASF 2FA.

Como você pode ver, todas as trocas são retidas por até 15 dias, o que não é um grande problema quando se trata do ASF, mas ainda pode ser irritante, especialmente para aqueles que querem uma automação completa. Felizmente, o ASF inclui uma solução para esse problema, chamada ASF 2FA.

* * *

# Lógica do ASF

Independentemente de você usar o ASF 2FA explicado abaixo ou não, o ASF inclui lógica adequada e está plenamente consciente das contas protegidas pelo 2FA padrão. Ele vai te pedir pelos dados necessários quando for preciso (durante o login, por exemplo). Se você usar o ASF 2FA, o programa será capaz de ignorar esses pedidos e gerar os tokens necessários, poupando-lhe aborrecimento e permitindo funcionalidades extras (descritas abaixo).

* * *

# ASF 2FA

O ASF 2FA é um módulo embutido responsável por prover as funcionalidades do 2FA no processo do ASF, tal como gerar tokens e aceitar confirmações. Ele duplica seu autenticador existente, então não há nenhuma necessidade de usar apenas o ASF 2FA.

Você pode verificar se sua conta bot já usa o ASF 2FA executando o **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-pt-BR) **`2fa`. A menos que você já tenha importado seu autenticador para o ASF 2FA, todos os comandos `2fa` não funcionarão, o que significa que sua conta não está usando o ASF 2FA e que não é possível usar as funcionalidades avançadas do ASF que requerem o módulo operante.

Para habilitar o 2FA ASF, você precisa ter:

- Um autenticador steam ativo no seu Android
- ou um autenticador steam ativo no seu iOS
- ou um autenticador steam ativo no **[SteamDesktopAuthenticator](https://github.com/Jessecar96/SteamDesktopAuthenticator)**
- ou um autenticador steam ativo no **[WinAuth](https://winauth.github.io/winauth)**
- ou qualquer outra implementação funcional do autenticador Steam com acesso ao segredo compartilhado/segredo de identificação e o ID do dispositivo

* * *

## Importar

In order to complete the steps explained below, you should have already linked and operational authenticator that is supported by ASF. ASF currently supports a few different sources of 2FA - Android, iOS, SteamDesktopAuthenticator and WinAuth. If you don't have any authenticator yet, you need to choose one of those and set it up firstly. If you don't know better which one to pick, we recommend WinAuth, but any of the above will work fine assuming you follow the instructions.

Todos os guias a seguir exigem que você já tenha um autenticador **funcionando e operacional** sendo usado com dada ferramenta/aplicativo. ASF 2FA não funcionará corretamente se você importar dados inválidos, portanto, tenha certeza de que seu autenticador funciona corretamente antes de tentar importá-lo. Isso inclui testar e verificar que as seguintes funções de autenticador funcionam corretamente:

- Você consegue gerar tokens e esses tokens são aceitos pela Steam
- Você pode buscar confirmações e elas estão chegando no seu autenticador móvel
- Você pode aceitar essas confirmações e elas são devidamente reconhecidas pela Steam como confirmadas/rejeitadas

Certifique-se de que seu autenticador funciona verificando se as ações acima funcionam - se não funcionarem, então elas também não funcionaram no ASF, você só vai perder tempo e te causar problemas.

* * *

### Android

Em geral, para importar o autenticador do seu Android você vai precisar de acesso ao **[root](https://en.wikipedia.org/wiki/Rooting_(Android_OS))**. Fazer o root varia de um dispositivo pra outro, então eu não vou te dizer como fazer no seu aparelho. Visit **[XDA](https://www.xda-developers.com/root)** for excellent guides on how to do that, as well as general information on rooting in general. If you can't find your device or the guide that you need, try to find it on google second.

At least officially, it's not possible to access protected Steam files without root. The only official non-root method for extracting Steam files is creating unencrypted `/data` backup in one way or another and manually fetching appropriate files from it on your PC, however because such thing highly depends on your phone manufacturer and **is not** in Android standard, we won't discuss it here. Se você tem sorte de ter essa funcionalidade, você pode fazer uso dela, mas a maioria dos usuários não tem algo assim.

Unofficially, it is possible to extract the needed files without root access, by installing or downgrading your Steam app to version 2.1 (or earlier), setting up mobile authenticator and then creating a snapshot of the app (together with the `data` files that we need) through `adb backup`. However, since it's a serious security breach and entirely unsupported way to extract the files, we won't elaborate further on this, Valve disabled this security hole in newer versions for a reason, and we only mention it as a possibility.

Assuming that you've successfully rooted your phone, you should afterwards download any root explorer available on the market, such as **[this one](https://play.google.com/store/apps/details?id=com.jrummy.root.browserfree)** (or any other one of your preference). You can also access the protected files through ADB (Android Debug Bridge) or any other available to you method, we'll do it through the explorer since it's definitely the most user-friendly way.

Once you opened your root explorer, navigate to `/data/data` folder. Tenha em mente que o diretório `/data/data` é protegido e você não será capaz de acessá-lo sem acesso ao root. Once there, find `com.valvesoftware.android.steam.community` folder and copy it to your `/sdcard`, which points to your built-in internal storage. Afterwards, you should be able to plug your phone to your PC and copy the folder from your internal storage like usual. If by any chance the folder won't be visible despite you being sure that you copied it to the right place, try restarting your phone first.

Agora, você deve escolher se quer importar seu autenticador para o WinAuth primeiro e então para o ASF ou diretamente para o ASF. A primeira opção é mais amigável e permite duplicar seu autenticador para o seu PC, permitindo que você faça confirmações e gere tokens de 3 lugares diferentes - seu celular, seu PC e o ASF. If you want to do that, simply open WinAuth, add new Steam authenticator and choose importing from Android option, then follow instructions by accessing the files that you've obtained above. When done, you can then import this authenticator from WinAuth to ASF, which is explained in dedicated WinAuth section below.

If you don't want to or don't need to go through WinAuth, then simply copy `files/Steamguard-SteamID` file from our protected directory, where `SteamID` is your 64-bit Steam identificator of the account that you want to add (if more than one, because if you have only one account then this will be the only file). You need to place that file in ASF's `config` directory. Once you do that, rename the file to `BotName.maFile`, where `BotName` is the name of your bot you're adding ASF 2FA to. Após esta etapa, abra o ASF - ele vai reconhecer o `.maFile` e vai importá-lo.

    [*] INFO: ImportAuthenticator() <1> Convertendo o arquivo .maFile para o formato ASF...
    <1> Por favor, insira o ID do seu dispositivo de autenticador móvel (incluindo "android:"):
    

You will need to do only one more step - find your `DeviceID` property in `shared_prefs/steam.uuid.xml`. Ele fica entre tags XML e começa com `android:`. Copy that (or write it down) and put it in ASF as asked. Se você fez tudo corretamente, a importação deve terminar.

    [*] INFO: ImportAuthenticator() <1> Verificação do autenticador móvel concluída com sucesso!
    

Por favor, verifique se aceitar confirmações, de fato, funciona. Se você cometeu um erro ao entrar com seu `DeviceID` então você terá um autenticador incompleto - os tokens funcionarão, mas você não conseguirá aceitar as confirmações. Você sempre pode remover o arquivo `Bot.db` e recomeçar, se necessário.

* * *

### iOS

Para iOS você pode usar o **[ios-steamguard-extractor](https://github.com/CaitSith2/ios-steamguard-extractor)**. Isso é possível graças ao fato de que você pode fazer backups descodificados, colocá-los em seu PC e usar a ferramenta para extrair dados da Steam que seriam impossíveis de se obter de outra forma (ao menos sem desbloqueio, dado a codificação do iOS).

Vá até o **[último lançamento](https://github.com/CaitSith2/ios-steamguard-extractor/releases/latest)** para baixar o programa. Depois que você extrair os dados você pode colocá-los, por exemplo, no WinAuth, em seguida, do WinAuth para o ASF (embora você também possa simplesmente copiar o código json gerado, começando no `{` até o `}` em um arquivo `NomeDoBot.maFile` e prosseguir como de costume). Se você me perguntar, eu recomendo fortemente importar para o WinAuth primeiro, depois certificar-se que tanto gerar tokens como aceitar confirmações estão funcionando corretamente, assim você pode ter certeza que está tudo bem. Se suas credenciais forem inválidas, ASF 2FA não vai funcionar direito, então é muito melhor deixar o passo de importar para o ASF por último.

Para perguntas/problemas, por favor visite **[problemas](https://github.com/CaitSith2/ios-steamguard-extractor/issues)**.

*Tenha em mente que a ferramenta acima não é oficial, você está usando por sua conta e risco. Nós não oferecemos suporte técnico se ela não funcionar corretamente - temos alguns indícios de que ela esteja exportando credenciais 2FA inválidas - verifique se as confirmações funcionam no autenticador, tal qual o WinAuth, antes de importar esses dados para o ASF!*

* * *

### SteamDesktopAuthenticator

Se você estiver usando seu autenticador no SDA, você deve ter notado que há um arquivo chamado `steamID.maFile` na pasta `maFiles`. Copie esse arquivo para a pasta `config` do ASF. Certifique-se de que `.maFile` está em formato não-criptografado, pois o ASF não pode descriptografar arquivos SDA - um arquivo com conteúdo descriptografado deve começar com o caractere `{`.

Agora você deve renomear o arquivo `steamID.maFile` para `NomeDoBot.maFile` na pasta config do ASF, onde `NomeDoBot` é o nome do bot para o qual você está adicionando o ASF 2FA. Como alternativa, você pode deixá-lo como está, o ASF vai selecioná-lo automaticamente após o login. Ajudar o ASF torna possível usar o ASF 2FA antes de fazer o login, se você não ajudar o ASF, então o arquivo poderá ser selecionado apenas depois que o ASF logar com sucesso (já que o ASF não sabe qual a `steamID` da sua conta antes de realmente logar).

Se você fez tudo corretamente, abra o ASF e você deverá notar:

    [*] INFO: ImportAuthenticator() <1> Convertendo o arquivo .maFile para o formato ASF...
    [*] INFO: ImportAuthenticator() <1> Verificação do autenticador móvel concluída com sucesso!
    

De agora em diante, seu ASF 2FA deverá estar operacional para esta conta.

* * *

### WinAuth

Em primeiro lugar, crie um arquivo novo com o nome `NomeDoBot.maFile` na pasta config do ASF, onde `NomeDoBot` é o nome do bot para o qual você está adicionando o ASF 2FA. Lembre-se que ele deve ser `NomeDoBot.maFile` e não `NomeDoBot.maFile.txt`, o Windows gosta de ocultar extensões conhecidas de arquivos por padrão. Se você colocar o nome errado, o arquivo não será selecionado pelo ASF.

Agora inicie o Winauth como de costume. Clique com o botão direito no ícone da steam e selecione "Show SteamGuard and Recovery Code". Então marque "Allow copy". Você vai observar uma estrutura JSON familiar para você na parte inferior da janela, começando com `{`. Copie todo o texto para o arquivo `NomeDoBot.maFile` que você criou na etapa anterior.

Se você fez tudo corretamente, abra o ASF e você deverá notar:

    [*] INFO: ImportAuthenticator() <1> Convertendo o arquivo .maFile para o formato ASF...
    <1> Por favor, insira o ID do seu dispositivo de autenticador móvel (incluindo "android:"):
    

Agora vem a parte complicada. O WinAuth não tem a propriedade deviceID, que é necessária para o ASF, então você deverá fazer mais uma coisa.

Volte até a opção "Show SteamGuard and Recovery Code" no WinAuth e você deverá notar a propriedade "Device ID" acima do código JSON que você copiou não faz muito tempo. Copie o ID do dispositivo android completo, incluindo a parte `android:` para o ASF.

Se você também fez isso corretamente, você terminou!

    [*] INFO: ImportAuthenticator() <1> Verificação do autenticador móvel concluída com sucesso!
    

Por favor, verifique se aceitar confirmações, de fato, funciona. Se você cometeu um erro ao entrar com seu `DeviceID` então você terá um autenticador incompleto - os tokens funcionarão, mas você não conseguirá aceitar as confirmações. Você sempre pode remover o arquivo `Bot.db` e recomeçar, se necessário.

* * *

## Pronto

From this moment, all `2fa` commands will work as they'd be called on your classic 2FA device. Você pode usar tanto o ASF 2FA quanto o seu autenticador de escolha (Android, iOS, SDA ou WinAuth) para gerar tokens e aceitar as confirmações.

Se você tem um autenticador em seu telefone, você pode, opcionalmente, remover o SteamDesktopAuthenticator e/ou o WinAuth, já que não precisaremos mais deles. No entanto, eu sugiro que você os mantenha para o caso de precisar, para não falar que eles são mais acessíveis que o autenticador normal da steam. Apenas tenha em mente que o ASF 2FA **NÃO** tem a finalidade geral de ser um autenticador e **nunca** deve ser o único que você utiliza, já que ele nem mesmo inclui todos os dados que um autenticador deve ter. Não é possível converter o ASF 2FA de volta ao autenticador original, portanto certifique-se sempre que você tem um autenticador de uso geral em outro lugar, como o WinAuth/SDA, ou no seu telefone.

* * *

## Perguntas frequentes (FAQ)

### Como o ASF faz uso do módulo 2FA?

Se o ASF 2FA estiver disponível, o ASF o usará para confirmação automática de transações que estão sendo enviadas/aceitas pelo ASF. Ele também será capaz de gerar automaticamente tokens 2FA conforme a necessidade, por exemplo para logar. Além disso, o ASF 2FA permite comandos `2fa` para você usar. Isso deve ser tudo por enquanto, se eu não esqueci de nada - basicamente o ASF usa o módulo 2FA conforme necessário.

* * *

### E se eu precisar de um token 2FA?

Você vai precisar de um token 2FA para acessar uma conta protegida pelo 2FA, que também inclui todas as contas com ASF 2FA. Você deve gerar tokens no autenticador que você usou para a importação, mas você também pode gerar tokens temporários através do comando `2fa` enviado via chat pelo bot escolhido. Você também pode usar o comando `2fa <BotNames>` para gerar um token temporário para a conta bot mencionada. Isto deve ser suficiente para você acessar contas bot através, por exemplo, do navegador, mas como mencionado acima - você deve usar seu autenticador de costume (Android, iOS, SDA ou WinAuth) em vez disso.

* * *

### Posso usar meu autenticador original depois de importá-lo como ASF 2FA?

Sim, seu autenticador original continua funcional e você pode usá-lo juntamente com o ASF 2FA. Essa é a questão toda do processo - nós estamos importando suas credenciais do autenticador para dentro do ASF, então o ASF pode usá-las e aceitar confirmações selecionadas em seu interesse.

* * *

### Onde fica salvo o autenticador móvel do ASF?

O autenticador móvel do ASF é salvo no arquivo `NomeDoBot.db` na sua pasta config, juntamente com outros dados cruciais relacionados a conta em questão. Se você deseja remover o ASF 2FA, leia como abaixo.

* * *

### Como remover o ASF 2FA?

Simplesmente pare o ASF e remova o arquivo `NomeDoBot.db` do bot com o ASF 2FA vinculado que deseja remover. Esta opção irá remover o 2FA associado importado para o ASF, mas NÃO desvinculará seu autenticador. Se ao invés disso você quiser desvincular seu autenticador, além de removê-lo do ASF (em primeiro lugar), você deve desvinculá-lo no autenticador de sua escolha (Android, iOS, SDA ou WinAuth), ou - se por alguma razão você não puder, use o código de revogação que recebeu durante a vinculação com o autenticador, no site da Steam. It's not possible to unlink your authenticator through ASF, this is what general-purpose authenticator that you already have should be used for.

* * *

### Eu vinculei o autenticador ao SDA/WinAuth, em seguida, importei para o ASF. Can I now unlink it and link it again on my phone?

**Não**. O ASF **importa** os dados do autenticador para usá-lo. Se você desvincular seu autenticador então você também fará com que o ASF 2FA pare de funcionar, independentemente de você tê-lo removido primeiro como referido na pergunta acima ou não. Se você quiser usar seu autenticador tanto em seu telefone quanto no ASF (e mais, opcionalmente, no SDA/WinAuth), então você precisa **importar** seu autenticador do seu telefone e não criar um novo no SDA/WinAuth. Você só pode ter **um** autenticador vinculado, é por isso que ASF **importa** esse autenticador e seus dados para usá-lo como ASF 2FA - é **o mesmo** autenticador, apenas existindo em dois lugares. Se você decidir desvincular suas credenciais no autenticador móvel - independentemente de qual modo, o ASF 2FA irá parar de funcionar, uma vez que as credenciais do autenticador móvel copiadas anteriormente deixarão de ser válidas. Para utilizar o ASF 2FA juntamente com o autenticador em seu telefone, você deve importá-lo do Android/iOS, como é descrito acima.

* * *

### Usar o ASF 2FA é melhor que o WinAuth/SDA/Outros configurados para aceitar todas as confirmações?

**Sim**, por vários motivos. Primeiro e mais importante - usar o ASF 2FA **significantemente** aumenta sua segurança, uma vez que o módulo 2FA do ASF assegura que o ASF só aceitará automaticamente suas próprias confirmações, então mesmo que um atacante solicite uma troca prejudicial, o ASF 2FA **não** aceitará tal troca, já que ela não foi gerada pelo ASF. Adicionalmente a questão de segurança, usar o ASF 2Fa também traz benefícios de performance/otimização, uma vez que ele busca e aceita confirmações imediatamente após serem geradas, e só nessa hora, em vez do método ineficiente de sondagem para confirmações a cada X minutos feitos, por exemplo, pelo SDA ou WinAuth. Resumindo, não há necessidade de se usar outros autenticadores além do ASF 2FA, se você planeja usar confirmações automatizadas geradas pelo ASF - isso é exatamente o que o ASF 2FA faz, e usá-lo não entra em conflito com você confirmando tudo o mais no autenticador de sua escolha. Recomendamos fortemente a utilização do ASF 2FA para toda atividade do ASF - isto é muito mais seguro do que qualquer outra solução.

* * *

## Avançado

Se você for um usuário avançado, você pode gerar o arquivo maFile manualmente. Ele deve ter uma **[estrutura JSON válida](https://jsonlint.com)** de:

```json
{
  "shared_secret": "STRING",
  "identity_secret": "STRING",
  "device_id": "STRING"
}
```

`device_id` é opcional durante a importação, mas obrigatória para a operação do ASF - o ASF vai perguntar por ele durante a importação se você omiti-lo. É claro, você precisa substituir `"STRING"` com conteúdo válido em cada campo.

Dados padrão do autenticador tem mais campos - eles são completamente ignorados pelo ASF durante a importação, já que não são necessários. Você também não precisa removê-los - o ASF só requer um JSON válido com 2 campos obrigatórios descritos anteriormente, e opcionalmente também o `device_id`.