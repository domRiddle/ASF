# SteamTokenDumperPlugin

`SteamTokenDumperPlugin` es un **[plugin](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Plugins-es-es)** oficial para ASF, disponible a partir de ASF V4.2.2, desarrollado por nosotros, el cual te permite contribuir al proyecto **[SteamDB](https://steamdb.info)** compartiendo tokens de paquetes, tokens de apps y "depot keys" a los que tiene acceso tu cuenta de Steam. La información extendida de los datos recolectados y por qué SteamDB los necesita se puede encontrar en la página **[Token Dumper](https://steamdb.info/tokendumper)** de SteamDB. Los datos enviados no incluyen ninguna información potencialmente sensible, y no presenta riesgo de seguridad/privacidad, como se indica en la descripción previa.

---

## Habilitando el plugin

ASF viene con `SteamTokenDumperPlugin` incluido, pero el plugin en sí está deshabilitado por defecto. Puedes habilitar el plugin estableciendo la propiedad de configuración global de ASF `SteamTokenDumperPluginEnabled` al valor `true`, en sintaxis JSON:

```json
{
  "SteamTokenDumperPluginEnabled": true
}
```

Al ejecutarse ASF, el plugin te hará saber si fue habilitado exitosamente a través del mecanismo estándar de registro de ASF. También puedes habilitar el plugin a través de nuestro generador de configuración web.

---

## Detalles técnicos

Después de habilitarlo, el plugin usará los bots que estés ejecutando en ASF para recolectar datos en forma de tokens de paquetes, tokens de aplicaciones y "depot keys" a los que tienen acceso tus bots. El módulo de recolección de datos incluye rutinas pasivas y activas que deben minimizar la sobrecarga adicional causada por la recolección de datos.

Para lograr el caso de uso previsto, además de la rutina de recolección de datos explicada anteriormente, la rutina de envío es inicializada como responsable de determinar qué datos necesitan ser enviados a SteamDB de forma periódica. Esta rutina se iniciará hasta `1` hora después de la ejecución de ASF, y se repetirá cada `24` horas. El plugin hará todo lo posible para minimizar la cantidad de datos que necesitan ser enviados, por lo que es posible que alguna información recolectada por el plugin sea marcada como no necesaria para enviar, y por lo tanto será omitida (por ejemplo, actualizaciones de la aplicación que no cambian el token de acceso).

El plugin utiliza una base de datos de caché persistente guardada en la ubicación `config/SteamTokenDumper.cache`, que tiene un propósito similar a `config/ASF.db` para ASF. El archivo se utiliza para registrar los datos recolectados y enviados, y minimizar la cantidad de trabajo necesario entre diferentes ejecuciones de ASF. Eliminar el archivo hace que el proceso se reinicie desde cero, lo que debe evitarse si es posible.

---

## Datos

ASF incluye el `steamID` del colaborador en la solicitud, el cual se determina con el `SteamOwnerID` que estableces en ASF, o en caso de que no lo hayas hecho, el Steam ID del bot que tenga más licencias. El colaborador podría recibir algunos beneficios adicionales de SteamDB por la ayuda continua (por ejemplo, el rango de donador en el sitio web), pero eso es totalmente a discreción de SteamDB.

En cualquier caso, el personal de SteamDB te agradece de antemano por tu ayuda. Los datos enviados permiten que SteamDB funcione, en particular para rastrear información de paquetes, aplicaciones y "depots", lo que ya no sería posible sin tu ayuda.