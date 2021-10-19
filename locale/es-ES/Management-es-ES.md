# Gestión

Esta sección cubre temas relacionados con la gestión del proceso de ASF de forma óptima. Aunque no es estrictamente obligatorio para su uso, incluye varios consejos, trucos y buenas prácticas que nos gustaría compartir, especialmente para administradores de sistema, personas que empaquetan ASF para su uso en repositorios de terceros, así como para usuarios avanzados y demás.

---

## Servicio `systemd` para Linux

En las variantes `generic` y `linux`, ASF viene con el archivo `ArchiSteamFarm@.service`, que es un archivo de configuración del servicio para **[`systemd`](https://systemd.io)**. Si deseas ejecutar ASF como un servicio, por ejemplo, para ejecutarlo automáticamente después de iniciar tu máquina, entonces un adecuado servicio `systemd` es posiblemente la mejor forma de hacerlo, por lo tanto lo recomendamos en lugar de gestionarlo por ti mismo a través de `nohup`, `screen` y demás.

Primero, crea la cuenta para el usuario con el que quieres ejecutar ASF, suponiendo que aún no existe. Usaremos el usuario `asf` para este ejemplo, si decidiste usar uno diferente, necesitarás cambiar el usuario `asf` en todos nuestros ejemplos con el que hayas seleccionado. Nuestro servicio no te permite ejecutar ASF como `root`, ya que es considerado una **[mala práctica](#nunca-ejecutes-asf-como-administrador)**.

```sh
su # or sudo -i
adduser asf
```

A continuación, descomprime ASF en la carpeta `/home/asf/ArchiSteamFarm`. La estructura de carpetas es importante para nuestra unidad de servicio, debería ser la carpeta `ArchiSteamFarm` en `$HOME`, por lo tanto `/home/<user>`.  Si hiciste todo correctamente, habrá un archivo `/home/asf/ArchiSteamFarm/ArchiSteamFarm@.service`.

Haremos todas las siguientes acciones como `root`, por lo que debes llegar al shell con el comando `su` o `sudo -i`.

En primer lugar, es buena idea asegurarse de que nuestra carpeta todavía pertenece a nuestro usuario `asf`, ejecutar `chown -hR asf:asf /home/asf/ArchiSteamFarm` una vez será suficiente. Los permisos podrían ser incorrectos, por ejemplo, si has descargado y/o descomprimido el archivo zip como usuario `root`.

A continuación, `cd /etc/systemd/system` y ejecuta `ln -s /home/asf/ArchiSteamFarm/ArchiSteamFarm\@.service .`, esto creará un enlace simbólico a nuestra declaración de servicio y lo registrará en `systemd`.

Después, asegúrate de que `systemd` reconoce nuestro servicio:

```
systemctl status ArchiSteamFarm@asf

○ ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
```

Presta especial atención al usuario que declaramos después de `@`, es `asf` en nuestro caso. Nuestra unidad de servicio systemd espera que declares el usuario, ya que influye el lugar exacto del binario `/home/<user>/ArchiSteamFarm`, así como el usuario real con el cual systemd generará el proceso.

Si systemd devolvió una salida similar a la de arriba, todo está en orden, y ya casi hemos terminado. Ahora todo lo que falta es iniciar nuestro servicio como nuestro usuario elegido: `systemctl start ArchiSteamFarm@asf`. Espera uno o dos segundos, y puedes comprobar el estatus de nuevo:

```
systemctl status ArchiSteamFarm@asf

● ArchiSteamFarm@asf.service - ArchiSteamFarm Service (on asf)
     Loaded: loaded (/etc/systemd/system/ArchiSteamFarm@.service; disabled; vendor preset: enabled)
     Active: active (running) since (...)
       Docs: https://github.com/JustArchiNET/ArchiSteamFarm/wiki
   Main PID: (...)
(...)
```

Si `systemd` indica `active (running)`, significa que todo ha salido bien, y puedes verificar que el proceso de ASF debería estar activo y ejecutándose, por ejemplo con `tail -f -n 100 /var/log/syslog`, ya que ASF por defecto también reporta su salida de consola a syslog. Si estás satisfecho con la configuración que tienes actualmente, puedes indicarle a `systemd` que inicie automáticamente tu servicio durante el arranque, ejecutando el comando `systemctl enable ArchiSteamFarm@asf`. Eso es todo.

Si por alguna razón quieres detener el proceso, simplemente ejecuta `systemctl stop ArchiSteamFarm@asf`. Del mismo modo, si quieres deshabilitar que ASF se inicie automáticamente durante el arranque,  `systemctl disable ArchiSteamFarm@asf` funcionará para ti, es muy simple.

Ten en cuenta que como no hay una entrada estándar habilitada para nuestro servicio `systemd`, no serás capaz de ingresar tus datos a través de la consola de forma habitual. Ejecutar a través de `systemd` es equivalente a especificar el ajuste **[`Headless: true`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-ES#headless)** y conlleva todas sus implicaciones. Afortunadamente para ti, es muy fácil administrar ASF a través de **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-ES#asf-ui)**, lo que recomendamos en caso de que necesites proporcionar detalles adicionales durante el inicio de sesión o administrar el proceso de ASF.

### Variables de entorno

Es posible proporcionar variables de entorno adicionales a nuestro servicio `systemd`, lo que te interesa en caso de que quieras usar un **[argumento de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-es-ES#argumentos)** `--cryptkey` personalizado, y por lo tanto especificar la variable de entorno `ASF_CRYPTKEY`.

Para proporcionar variables de entorno personalizadas, crea la carpeta `/etc/asf` (en caso de que no exista), `mkdir -p /etc/asf`, luego escribe a un archivo `/etc/asf/<user>`, donde `<user>` es el usuario con el que estás ejecutando el servicio (`asf` en nuestro ejemplo de arriba, por lo tanto `/etc/asf/asf`).

El archivo debería contener todas las variables de entorno que quieras proporcionar al proceso:

```sh
# Declara solo aquellos que realmente necesitas
ASF_CRYPTKEY="mi_cryptkey_secreta_super_importante"
ASF_NETWORK_GROUP="mi_grupo_de_red"

# Y cualquier otro que te interese
```

---

## ¡Nunca ejecutes ASF como administrador!

ASF incluye su propia validación ya sea que el proceso se ejecuto como administrador (`root`) o no. Ejecutar como root **no** se requiere para ningún tipo de operación realizada por el proceso de ASF, suponiendo que el entorno en el que opera esté correctamente configurado, y por lo tanto debería ser considerado como una **mala práctica**. Esto significa que en Windows, ASF nunca debe ser ejecutado con la configuración "ejecutar como administrador", y en Unix ASF debería tener una cuenta de usuario dedicada, o reutilizar la tuya en caso de un sistema de escritorio.

Para más información sobre *por qué* no recomendamos ejecutar ASF como root, revisa **[superuser](https://superuser.com/questions/218379/why-is-it-bad-to-run-as-root)** y otros recursos. Si todavía no estás convencido, pregúntate qué le pasaría a tu máquina si el proceso de ASF ejecutara el comando `rm -rf --no-preserve-root /` justo después de ejecutarse.

### Ejecuto como `root` porque ASF no puede escribir a sus archivos

Esto significa que configuraste incorrectamente los permisos de los archivos a los que ASF está intentando acceder. Deberías iniciar sesión como cuenta `root` (ya sea con `su` o `sudo -i`) y luego **corregir** los permisos enviando el comando `chown -hR asf:asf /path/to/ASF`, sustituyendo `asf:asf` con el usuario con el que ejecutarás ASF, y reemplazar `/path/to/ASF` con la ruta real de ASF. Si por alguna razón estás usando un `--path` personalizado indicando al usuario de ASF que utilice un directorio diferente, deberías ejecutar el mismo comando también para esa ruta.

Después de eso, ya no deberías tener ningún tipo de problema relacionado con que ASF no pueda escribir a sus propios archivos, ya que has cambiado el propietario de todo en lo que ASF está interesado al usuario con el cual el proceso de ASF se ejecutará realmente.

### Ejecuto como `root` porque no sé cómo hacerlo de otro modo

```sh
su # or sudo -i
adduser asf
chown -hR asf:asf /path/to/ASF
su asf -c /path/to/ASF/ArchiSteamFarm # or sudo -u asf /path/to/ASF/ArchiSteamFarm
```

Eso sería hacerlo manualmente, es mucho más fácil usar nuestro **servicio [`systemd`](#servicio-systemd-para-linux)** explicado anteriormente.

---

## Múltiples instancias

ASF es compatible con ejecutar múltiples instancias del proceso en la misma máquina. Las instancias pueden ser completamente independientes o derivadas de la ubicación del mismo ejecutable (en cuyo caso, querrás ejecutarlas con un **[argumento de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-es-es)** `--path` diferente).

Al ejecutar múltiples instancias del mismo ejecutable, ten en cuenta que normalmente debes deshabilitar las actualizaciones automáticas en todas sus configuraciones, ya que no hay sincronización entre ellas en lo que se refiere a las actualizaciones automáticas. Si quieres dejar las actualizaciones automáticas activadas, recomendamos instancias independientes, pero aún puedes hacer que las actualizaciones funcionen, siempre y cuando te asegures de que todas las demás instancias de ASF están cerradas.

ASF hará lo posible para mantener una cantidad mínima de comunicación entre procesos en todo el sistema operativo con otras instancias de ASF. Esto incluye que ASF compruebe su directorio de configuración contra otras instancias, así como compartir limitadores en el proceso configurados con la **[propiedad de configuración global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#configuraci%C3%B3n-global)** `*LimiterDelay`, asegurando que ejecutar múltiples instancias de ASF no causará tener problemas por alcanzar el límite de solicitudes. En lo que respecta a los aspectos técnicos, todas las plataformas utilizan nuestro mecanismo dedicado de bloqueos basado en archivos creados en un directorio temporal, que en Windows es `C:\Users\<YourUser>\AppData\Local\Temp\ASF`, y en Unix es `/tmp/ASF`.

No es necesario que las instancias de ASF compartan las mismas propiedades `*LimiterDelay`, pueden usar diferentes valores, ya que cada ASF añadirá su propio atraso configurado al tiempo de liberación después de adquirir el bloqueo. Si el `*LimiterDelay` configurado se establece en `0`, la instancia de ASF omitirá esperar para el bloqueo de un recurso determinado que se comparte con otras instancias (que potencialmente aún podrían mantener un bloqueo compartido entre sí). Cuando se establece en cualquier otro valor, ASF se sincronizará correctamente con otras instancias y esperará su turno, luego libera el bloqueo después del retraso configurado, permitiendo que otras instancias continúen.

ASF toma en cuenta la configuración de `WebProxy` al decidir sobre el ámbito compartido, lo que significa que dos instancias de ASF que usan diferentes configuraciones `WebProxy` no compartirán sus limitadores entre sí. Esto se implementa para permitir que las configuraciones `WebProxy` funcionen sin retrasos excesivos, como se espera de diferentes interfaces de red. Esto debería ser suficiente para la mayoría de los casos de uso, sin embargo, si tienes una configuración personalizada en la que, por ejemplo, estés enrutando solicitudes de forma distinta, puedes especificar el grupo de red a través del **[argumento de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-es-es)** `--network-group`, lo que te permitirá declarar el grupo de ASF que se sincronizará con esta instancia. Ten en cuenta que los grupos de red personalizados se utilizan exclusivamente, lo que significa que ASF ya no usará `WebProxy` para determinar el grupo correcto, ya que tú eres responsable del agrupamiento en este caso.