# Docker

A partir de la versión 3.0.3.2, ASF también está disponible como **[contenedor docker](https://www.docker.com/what-container)**. Ejecutar ASF en contenedor docker normalmente no tiene ventajas para los usuarios casuales, pero podría ser una excelente forma de usar ASF en servidores, asegurando que ASF se está ejecutando en un entorno sandbox separado de todas las demás aplicaciones. Nuestros paquetes docker están actualmente disponibles en **[ghcr.io](https://github.com/orgs/JustArchiNET/packages/container/archisteamfarm/versions)** y en **[Docker Hub](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Etiquetas

ASF está disponible a través de 4 tipos principales de **[etiquetas](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `main`

Esta etiqueta siempre apunta al ASF compilado a partir del último commit en la rama `main`, que funciona igual que la compilación AppVeyor experimental descrita en nuestro **[ciclo de lanzamiento](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-es-es)**. Normalmente debes evitar esta etiqueta, ya que es el nivel más alto de software con bugs dedicado a desarrolladores y usuarios avanzados para fines de desarrollo. La imagen se actualiza con cada commit en la rama `main` de GitHub, por lo tanto puedes esperar actualizaciones muy frecuentes (y cosas con fallos), como en nuestra compilación AppVeyor. Esta etiqueta está aquí para que marquemos el estado actual del proyecto ASF, que no necesariamente se garantiza que sea estable o probado, tal como se indica en nuestro ciclo de lanzamiento. Esta etiqueta no debe ser usada en ningún entorno de producción.


### `released`

Muy similar a la anterior, esta etiqueta siempre apunta a la última versión **[publicada](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** de ASF, incluyendo pre-versiones. Comparado con la etiqueta `main`, esta imagen se actualiza cada vez que se envía una nueva etiqueta de GitHub. Dedicado a usuarios avanzados que les encanta vivir al límite de lo que se puede considerar estable y fresco al mismo tiempo. Esta es la que recomendamos si no quieres usar la etiqueta `latest`. Por favor, ten en cuenta que usar esta etiqueta es igual a usar nuestros **[prelanzamientos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-es-es)**.


### `latest`

Esta etiqueta en comparación con otras, es la única que incluye la característica de actualizaciones automáticas y apunta a la última versión **[estable](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** de ASF. El objetivo de esta etiqueta es proporcionar un contenedor Docker por defecto que sea capaz de ejecutar una compilación autoactualizable de ASF para sistema operativo específico. Por eso, la imagen no tiene que actualizarse con tanta frecuencia, ya que la versión incluida de ASF siempre será capaz de actualizarse si es necesario. Por supuesto, `UpdatePeriod` puede desactivarse de forma segura (establecido a `0`), pero en este caso probablemente debas usar una versión congelada `A.B.C.D`. Del mismo modo, puedes modificar el `UpdateChannel` por defecto para hacer autoactualizable la etiqueta `released`.

Debido a que la imagen `latest` tiene la capacidad de actualizarse automáticamente, incluye un sistema operativo básico con la versión de ASF de sistema operativo específico, contrario a todas las demás etiquetas que incluyen sistema operativo con .NET Core runtime y la versión `generic` de ASF. Esto se debe a que la versión más reciente (actualizada) de ASF también puede podría un runtime más reciente que el que posiblemente esté integrado en la imagen, que de otro modo requeriría que la imagen sea reconstruida desde cero, anulando el caso de uso previsto.

### `A.B.C.D`

En comparación con las etiquetas anteriores, esta etiquetas está completamente congelada, lo que significa que la imagen no se actualizará una vez publicada. Esto funciona similar a nuestras publicaciones de GitHub que nunca se tocan después de la publicación inicial, lo que te garantiza un entorno estable y congelado. Normalmente debes utilizar esta etiqueta cuando quieras usar alguna versión específica de ASF (más antigua que `latest`) y no quieres usar ningún tipo de actualizaciones automáticas (por ejemplo, aquellas ofrecidas en la etiqueta `latest`).

---

## ¿Qué etiqueta es mejor para mí?

Eso depende de lo que busques. Para la mayoría de los usuarios, la etiqueta `latest` debería ser la mejor ya que ofrece exactamente lo que hace ASF de escritorio, solo que en un contenedor Docker especial como servicio. Las personas que recompilan sus imágenes con frecuencia y en su lugar preferirían tener una versión de ASF atada a una publicación determinada son bienvenidas a usar la etiqueta `released`. Si en cambio quieres usar alguna versión congelada de ASF que nunca cambiará sin tu clara intención, las versiones `A.B.C.D` están disponibles como marcas fijas a las que siempre puedes regresar.

Generalmente no recomendamos probar las compilaciones `main`, al igual que las compilaciones automatizadas de AppVeyor - esta compilación está para marcar el estado actual del proyecto ASF. Nada garantiza que dicho estado funcione correctamente, pero eres más que bienvenido a probarlas si estás interesado en el desarrollo de ASF.

---

## Arquitecturas

La imagen docker ASF se compila actualmente en la plataforma `linux` con tres arquitecturas - `x64`, `arm` y `arm64`. Puedes leer más acerca de ellas en la sección **[compatibilidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-es)**.

Desde la versión V5.0.2.2 de ASF, nuestras etiquetas utilizan un manifiesto multiplataforma, lo que significa que el Docker instalado en tu máquina seleccionará automáticamente la imagen adecuada para tu plataforma al extraer la imagen. Si por algún motivo deseas extraer una imagen de plataforma específica que no coincida con la que estás ejecutando, puedes hacerlo mediante el modificador `--platform` con los comandos docker apropiados, tal como `docker run`. Para más información revisa la documentación docker en **[image manifest](https://docs.docker.com/registry/spec/manifest-v2-2)**.

---

## Uso

Para referencia completa debes usar **[documentación oficial de docker](https://docs.docker.com/engine/reference/commandline/docker)**, solo cubriremos el uso básico en esta guía, eres más que bienvenido a profundizar más.

### ¡Hola, ASF!

Primeramente debemos verificar si nuestro docker funciona correctamente, esto servirá como nuestro "hola, mundo" de ASF:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm
```

`docker run` crea un nuevo contenedor docker ASF y lo ejecuta en primer plano (`-it`). `--pull always` asegura que una imagen actualizada sea extraída primero, y `--rm` asegura que nuestro contenedor sea purgado una vez que se detenga, ya que solo estamos probando si todo funciona bien por ahora.

Si todo termina exitosamente, después de quitar todas las capas e iniciar el contenedor, deberías notar que ASF inició correctamente e informó que no hay bots definidos, lo que es bueno - hemos verificado que ASF en docker funciona correctamente. Presiona `CTRL+P` y luego `CTRL+Q` para cerrar el contenedor docker en primer plano, luego detén el contenedor ASF con `docker stop asf`.

Si observas más de cerca el comando entonces notarás que no declaramos ninguna etiqueta, que automáticamente está predeterminada a `latest`. Si quieres usar otra etiqueta que no sea `latest`, por ejemplo `released`, entonces debes declararla explícitamente:

```shell
docker run -it --name asf --pull always --rm justarchi/archisteamfarm:released
```

---

## Usando un volumen

Si usas ASF en contenedor docker obviamente necesitas configurar el programa. Puedes hacerlo de varias formas, pero la recomendada sería crear el directorio `config` de ASF en la máquina local, luego montarla como un volumen compartido en el contenedor docker de ASF.

Por ejemplo, supongamos que tu carpeta de configuraciones de ASF está en el directorio `/home/archi/ASF/config`. Este directorio contiene `ASF.json` así como los bots que queremos ejecutar. Ahora todo lo que tenemos que hacer es adjuntar ese directorio como volumen compartido en nuestro contenedor docker, donde ASF espera que esté su directorio de configuraciones (`/app/config`).

```shell
docker run -it -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Y eso es todo, ahora el contenedor docker ASF usará el directorio compartido con tu máquina local en modo lectura-escritura, que es todo lo que necesitas para configurar ASF. De forma similar puedes montar otros volúmenes que te gustaría compartir con ASF, tal como `/app/logs` o `/app/plugins`.

Por supuesto, esta solo es una forma específica de lograr lo que queremos, nada te impide, por ejemplo, crear tu propio `Dockerfile` que copiará tus archivos de configuración en el directorio `/app/config` dentro del contenedor docker ASF. Solo estamos cubriendo el uso básico en esta guía.

### Permisos del volumen

Por defecto ASF se ejecuta con el usuario `root` predeterminado dentro de un contenedor. Esto no es un problema en cuanto a seguridad, debido a que ya estamos dentro del contenedor Docker, pero sí afecta al volumen compartido ya que los archivos recién generados normalmente serán propiedad de `root`, lo que puede no ser la situación deseada cuando se usa un volumen compartido.

Docker te permite pasar la **[bandera](https://docs.docker.com/engine/reference/run/#user)** `--user` al comando `docker run` lo que definirá el usuario predeterminado bajo el que se ejecutará ASF. Puedes comprobar tu `uid` y `gid` por ejemplo con el comando `id`, luego pasarlo al resto del comando. Por ejemplo, si tu usuario objetivo tiene `uid` y `gid` de 1000:

```shell
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf --pull always justarchi/archisteamfarm
```

Recuerda que por defecto el directorio `/app` usado por ASF sigue siendo propiedad de `root`. Si ejecutas ASF con un usuario personalizado, entonces tu proceso de ASF no tendrá acceso de escritura a sus propios archivos. Este acceso no es obligatorio para la operación, pero es crucial, por ejemplo, para la función de actualizaciones automáticas. Para arreglar esto, basta con cambiar la propiedad de todos los archivos de ASF del predeterminado `root` a tu nuevo usuario personalizado.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

Esto solo tiene que hacerse una vez después de crear tu contenedor con `docker run`, y solo si decidiste usar un usuario personalizado para el proceso de ASF. Tampoco olvides cambiar el argumento `1000:1000` en los dos comandos al `uid` y `gid` con los que en realidad quieres ejecutar ASF.

---

## Sincronización de múltiples instancias

ASF incluye soporte para la sincronización de múltiples instancias, como se menciona en la sección **[compatibilidad](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-es-es#m%C3%BAltiples-instancias)**. Al ejecutar ASF en el contenedor docker, opcionalmente puedes "participar" en el proceso, en caso de que estés ejecutando múltiples contenedores con ASF y quieres que se sincronicen entre sí.

Por defecto, cada ASF ejecutándose dentro de un contenedor es independiente, lo que significa que no hay sincronización alguna. Para habilitar la sincronización entre ellos, debes enlazar la ruta `/tmp/ASF` en cada contenedor de ASF que quieras que se sincronice, a una ruta compartida en tu host docker, en modo lectura-escritura. Esto se logra exactamente igual que enlazar un volumen, lo que se describió anteriormente, solo que con diferentes rutas:

```shell
mkdir -p /tmp/ASF-g1
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 --pull always justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 --pull always justarchi/archisteamfarm
# Y así sucesivamente, ahora todos los contenedores de ASF están sincronizados entre sí
```

Recomendamos enlazar el directorio de ASF `/tmp/ASF` también al directorio temporal `/tmp` en tu máquina, pero eres libre de elegir cualquier otro que se ajuste a tu uso. Cada contenedor de ASF que se espera que esté sincronizado debería tener su directorio `/tmp/ASF` compartido con otros contenedores que forman parte del mismo proceso de sincronización.

Como probablemente hayas notado del ejemplo anterior, también es posible crear dos o más "grupos de sincronización", vinculando diferentes rutas del host docker al directorio `/tmp/ASF` de ASF.

Montar `/tmp/ASF` es completamente opcional y no recomendable, a menos que explícitamente quieras sincronizar dos o más contenedores de ASF. No recomendamos montar `/tmp/ASF` para uso de un solo contenedor, ya que no aporta ningún beneficio si esperas ejecutar solo un contenedor de ASF, y en realidad podría causar problemas que de otro modo podrían evitarse.

---

## Argumentos de la línea de comandos

ASF permite pasar **[argumentos de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-es-es)** en el contenedor docker a través de variables de entorno. Debes usar variables de entorno específicas para los parámetros soportados, y `ASF_ARGS` para el resto. Esto se puede lograr con el parámetro `-e` añadido a `docker run`, por ejemplo:

```shell
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--process-required" --name asf --pull always justarchi/archisteamfarm
```

Esto pasará correctamente tu argumento `--cryptkey` al proceso de ASF que se ejecuta dentro del contenedor docker, así como otros argumentos. Por supuesto, si eres un usuario avanzado también puedes modificar `ENTRYPOINT` o añadir `CMD` y pasar tus argumentos personalizados.

A menos que quieras proporcionar una clave de cifrado personalizada u otras opciones avanzadas, normalmente no necesitas incluir variables de entorno especiales, debido a que nuestros contenedores docker ya están configurados para ejecutarse con opciones predeterminadas de `--no-restart` `--process-required` `--system-required`, así que como puedes ver nuestros `ASF_ARGS` arriba son redundantes en este caso, y solo `ASF_CRYPTKEY` es relevante.

---

## IPC

Para usar IPC, primero debes cambiar la **[propiedad de configuración global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#configuraci%C3%B3n-global)** `IPC` a `true`. Además, **debes** modificar la dirección de escucha por defecto de `localhost`, ya que docker no puede enrutar el tráfico externo a la interfaz loopback. Un ejemplo de configuración que escuchará en todas las interfaces sería: `http://*:1242`. Claro, también puedes usar enlaces más restrictivos, como solo LAN local o red VPN, pero tiene que ser una ruta accesible desde el exterior - `localhost` no funcionará, ya que la ruta está enteramente en la máquina invitado.

Para hacer lo anterior debes usar una **[configuración personalizada de IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es#configuraci%C3%B3n-personalizada)** como la siguiente:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

Una vez que establezcamos IPC en una interfaz no-loopback, necesitamos decirle al docker que mapee el puerto `1242/tcp` de ASF con el parámetro `-P` o `-p`.

Por ejemplo, este comando expondría la interfaz IPC de ASF a la máquina huésped (solo):

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf --pull always justarchi/archisteamfarm
```

Si estableciste todo correctamente, el comando `docker run` hará que la interfaz **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-es-es)** funcione desde tu máquina anfitrión, en la ruta estándar `localhost:1242` que ahora está redirigida correctamente a tu máquina huésped. También es bueno notar que no exponemos de más esta ruta, así la conexión se puede hacer solo dentro del docker huésped, y por lo tanto mantenerla segura. Por supuesto, puedes exponer la ruta aún más si sabes lo que haces y aplicas las medidas de seguridad apropiadas.

---

### Ejemplo completo

Combinando todo el conocimiento anterior, un ejemplo de configuración completa se vería así:

```shell
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf --pull always justarchi/archisteamfarm
```

Esto asume que usarás un solo contenedor de ASF, con todos los archivos de configuración de ASF en `/home/archi/asf`. Debes modificar la ruta de configuración a la que coincide con tu máquina. Esta configuración también está lista para el uso opcional de IPC si decidiste incluir `IPC.config` en tu directorio de configuración con un contenido como el siguiente:

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

---

## Consejos avanzados

Cuando ya tengas listo tu contenedor docker ASF, no tienes que usar `docker run` cada vez. Fácilmente puedes detener/iniciar el contenedor docker ASF con `docker stop asf` y `docker start asf`. Ten en cuenta que si no estás usando la etiqueta `latest` entonces usar ASF actualizado requerirá que ejecutes de nuevo los comandos `docker stop`, `docker rm` y `docker run`. Esto es porque debes reconstruir tu contenedor a partir de una imagen docker ASF cada vez que quieras usar una versión de ASF incluida en esa imagen. En la etiqueta `latest`, ASF tiene la capacidad integrada para actualizarse automáticamente, así que reconstruir la imagen no es necesario para usar ASF actualizado (pero es buena idea hacerlo de vez en cuando para usar un .NET Core runtime y sistema operativo subyacente frescos).

Como se ha insinuado arriba, ASF en una etiqueta diferente de `latest` no se actualizará automáticamente, lo que significa que **tú** eres responsable de usar un repositorio `justarchi/archisteamfarm` actualizado. Esto tiene muchas ventajas, ya que normalmente la aplicación no debe tocar su propio código cuando se ejecuta, pero también entendemos la conveniencia que viene de no tener que preocuparse por la versión de ASF en tu contenedor docker. Si te importan las buenas prácticas y un uso adecuado del docker, sugerimos usar la etiqueta `released` en lugar de `latest`, pero si no quieres molestarte con eso y solo quieres hacer que ASF funcione y se actualice automáticamente, entonces la etiqueta `latest` servirá.

Normalmente deberías ejecutar ASF en el contenedor docker con la configuración global `Headless: true`. Esto claramente le dirá a ASF que no podrás proporcionar los datos faltantes y que no debe pedirlos. Por supuesto, para la configuración inicial debes considerar dejar esa opción en `false` para que puedas configurar las cosas fácilmente, pero a largo plazo normalmente no estás atado a la consola de ASF, por lo tanto tendría sentido informar a ASF sobre eso y usar el **[comando](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-es-es)** `input` si surge la necesidad. De está forma ASF no tendrá que esperar infinitamente una interacción del usuario que nunca sucederá (y desperdiciar recursos mientras tanto). También permitirá que ASF se ejecute en modo no interactivo dentro del contenedor, lo que es crucial, por ejemplo, en lo que respecta al reenvío de señales, haciendo posible que ASF se cierre adecuadamente con la solicitud `docker stop asf`.