# Iniciando

Si llegaste aquí por primera vez, ¡bienvenido! Estamos muy contentos de ver a otro viajero interesado en nuestro proyecto, aunque ten en cuenta que con un gran poder viene una gran responsabilidad - ASF es capaz de hacer muchas cosas relacionadas con Steam, siempre y cuando **te intereses lo suficiente para aprender cómo usarlo**. Hay una difícil curva de aprendizaje involucrada aquí, y esperamos que leas la wiki en este sentido, la cual explica en detalle cómo funciona todo.

Si todavía sigues aquí significa que soportaste nuestro texto de arriba, lo cual es bueno. A menos que te lo hayas saltado, entonces vas a tener un **[mal momento](https://www.youtube.com/watch?v=WJgt6m6njVw)** muy pronto... En cualquier caso, ASF es una aplicación de consola, lo que significa que el programa en sí no tiene una GUI (Interfaz Gráfica de Usuario) amigable como a las que en general estás acostumbrado. ASF estaba pensado principalmente para ser ejecutado en servidores, por lo que actúa como un servicio (daemon) y no como una aplicación de escritorio.

Sin embargo, esto no significa que no puedas usarlo en tu PC o que usarlo es de alguna manera más complicado de lo usual, nada de eso. ASF es un programa independiente que no necesita instalación, y funciona en seguida, pero requiere una configuración antes de ser útil. La configuración es decirle a ASF lo que en realidad debe hacer después de ejecutarlo. Si lo ejecutas sin configuración, entonces ASF no hará nada, simple.

* * *

## Configuración de SO específico

En general, aquí está lo que haremos los próximos minutos:

- Instalar los **[prerrequisitos de .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Descargar la **[última versión de ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** en su apropiada variante de SO específico.
- Extraer el archivo un una nueva ubicación (y usar `chmod +x ArchiSteamFarm` si estás en Linux/OS X).
- **[Configurar ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Ejecutar ASF y ver la magia.

Suena bastante simple, ¿cierto? Así que hagámoslo.

* * *

### Prerrequisitos de .NET Core

El primer paso es asegurarte de que tu SO puede siquiera ejecutar ASF correctamente. ASF está escrito en C#, basado en .NET Core y puede requerir librerías nativas que todavía no están disponibles en tu plataforma. Dependiendo de si usas Windows, Linux o OS X, tendrás diferentes requerimientos, aunque todos ellos están enlistados en el documento **[Prerrequisitos de .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** que debes seguir. Este es nuestro material de referencia que debe ser usado, pero por simplicidad también hemos detallado debajo todos los paquetes requeridos, para que no tengas que leer el documento completo.

Es perfectamente normal que algunas (o incluso todas) las dependencias ya existan en tu sistema por haber sido instaladas por software de terceros que utilices. Aún así, debes asegurarte que verdaderamente ese es el caso ejecutando el instalador adecuado para tu sistema operativo - sin esas dependencias ASF no se ejecutará.

Ten en cuenta que no necesitas hacer nada más para la compilación de SO específico, especialmente instalar .NET Core SDK o incluso el tiempo de ejecución, ya que el paquete de SO específico ya incluye todo eso. Solamente necesitas los prerrequisitos de .NET Core (dependencias) para correr el tiempo de ejecución de .NET Core incluido en ASF.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 para Windows de 64-bits, x86 para Windows de 32-bits)
- Es altamente recomendado que te asegures que todas las actualizaciones de Windows ya estén instaladas. Por lo menos necesitas **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** y **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**, pero podrían necesitarse más actualizaciones. Todas ellas ya están instaladas si tu Windows está actualizado. Asegúrate de cumplir esos requisitos antes de instalar el paquete Visual C++.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Los nombres de los paquetes depende de la distribución de Linux que estés usando, hemos listado las más comunes. Puedes obtener todas con el administrador de paquetes nativos para tu sistema operativo (tal como `apt` para Debian o `yum` para CentOS).

- libcurl3 (libcurl)
- libicu (última versión para tu distribución, por ejemplo `libicu57` para Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, compat-openssl10, última versión 1.0.X para tu distribución)
- zlib1g (zlib)

Al menos algunas de esas ya deberían estar disponibles nativamente en tu sistema (tal como zlib1g que es requerida en casi cualquier distro de Linux hoy en día).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- Ninguno por ahora

* * *

### Descargando

Ya que tengamos todas las dependencias requeridas, el siguiente paso es descargar la **[última versión de ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF está disponible en diversas variantes, pero te interesa el paquete que concuerde con tu sistema operativo y arquitectura. Por ejemplo, si usas `Win`dows de `64`-bits, entonces necesitas el paquete `ASF-win-x64`. Para más información acerca de las variantes disponibles, visita la sección de **[compatibilidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)**. ASF también es capaz de ejecutarse en sistemas operativos para los que no construimos un paquete de SO específico, tal como **Windows de 32-bits**, dirígite a **[configuración genérica](#generic-setup)** para eso.

![Recursos](https://i.imgur.com/Ym2xPE5.png)

Una vez que tengas tu paquete y extraigas el archivo zip (recomendamos usar **[7-zip](https://www.7-zip.org)**), tendrás un gran desorden de carpetas y archivos. No te preocupes, lo limpiaremos en un segundo.

Si estás usando Linux/OS X, no te olvides de usar `chmod +x ArchiSteamFarm`, ya que los permisos no están establecidos automáticamente en el archivo zip. Esto solo se tiene que hacer una vez después del desempaquetado inicial.

Ten en cuenta desempaquetar ASF en **su propio directorio** y no a alguno existente que ya estés usando para algo más - la función de autoactualización de ASF eliminará todos los archivos antiguos y no relacionados cuando se actualice, lo que podría resultar en la pérdida de cualquier cosa no relacionada que pongas en el directorio de ASF. Si tienes algún script o archivo adicional que quieras usar con ASF, pónlos en una carpeta superior.

Un ejemplo de la estructura se vería así:

    C:\ASF (donde pones tus cosas)
        ├── ASF shortcut.lnk (opcional)
        ├── Config shortcut.lnk (opcional)
        ├── Commands.txt (opcional)
        ├── MyExtraScript.bat (opcional)
        ├── ... (cualquier otro archivo de tu elección, opcional)
        └── Core (dedicado a ASF solamente, donde extraes el archivo)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

Esta es una estructura que recomendamos, para que no tengas la necesidad de navegar por un gran número de archivos y carpetas incluidos en ASF, ya que para su uso solamente necesitas un acceso directo a la carpeta de configuración (config) y el binario principal.

Muy bien, ahora prepararemos la carpeta de ASF para su uso. Si lo deseas, puedes saltar al siguiente paso, ya que limpiar la estructura de ASF no es obligatorio, pero hará tu vida un poco más fácil.

Abre la carpeta de ASF y busca el archivo ejecutable, este será `ArchiSteamFarm.exe` en Windows, y `ArchiSteamFarm` en Linux/OS X. Haz clic derecho y selecciona "copiar". Ahora navega al lugar donde quieras tener el acceso directo de ASF (como tu escritorio), haz clic derecho y elige "pegar acceso directo". Puedes renombrar tu acceso directo si lo deseas, como nombrarlo "ASF". Ahora haz lo mismo con el directorio `config` que puedes encontrar en el mismo lugar que el binario de ASF.

Después de una pequeña limpieza, tendrás una estructura conveniente similar a la de abajo:

![Estructura](https://i.imgur.com/k85csaZ.png)

Esto te permitirá acceder fácilmente al binario de ASF y a los archivos de configuración sin mucho problema. En mi caso decidí usar la estructura mencionada arriba, así que mis archivos de ASF están directamente en el directorio "Core". Puedes adaptar esta estructura a tu gusto, tal como tener ASF más los accesos directos de configuración en el escritorio y el directorio de ASF, por ejemplo, en `C:\ASF`; es tu decisión.

Se aconseja a los usuarios de Linux/OS X que hagan lo mismo, pueden usar excelentes mecanismos de enlaces simbólicos disponibles a través de `ln -s`.

* * *

### Configuración

Ahora estamos listos para hacer el último paso, la configuración. Este es por mucho el paso más difícil, ya que involucra mucha información nueva con la que todavía no estás familiarizado, así que intentaremos proporcionar ejemplos fáciles de entender y una explicación simplificada.

Primero y más importante, hay una página de **[configuración](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** que explica **todo** lo que se relaciona con la configuración, pero es una enorme cantidad de información nueva, mucha de la cual no necesitamos saber ahora mismo. En cambio, te enseñaremos cómo obtener la información que realmente necesitas.

La configuración de ASF puede realizarse de dos maneras - ya sea usando nuestro generador de configuración web, o manualmente. Esto se explica a detalle en la sección de **[configuración](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**, así que consúltala si quieres información más detallada. Usaremos el generador de configuración web, ya que es mucho más fácil.

Dirígete a la página de nuestro **[generador de configuración web](https://justarchinet.github.io/ASF-WebConfigGenerator)** con tu navegador preferido, necesitas tener javascript activado en caso de que lo hayas desactivado manualmente. Recomendamos Chrome o Firefox, pero debería funcionar en todos los navegadores más populares.

Después de abrir la página, cambia a la pestaña "Bot". Ahora deberías ver una página similar a la siguiente:

![Pestaña de bot](https://i.imgur.com/aF3k8Rg.png)

Si por cualquier motivo la versión de ASF que hayas descargado es más antigua que la que el generador de configuración usa por defecto, simplemente elige tu versión de ASF del menú desplegable. Esto puede ocurrir ya que el generador de configuración puede ser usado para generar configuraciones para versiones más nuevas de ASF (prelanzamiento) que aún no han sido marcadas como estables. Has descargado la última versión estable de ASF que se ha verificado que funciona confiablemente.

Empieza por poner un nombre para tu bot en el campo resaltado en rojo. Este puede ser cualquier nombre que desees utilizar, tal como tu "nickname", nombre de la cuenta, un número, o cualquier otra cosa. Solo hay una palabra que no se puede utilizar, `ASF`, ya que esa palabra está reservada para el archivo de configuración global. Además de eso el nombre de tu bot no puede empezar con un punto (ASF intencionalmente ignora esos archivos). También recomendamos que evites usar espacios, puedes usar `_` como separador de palabras si es necesario.

Después de decidir tu nombre, cambia el interruptor de `Enabled` para que esté activo, esto determina si tu bot es iniciado automáticamente por ASF tras la ejecución (del programa).

Ahora puedes decidir entre dos cosas:

- Puedes poner tu nombre de usuario en el campo `SteamLogin` y tu contraseña en el campo `SteamPassword`
- O puedes dejarlos vacíos

Haciendo lo primero permitirá que ASF use automáticamente las credenciales de tu cuenta durante el inicio, por lo que no necesitarás ingresarlas manualmente cada vez que ASF las necesite. Sin embargo puedes decidir omitirlas, en cuyo caso no serán guardadas, por lo que ASF no será capaz de iniciar automáticamente sin tu ayuda y necesitarás ingresarlas durante el tiempo de ejecución (runtime).

ASF requiere tus credenciales de inicio de sesión porque incluye su propia implementación del cliente de Steam y necesita los mismos detalles para iniciar sesión como el que usas. Tus credenciales de inicio de sesión no se guardan en ninguna parte, excepto en tu PC, solo en el directorio `config` de ASF, nuestro generador de configuración web es un cliente, lo que significa que el código es ejecutado localmente en tu explorador para generar configuraciones válidas de ASF, sin detalles, ingresa siempre dejando tu PC en primer lugar, por lo que no hay necesidad de preocuparse por ninguna posible fuga de datos confidenciales. Aún, si por cualquier razón no quieres ingresar tus credenciales ahí, entendemos eso, y puedes ingresarlas manualmente luego en los archivos generados, u omitirlas completamente e ingresarlas solo en la línea de comandos ASF. Puedes encontrar más acerca de seguridad en la sección **[configuración](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.

También puedes decidir dejar un campo vacío, como `ContraseñaSteam`, entonces ASF sera capaz de usar tu inicio de sesión automaticamente, pero todavía le pedirá la contraseña (simiar al cliente Steam). Si usas el control parental de steam para desbloquear la cuenta, necesitarás ponerlo en el campo `SteamParentalCode`.

Después de la decisión y detalles opcionales, ahora su página web se verá similar a la siguiente:

![Pestaña de bot 2](https://i.imgur.com/yf54Ouc.png)

Ahora puedes presionar el botón "Descargar" y nuestro generador de configuración web creará un nuevo archivo `json` basado en el nombre que hayas elegido. Guarda ese archivo en el directorio `config` de ASF. Puedes usar el atajo a `config` previamente creado, o buscar manualmente el directorio `config`, directamente en la estructura de archivos de ASF.

Tu directorio `config` ahora se verá así:

![Estructura 2](https://i.imgur.com/2s7ZUUu.png)

¡Felicidades! Acabas de terminar la configuración más básica de un bot en ASF. Explicaremos más en breve, por ahora esto es todo lo que necesitas.

* * *

### Ejecutando ASF

Ahora estás listo para ejecutar el programa por primera vez. Simplemente haz doble clic en el acceso directo de ASF, o en el ejecutable `ArchiSteamFarm(.exe)` que se encuentra en el directorio de ASF.

Posteriormente, asumiendo que instalaste todas las dependencias necesarias en el primer paso, ASF debería ejecutarse correctamente, detectar tu primer bot (si no olvidaste poner el archivo generado en el directorio `config`), e intentar iniciar sesión:

![ASF](https://i.imgur.com/u5hrSFz.png)

Si proporcionaste `SteamLogin` y `SteamPassword` para que utilice ASF, se te pedirá solamente tu código de Steam Guard (e-mail, 2FA o ninguno, dependiendo de tu configuración en Steam). Si no lo hiciste, también se te pedirá tu nombre de usuario y contraseña.

Ahora es buen momento para revisar nuestra sección **[política de privacidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** si te preocupa qué pasará a continuación, como se expresa por el mismo ASF.

Después del inicio de sesión, asumiendo que tus datos son correctos, iniciarás sesión con éxito, y ASF comenzará a "idlear" usando la configuración predeterminada:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

Esto prueba que ASF está haciendo su trabajo con éxito en tu cuenta, ahora puedes minimizar el programa y hacer algo más. Después del suficiente tiempo (dependiendo de **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**) verás que lentamente recibes cromos de Steam. Por supuesto, para que eso suceda debes tener juegos válidos para "idlear", mostrado como "puedes obtener X cromos más jugando este juego" en tu **[página de insignias](https://steamcommunity.com/my/badges)** - si no hay juegos para "idlear", entonces ASF indicará que no hay nada que hacer, como se expresa en nuestras **[Preguntas Frecuentes](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#so-how-it-exactly-works)**.

Esto concluye nuestra guía de configuración básica. Ahora puedes decidir si quieres seguir configurando ASF, o dejarlo hacer su trabajo con la configuración predeterminada. Abarcaremos algunos detalles básicos más, y luego dejaremos toda la wiki para que descubras.

* * *

### Configuración extendida

#### "Idlear" varias cuentas a la vez

ASF soporta "idlear" más de una cuenta a la vez, la cual es su función primaria. Puedes añadir más cuentas a ASF generando más archivos de configuración de bots, exactamente del mismo modo que generaste el primero hace algunos minutos. Solo debes de asegurarte de dos cosas:

- Nombre de bot único, si ya nombraste tu primer bot como "CuentaPrincipal", no puedes tener otro con el mismo nombre.
- Detalles de inicio de sesión válidos, tales como `SteamLogin`, `SteamPassword` y `SteamParentalCode` (si usas la configuración parental de Steam)

En otras palabras, simplemente ve a configuración de nuevo y haz exactamente los mismo, solo que para tu segunda o tercera cuenta. Recuerda usar nombres únicos para todos tus bots.

* * *

#### Cambiar la configuración

Puedes cambiar ajustes existentes de la misma forma - generando un nuevo archivo de configuración. Si aún no has cerrado nuestro generador de configuración web, haz clic en "toggle advanced settings" y ve lo que hay ahí para descubrir. Para este tutorial cambiaremos el ajuste `CustomGamePlayedWhileFarming`, que te permite establecer que se muestre un nombre personalizado cuando ASF está "idleando", en lugar de mostrar el nombre real del juego.

Empecemos, si ejecutas ASF y empieza a "idlear", en ajustes predeterminados verás que tu cuenta de Steam está jugando:

![Steam](https://i.imgur.com/sCdSMZj.png)

Cambiemos eso. Cambia a configuración avanzada en el generador de configuración web y busca `CustomGamePlayedWhileFarming`. Una vez que hagas eso, pon el texto personalizado que quieras que se muestre, tal como "idleando cromos":

![Pestaña de bot 4](https://i.imgur.com/gHqdEqb.png)

Ahora descarga el nuevo archivo de configuración exactamente de la misma manera, luego **sobrescribe** tu archivo de configuración previo con el nuevo. También puedes eliminar tu viejo archivo de configuración y poner el nuevo en su lugar.

Una vez que hagas eso y ejecutes ASF de nuevo, notarás que ASF ahora muestra tu texto personalizado:

![Steam 2](https://i.imgur.com/NeFYrdU.png)

Esto confirma que editaste con éxito tu configuración. Del mismo modo puedes cambiar las propiedades globales de ASF, cambiando de la pestaña del bot a la de "ASF", y luego descargando el archivo de configuración generado y reemplazando el archivo `ASF.json`.

* * *

#### Usando la interfaz de ASF

ASF es una aplicación de consola y no incluye una interfaz gráfica de usuario. However, there is ongoing work on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface which is still in preview state, but can be used without bigger issues.

In order to use ASF-ui, you should ensure that you set up `IPC` and `SteamOwnerID` global configuration properties (ASF tab).

For `SteamOwnerID`, you need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, we'll use **[SteamRep](https://steamrep.com)** for that purpose. Open the website, locate sign in through Steam button in top right corner, then log in. Afterwards, in the same place, click on your avatar, and look up `steamID64` field on your profile.

![SteamRep](https://i.imgur.com/RUuJ63i.png)

For my account, this is `76561198006963719` number. You'll have a similar one, also starting from `7656`. Copy it.

Now navigate once again to our web config generator and input that number as SteamOwnerID.

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

You need to do only one more thing, toggle advanced settings, find `IPC` option, and enable it.

![IPC](https://i.imgur.com/NhujZCN.png)

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![IPC 3](https://i.imgur.com/vCu2ZY5.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### Summary

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Instalar los **[prerrequisitos de .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configurar ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.