# Configuración de alto rendimiento

Esto es exactamente lo contrario de la **[configuración de bajo uso de memoria](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-es)** y normalmente quieres seguir esos consejos si quieres aumentar el rendimiento de ASF (en términos de velocidad de CPU), por el potencial costo de mayor uso de memoria.

* * *

ASF ya intenta preferir el rendimiento cuando se trata de un ajuste general equilibrado, por lo tanto no hay mucho que puedas hacer para aumentar su rendimiento, aunque tampoco estás completamente fuera de opciones. Sin embargo, ten en cuenta que esas opciones no están habilitadas por defecto, lo que significa que no son lo suficientemente buenas para considerarlas equilibradas para la mayoría de los usos, por lo tanto debes decidir si el uso aumentado de memoria que ofrecen es aceptable para ti.

* * *

## Ajustes de runtime (avanzado)

Lo siguientes trucos **involucran un serio aumento de uso de memoria** y deben ser usados con precaución.

`ArchiSteamFarm.runtimeconfig.json` te permite ajustar el runtime de ASF, especialmente permitiéndote cambiar entre server GC y workstation GC.

> El recolector de basura es autoajustable y puede trabajar en una amplia variedad de escenarios. Puedes usar un archivo de configuración para establecer el tipo recolector de basura basado en las características de la carga de trabajo. El CLR proporciona los siguientes tipos de recolección de basura: - Recolección de basura de estación de trabajo, que es para todas las estaciones de trabajo y PC independientes. Esta es la configuración predeterminada para el elemento `<gcServer>` en el esquema de configuración de runtime. - Recolección de basura de servidor, que está destinada para aplicaciones de servidor que necesitan alto rendimiento y escalabilidad. La recolección de basura de servidor puede ser no concurrente o en segundo plano.

Puedes leer más en **[fundamentos de la recolección de basura](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)**.

ASF usa la recolección de basura de estación de trabajo por defecto. Esto se debe principalmente a un buen equilibrio entre uso de memoria y rendimiento, lo que es más que suficiente para solo unos bots, y normalmente un solo hilo GC concurrente en segundo plano es lo suficientemente rápido para manejar toda la memoria asignada por ASF.

Sin embargo, hoy tenemos muchos núcleos de CPU de los que ASF se puede beneficiarse en gran medida, teniendo un hilo GC dedicado por cada CPU vCore que esté disponible. Esto puede mejorar enormemente el rendimiento durante tareas pesadas de ASF tal como analizar las páginas de inventario o el inventario, ya que cada CPU vCore puede ayudar, en lugar de solo 2 (principal y GC). Servidor GC se recomienda para máquinas con 3 CPU vCores y más, estación de trabajo GC es forzado automáticamente si tu máquina solo tiene 1 CPU vCore, si tienes exactamente 2 entonces puedes considerar probar ambos (los resultados pueden variar).

Puedes activar el recolector de basura de servidor cambiando la propiedad `System.GC.Server` de `ArchiSteamFarm.runtimeconfig.json` de `false` a `true`. Ten en cuenta que podrías necesitar hacerlo más de una vez, ya que ASF seguirá usando `false` por defecto después de la actualización automática.

El servidor GC en sí no da como resultado un aumento de la memoria muy grande solo por estar activo, pero tiene tamaños de generación mucho más grandes, y por lo tanto es mucho más perezoso cuando se trata de devolver memoria al sistema operativo. Puede que te encuentres en un buen lugar donde el servidor GC aumenta su rendimiento significativamente y te gustaría seguir usándolo, pero al mismo tiempo no puedes permitirte ese gran aumento de memoria que viene con su uso. Luckily for you, there is a "best of both worlds" setting, by using server GC with **[GC latency level](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** set to `0`, which will still enable server GC, but limit generation sizes and focus more on memory.

However, if memory is not a problem for you (as GC still takes into account your available memory and tweaks itself), it's much better to not change `GCLatencyLevel` at all, achieving superior performance in result.

* * *

## Optimización recomendada

- Ensure that you're using default value of `OptimizationMode` which is `MaxPerformance`. This is by far the most important setting, as using `MinMemoryUsage` value has dramatic effects on performance.
- Enable server GC by switching `System.GC.Server` property of `ArchiSteamFarm.runtimeconfig.json` from `false` to `true`. This will enable server GC which can be immediately seen as being active by memory increase compared to workstation GC.
- If you can't afford that much memory increase, consider using `GCLatencyLevel` of `0` to achieve "the best of both worlds". However, if your memory can afford it, then it's better to keep it at default - server GC already tweaks itself during runtime and is smart enough to use less memory when your OS will truly need it.

If you've enabled server GC and kept `GCLatencyLevel` at default setting, then you have superior ASF performance that should be blazing fast even with hundreds or thousands of enabled bots. CPU should not be a bottleneck anymore, as ASF is able to use your entire CPU power when needed, cutting required time to bare minimum. The next step would be CPU and RAM upgrades.