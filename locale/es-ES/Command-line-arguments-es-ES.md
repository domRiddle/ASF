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

Los argumentos de la línea de comandos también están soportados en scripts de ayuda genéricos como `ArchiSteamFarm.cmd` o `ArchiSteamFarm.sh`. Además, al usar scripts de ayuda también puedes emplear la propiedad de entorno `ASF_ARGS`, tal y como se explica en la sección **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-es-es#argumentos-de-la-l%C3%ADnea-de-comandos)**.

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

`--cryptkey <key>` o `--cryptkey=<key>` - lanzará ASF con una clave criptográfica personalizada de valor `<key>`. Esta opción afecta a la **[seguridad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security-es-es)** y hará que ASF use la clave personalizada `<key>` que has proporcionado, en lugar de la que está establecida por defecto en el ejecutable. Since this property affects default encryption key (for encrypting purposes) as well as salt (for hashing purposes), Keep in mind that everything encrypted/hashed with this key will require it to be passed on each ASF run.

Debido a la naturaleza de esta propiedad, también es posible establecer la clave de encriptado declarando la variable de entorno `ASF_CRYPTKEY`, que podría ser más apropiada para quienes quieran evitar información sensible en los argumentos del proceso.

* * *

`--network-group <group>` o `--network-group=<group>` - causará que ASF inicialice sus limitadores con un grupo de red personalizado con el valor `<group>`. Esta opción afecta a ASF ejecutándose en **[múltiples instancias](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-es#m%C3%BAltiples-instancias)** señalizando que una instancia dada es dependiente solo de instancias que compartan el mismo grupo de red, e independiente del resto. Normalmente quieres usar esta propiedad solo si estás enrutando solicitudes de ASF a través de un mecanismo personalizado (por ejemplo, diferentes direcciones IP) y quieres establecer grupos de red, sin depender de ASF para hacerlo automáticamente (lo que actualmente incluye tomar en cuenta solo `WebProxy`). Ten en cuenta que al usar un grupo de red personalizado, este es un identificador único dentro de la misma máquina, y ASF no tendrá en cuenta ningún otro detalle, tal como el valor `WebProxy`, permitiéndote, por ejemplo, iniciar dos instancias con diferentes valores `WebProxy` que siguen siendo dependientes entre sí.

Debido a la naturaleza de esta propiedad, también es posible establecer el valor declarando la variable de entorno `ASF_NETWORK_GROUP`, lo que podría ser más apropiado para las personas que quieran evitar información confidencial en los argumentos del proceso.

* * *

`--no-restart` - esta opción se usa principlamente para nuestros contenedores **[docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker-es-es)** y fuerza `AutoRestart` al valor `false`. A menos que tengas una necesidad particular, en lugar de utilizar esta opción deberías configurar la propiedad `AutoRestart` directamente en tu configuración. Esta opción existe para que nuestro script docker no necesite modificar tu configuración globar para adaptarla a su propio entorno. Por supuesto, si estás ejecutando ASF a través de un script, también puedes utilizar esta opción (si no, es preferible emplear la propiedad de la configuración global).

* * *

`--path <path>` o `--path=<path>` - ASF siempre navega a su propio directorio al iniciarse. Al especificar este argumento, ASF navegará al directorio especificado tras la inicialización, lo que te permite usar una ruta personalizada para diversas partes de la aplicación (incluyendo los directorios `config`, `plugins` y `www`, además del archivo `NLog.config`), sin la necesidad de duplicar el ejecutable en el mismo lugar. Puede resultar especialmente útil si quieres separar el ejecutable de la configuración, como se hace en el paquete Linux - de esta forma puedes usar un (actualizado) binario con diferentes configuraciones. La ruta puede ser relativa según el lugar actual del binario de ASF, o absoluta. Ten en cuenta que este comando apunta a un nuevo "ASF home" - el directorio que tiene la misma estructura que el ASF original, con el directorio config dentro, ve el ejemplo de abajo para la explicación.

Debido a la naturaleza de esta propiedad, también es posible establecer una ruta esperada declarando la variable de entorno `ASF_PATH`, que puede ser más apropiado para las personas que quieran evitar detalles sensibles en los argumentos del proceso.

Si estás considerando usar este argumento de la línea de comandos para ejecutar múltiples instancias de ASF, recomendamos leer nuestra **[página de compatibilidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-es#múltiples-instancias)**.

Ejemplos:

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path /opt/TargetDirectory # Absolute path
dotnet /opt/ASF/ArchiSteamFarm.dll --path ../TargetDirectory # Relative path works as well
ASF_PATH=/opt/TargetDirectory dotnet /opt/ASF/ArchiSteamFarm.dll # Same as env variable
```

```text
├── /opt
│     ├── ASF
│     │     ├── ArchiSteamFarm.dll
│     │     └── ...
│     └── TargetDirectory
│           ├── config
│           ├── logs (generado)
│           ├── plugins (opcional)
│           ├── www (opcional)
│           ├── log.txt (generado)
│           └── NLog.config (opcional)
└── ...
```

* * *

`--process-required` - declarar esto desactivará el comportamiento predeterminado de ASF de cerrarse cuando no hay bots en ejecución. El comportamiento de autocerrarse es especialmente útil en combinación con **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es)** donde la mayoría de usuarios esperarían que su servicio web se ejecute independientemente de la cantidad de bots activos. Si estás usando la opción IPC o necesitas que el proceso de ASF se ejecute todo el tiempo hasta que lo cierrres, esta es la opción correcta.

Si no pretendes usar IPC, esta opción será inútil para ti, ya que puedes solamente iniciar el proceso de nuevo cuando lo necesites (contrario al servidor web de ASF donde requieres que esté escuchando todo el tiempo para enviar comandos).

* * *

`--system-required` - declarar esto causará que ASF intente indicar al sistema operativo que el proceso requiere que el sistema esté activo y ejecutándose durante todo su tiempo de vida. Actualmente esto solo tiene efecto máquinas con Windows donde este impedirá que tu sistema entre en modo de suspensión mientras el proceso se esté ejecutando. Esto puede ser especialmente útil cuando estás recolectando cromos en tu PC o laptop durante la noche, ya que ASF podrá mantener tu sistema despierto mientras está recolectando, luego, una vez que ASF termine, se cerrará como de costumbre, permitiendo a tu sistema entrar en modo de sueño nuevamente, y por lo tanto ahorrando energía inmediatamente una vez que la recolección termine.

Ten cuenta que para un adecuado cierre automático de ASF necesitas otros ajustes - especialmente evitar `--process-required` y asegurarte que todos tus bots tengan `ShutdownOnFarmingFinished`. Por supuesto, el autocierre solo es una posibilidad para esta función, no un requisito, ya que también puedes usar esta bandera junto con, por ejemplo `--process-required`, haciendo que tu sistema se mantenga despierto infinitamente después de iniciar ASF.