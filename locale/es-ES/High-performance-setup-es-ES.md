# Configuración de alto rendimiento

Esto es exactamente lo contrario a la **[configuración de poca memoria](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-ES)** y normalmente quieres seguir estos consejos si deseas aumentar el rendimiento de ASF (en términos de velocidad de CPU), por el potencial costo de un mayor uso de memoria.

---

ASF ya intenta preferir el rendimiento cuando se trata de un ajuste general equilibrado, por lo tanto no hay mucho que puedas hacer para aumentar su rendimiento, aunque tampoco estás completamente sin opciones. Sin embargo, ten en cuenta que esas opciones no están habilitadas por defecto, lo que significa que no son lo suficientemente buenas para considerarlas equilibradas para la mayoría de los usos, por lo tanto debes decidir si el aumento de memoria que ofrecen es aceptable para ti.

---

## Ajuste de runtime (avanzado)

Los siguientes trucos **involucran un importante aumento de uso de memoria** y deben ser usados con precaución.

.NET runtime te permite **[modificar el recolector de basura](https://docs.microsoft.com/es-es/dotnet/core/run-time-config/garbage-collector)** de muchas formas, ajustando eficazmente el proceso de recolección de basura de acuerdo a tus necesidades.

La forma recomendada de aplicar estas configuraciones es a través de las propiedades de entorno `COMPlus_`. Por supuesto, también podrías usar otros métodos, por ejemplo, `runtimeconfig.json`, pero algunas configuraciones son imposibles de establecer de esta manera, encima de eso ASF reemplazará tu `runtimeconfig.json` personalizado en la siguiente actualización, por lo tanto recomendamos propiedades de entorno que puedas establecer fácilmente antes de ejecutar el proceso.

Consulta la documentación para todas las propiedades que puedes utilizar, a continuación mencionaremos las más importantes (en nuestra opinión):

### [`gcServer`](https://docs.microsoft.com/es-es/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection)

> Configura si la aplicación usa la recolección de basura de estación de trabajo o de servidor.

Puedes leer los específicos de la recolección de basura de servidor en **[fundamentos de la recolección de basura](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals)**.

ASF usa la recolección de basura de estación de trabajo por defecto. Esto se debe principalmente a un buen equilibrio entre uso de memoria y rendimiento, lo que es más que suficiente para algunos bots, ya que normalmente un solo subproceso de recolección de basura concurrente en segundo plano es lo suficientemente rápido para manejar toda la memoria asignada por ASF.

Sin embargo, hoy tenemos muchos núcleos de CPU de los que ASF puede beneficiarse en gran medida, teniendo un subproceso de recolección de basura dedicado por cada CPU vCore que esté disponible. Esto puede mejorar enormemente el rendimiento durante tareas pesadas de ASF tal como analizar las páginas de insignias o el inventario, ya que cada CPU vCore puede ayudar, en lugar de solo 2 (el principal y el de recolección de basura). El recolector de basura de servidor se recomienda para máquinas con 3 CPU vCores y más, el recolector de basura de estación de trabajo es forzado automáticamente si tu máquina solo tiene 1 CPU vCore, si tienes exactamente 2 entonces puedes considerar probar ambos (los resultados pueden variar).

El recolector de basura de servidor por sí mismo no da como resultado un aumento de memoria muy grande solo por estar activo, pero tiene tamaños de generación mucho más grandes, y por lo tanto es mucho más lento cuando se trata de devolver memoria al sistema operativo. Puede que te encuentres en un punto óptimo en el que el recolector de basura de servidor aumenta su rendimiento significativamente y te gustaría seguir usándolo, pero al mismo tiempo no puedes permitirte ese gran aumento de memoria que viene con su uso. Afortunadamente, hay una configuración que tiene lo "mejor de ambos mundos", usando la recolección de basura de servidor con la propiedad de configuración **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-ES#gclatencylevel)** establecida a `0`, lo que aún habilitará la recolección de basura de servidor, pero limitará los tamaños de generación y se enfocará más en la memoria. Alternativamente, también puedes experimentar con otra propiedad, **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-ES#gcheaphardlimitpercent)**, o incluso con ambas al mismo tiempo.

Sin embargo, si la memoria no es un problema (ya que el recolector de basura toma en cuenta la memoria disponible y se ajusta automáticamente), es mucho mejor idea no cambiar esas propiedades, logrando un rendimiento superior como resultado.

---

Puedes habilitar todas las propiedades del recolector de basura estableciendo las variables de entorno `COMPlus_` apropiadas. Por ejemplo, en Linux (shell):

```shell
export COMPlus_gcServer=1

./ArchiSteamFarm # Para compilación de sistema operativo específico
```

O en Windows (powershell):

```powershell
$Env:COMPlus_gcServer=1

.\ArchiSteamFarm.exe # Para compilación de sistema operativo específico
```

---

## Optimización recomendada

- Asegúrate de que estás usando el valor predeterminado de `OptimizationMode` que es `MaxPerformance`. Este es por mucho el ajuste más importante, ya que usar el valor `MinMemoryUsage` tiene efectos dramáticos en el rendimiento.
- Habilita el recolector de basura de servidor. El recolector de basura de servidor se puede notar que está activo inmediatamente por un aumento significativo de la memoria en comparación con el recolector de basura de estación de trabajo.
- Si no puedes permitirte ese incremento de memoria, considera ajustar **[`GCLatencyLevel`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-ES#gclatencylevel)** y/o **[`GCHeapHardLimitPercent`](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Low-memory-setup-es-ES#gcheaphardlimitpercent)** para lograr lo "mejor de ambos mundos". Sin embargo, si tu memoria se lo puede permitir, entonces es mejor mantenerlo en predeterminado - el recolector de basura de servidor ya se ajusta a sí mismo durante el tiempo de ejecución y es lo suficientemente inteligente para usar menos memoria cuando tu sistema operativo realmente la necesite.

Si habilitaste el recolector de basura de servidor y dejaste las otras propiedades de configuración en sus valores por defecto, entonces tienes un rendimiento superior de ASF que debería funcionar muy rápido incluso con cientos o miles de bots activos. La CPU ya no debería tener un cuello de botella, ya que ASF es capaz de usar todo el poder de la CPU cuando sea necesario, reduciendo el tiempo requerido al mínimo. El siguiente paso sería actualizar la CPU y RAM.