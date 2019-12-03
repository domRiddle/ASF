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

El servidor GC en sí no da como resultado un aumento de la memoria muy grande solo por estar activo, pero tiene tamaños de generación mucho más grandes, y por lo tanto es mucho más perezoso cuando se trata de devolver memoria al sistema operativo. Puede que te encuentres en un buen lugar donde el servidor GC aumenta su rendimiento significativamente y te gustaría seguir usándolo, pero al mismo tiempo no puedes permitirte ese gran aumento de memoria que viene con su uso. Afortunadamente, hay una configuración que tiene lo "mejor de ambos mundos", usando la confugración del servidor GC con **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-es#gclatencylevel)** establecido a `0`, lo que habilitará la recolección de elementos no utilizados de servidor, pero limitará la generación de tamaños y se enfocará más en la memoria. Alternativamente, también puedes experimentar con otra configuración, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-es#gcheaphardlimitpercent)**, o incluso con ambas al mismo tiempo.

Sin embargo, si la memoria no es un problema (ya que la recolección de elementos no utilizados toma en cuenta la memoria disponible y se ajusta automáticamente), es mucho mejor idea no cambiar esos ajustes en absoluto, logrando un mejor rendimiento como resultado.

* * *

## Optimización recomendada

- Asegúrate de que estás el valor predeterminado de `OptimizationMode` que es `MaxPerformance`. Este es por mucho el ajuste más importante, ya que usar el valor `MinMemoryUsage` tiene efectos dramáticos en el rendimiento.
- Habilita servidor GC cambiando la propiedad `System.GC.Server` de `ArchiSteamFarm.runtimeconfig.json` de `false` a `true`. Esto habilitará el servidor GC que se puede ver inmediatamente como activo por el aumento de memoria comparación con estación de trabajo GC.
- Si no puedes permitirte ese incremento de memoria, considera ajustar **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gclatencylevel)** y/o **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup#gcheaphardlimitpercent)** para lograr lo "mejor de ambos mundos". Sin embargo, si tu memoria se lo puede permitir, entonces es mejor mantenerlo en predeterminado - servidor GC ya se ajusta a sí mismo durante runtime y es lo suficientemente inteligente para usar menos memoria cuando tu sistema operativo realmente lo necesite.

Si habilitaste la recolección de elementos no utilizados de servidor (server GC) y dejaste la configuración en sus valores por defecto, entonces tienes un rendimiento de ASF superior que debería funcionar muy rápido incluso con cientos o miles de bots activos. La CPU ya no debe hacer cuello de botella, ya que ASF es capaz de usar todo el poder de CPU cuando sea necesario, reduciendo el tiempo requerido al mínimo. El siguiente paso sería actualizar la CPU y RAM.