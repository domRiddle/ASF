# Argumentos de la línea de comandos

ASF incluye soporte para varios argumentos de la línea de comandos que pueden afectar al tiempo de ejecución del programa. Estos argumentos pueden ser usados por usuarios avanzados para especificar cómo debe ejecutarse el programa. En comparación con la forma predeterminada establecida en el archivo de configuración `ASF.json`, los argumentos de la línea de comandos pueden ser usados para la inicialización de núcleos (p.ej. `--path`), opciones específicas de plataforma (p.ej. `--system-required`) o información sensible (p.ej. `--cryptkey`).

* * *

## Utilización

El uso depende de tu Sistema Operativo y la versión de ASF.

Genérico:

```shell
dotnet ArchiSteamFarm.dll --argumento --otroArgumento
```

Windows:

```powershell
.\ArchiSteamFarm.exe --argumento --otroArgumento
```

Linux/OS X:

```shell
./ArchiSteamFarm --argumento --otroArgumento
```

Los argumentos de la línea de comandos también están soportados en scripts de ayuda genéricos como `ArchiSteamFarm.cmd` o `ArchiSteamFarm.sh`. Además, al usar scripts de ayuda también puedes emplear la propiedad de entorno `ASF_ARGS`, tal y como se explica en la sección **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**.

Si tu argumento incluye espacios, no olvides ponerlo entre comillas. Estos dos están mal:

```shell
./ArchiSteamFarm --path /home/archi/Mis Descargas/ASF # Mal!
./ArchiSteamFarm --path=/home/archi/Mis Descargas/ASF # Mal!
```

Sin embargo, éstas dos están perfectamente bien:

```shell
./ArchiSteamFarm --path "/home/archi/Mis Descargas/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/Mis Descargas/ASF" # OK
```

## Argumentos

`--cryptkey <key>` o `--cryptkey=<key>` - lanzará ASF con una clave criptográfica personalizada de valor `<key>`. Esta opción afecta a la **[seguridad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)** y hará que ASF use la clave personalizada `<key>` que has proporcionado, en lugar de la que está establecida por defecto en el ejecutable. Recuerda que las contraseñas encriptadas con dicha clave requerirán que ésta sea proporcionada en cada ejecución de ASF.

Debido a la naturaleza de esta propiedad, también es posible establecer la clave de encriptado declarando la variable de entorno `ASF_CRYPTKEY`, que podría ser más apropiada para quienes quieran evitar información sensible en los argumentos del proceso.

* * *

`--no-restart` - esta opción se usa principlamente para nuestros contenedores **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)** y fuerza `AutoRestart` al valor `false`. A menos que tengas una necesidad particular, en lugar de utilizar esta opción deberías configurar la propiedad `AutoRestart` directamente en tu configuración. Esta opción existe para que nuestro script docker no necesite modificar tu configuración globar para adaptarla a su propio entorno. Por supuesto, si estás ejecutando ASF a través de un script, también puedes utilizar esta opción (si no, es preferible emplear la propiedad de la configuración global).

* * *

`--path <path>` or `--path=<path>` - ASF siempre navega a su propio directorio al iniciarse. Al especificar este argumento, ASF navegará al directorio especificado tras la incialización, lo que te permite usar una ruta personalizada para diversas partes de la aplicación (incluyendo los directorios `config`, `plugins` y `www`, además del archivo `NLog.config`), sin la necesidad de duplicar el ejecutable en el mismo lugar. It may come especially useful if you'd like to separate binary from actual config, as it's done in Linux-like packaging - this way you can use one (up-to-date) binary with several different setups. The path can be either relative according to current place of ASF binary, or absolute. When running multiple instances of the same binary, keep in mind that you should typically disable auto-updates, as there is no synchronization between them. Also keep in mind that this command points to new "ASF home" - the directory that has the same structure as original ASF, with `config` directory inside.

Ejemplo:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/DirectorioObjetivo # Ruta absoluta
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../DirectorioObjetivo # También acepta ruta relativa
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           ├── config
    │           ├── plugins (optional)
    │           ├── www (optional)
    │           └── NLog.config (optional)
    └── ...
    

* * *

`--process-required` - declaring this switch will disable default ASF behaviour of shutting down when no bots are running. No auto-shutdown behaviour is especially useful in combination with **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)** where majority of users would expect their web service to be running regardless of the amount of bots that are enabled. If you're using IPC option or otherwise need ASF process to be running all the time until you close it yourself, this is the right option.

If you do not intend to run IPC, this option will be rather useless for you, as you can just start the process again when needed (as opposed to ASF's web server where you require it listening all the time in order to send commands).

* * *

`--system-required` - declaring this switch will cause ASF to try signalizing the OS that the process requires system to be up and running for its entire lifetime. Currently this switch has effect only on Windows machines where it'll forbid your system from going into sleep mode as long as the process is running. This can be proven especially useful when idling on your PC or laptop during night, as ASF will be able to keep your system awake while it's idling, then, once ASF is done, it'll shutdown itself like usual, making your system allowed to enter into sleep mode again, therefore saving power immediately once idling is finished.

Keep in mind that for proper auto-shutdown of ASF you need other settings - especially avoiding `--process-required` and ensuring that all your bots are following `ShutdownOnFarmingFinished`. Of course, auto-shutdown is only a possibility for this feature, not a requirement, since you can also use this flag together with e.g. `--process-required`, effectively making your system awake infinitely after starting ASF.