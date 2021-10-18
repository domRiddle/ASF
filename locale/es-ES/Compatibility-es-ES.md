# Compatibilidad

ASF es una aplicación C# que se ejecuta en la plataforma .NET Core. Esto significa que ASF no se compila directamente en el **[código de máquina](https://es.wikipedia.org/wiki/Lenguaje_de_m%C3%A1quina)** que se ejecuta en tu PC, sino en **[CIL](https://es.wikipedia.org/wiki/Common_Intermediate_Language)** que requiere un tiempo de ejecución compatible con CIL para ejecutarse.

Este enfoque tiene una enorme cantidad de ventajas, ya que CIL es independiente de las plataformas, por lo que ASF puede ejecutarse nativamente en muchos sistemas operativos, especialmente Windows, Linux y OS X. No solo no es necesaria la emulación, sino que también hay soporte para todas las optimizaciones relacionadas con plataformas y hardware, tal como instrucciones CPU SSE. Gracias a eso, ASF puede lograr rendimiento y optimización superiores, al mismo tiempo que ofrece compatibilidad y fiabilidad perfectas.

Esto también significa que ASF **no tiene requisitos específicos de sistema operativo**, porque requiere **runtime** funcionando en ese sistema operativo y no el sistema operativo en sí. Siempre que runtime ejecute correctamente el código de ASF, no importa si el sistema operativo subyacente es Windows, Linux, OS X, BSD, Sony Playstation 4, Nintendo Wii o tu tostadora - siempre que exista **[.NET Core para el dispositivo](https://github.com/dotnet/core-setup#daily-builds)**, también hay **[ASF](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** para este.

Sin embargo, independientemente de dónde ejecutes ASF, debes asegurarte de que tu plataforma objetivo tiene los **[prerrequisitos .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados. Estos prerrequisitos son bibliotecas de bajo nivel requeridas para una correcta funcionalidad de runtime y primordiales para el funcionamiento de ASF. Es muy probable que tengas algunas (o incluso todas) ya instaladas.

---

## Paquetes de ASF

ASF viene en 2 sabores principales - paquete genérico y específico de SO. En cuanto a funcionalidad ambos paquetes son exactamente iguales, ambos son capaces de actualizarse automáticamente. La única diferencia entre ellos es si el paquete **genérico** de ASF también viene con runtime **de sistema operativo específico**.

---

### Genérico

El paquete genérico es una compilación que no depende de plataformas y no incluye ningún código de máquina específico. Esta configuración requiere que tengas .NET Core runtime instalado en su sistema operativo **en su versión apropiada**. Todos sabemos lo problemático que es mantener las dependencias actualizadas, por lo tanto este paquete está aquí principalmente para las personas que **ya usan** .Net Core y no quiere duplicar su runtime solo por ASF si pueden hacer uso de lo que ya tienen instalado. El paquete genérico también te permite ejecutar ASF **en cualquier lugar donde puedas obtener una implementación funcional de .NET Core runtime**, independientemente de si existe o no una compilación de ASF específica para el sistema operativo.

No se recomienda usar el sabor genérico si eres un usuario casual o incluso avanzado que solo quiere hacer funcionar ASF y no entrar en detalles técnicos de .NET Core. En otras palabras - si sabes que es esto, lo puedes usar, de lo contrario es mucho mejor usar un paquete específico de SO.

#### Paquete .NET Framework

Además del paquete genérico mencionado antes, también hay un paquete `generic-netf` construido en .NET Framework (no .NET Core). Este paquete es una variante legacy que proporciona compatibilidad faltante conocida de los tiempos de ASF V2, y puede ejecutarse, por ejemplo, con **[Mono](https://www.mono-project.com)**, mientras que el paquete `generic` de .NET Core no puede hacer eso al día de hoy.

En general, deberías **evitar este paquete como sea posible**, ya que la mayoría de los sistemas operativos y configuraciones están perfectamente (y mucho mejor) soportados con el paquete `generic` mencionado antes. De hecho, este paquete tiene sentido usarlo solo en plataformas que no tienen .NET Core runtime funcional, mientras que tienen implementación Mono funcional. Algunos ejemplos de tales plataformas incluyen `linux-x86` (32-bit i386/i686 linux), así como `linux-armel` (tarjetas ARMv6 encontradas, por ejemplo, en Raspberry Pi 0 & 1).

A medida que el tiempo pasa más plataformas son soportadas por .NET Core y hay menos compatibilidad entre .NET Framework y .NET Core, el paquete `generic-netf` será totalmente reemplazado por `generic` en el futuro. Por favor, absténte de usarlo si en su lugar puedes usar cualquier paquete .NET Core, ya que `generic-netf` carece de mucha funcionalidad y compatibilidad en comparación con las versiones .NET Core, y solo será menos funcional mientras pasa el tiempo. Ofrecemos soporte para este paquete **solo** en máquinas que no pueden usar la variante `generic` antes mencionada (por ejemplo, `linux-x86`), y solo con runtime actualizado (por ejemplo, el más reciente Mono).

---

### Sistema operativo específico

El paquete específico de SO, aparte del código incluido en el paquete genérico, también incluye código nativo para una plataforma dada. En otras palabras, el paquete de sistema operativo específico **ya incluye el adecuado .NET Core runtime**, lo que permite omitir todo el lío de la instalación y en su lugar ejecutar ASF directamente. El paquete de SO específico, como puedes adivinar por el nombre, es específico del SO y cada SO requiere su propia versión - por ejemplo, Windows requiere el binario PE32+ `ArchiSteamFarm.exe` mientras que Linux funciona con el binario ELF `ArchiSteamFarm`. Como debes saber, esos dos tipos no son compatibles entre sí.

ASF actualmente está disponible en las siguientes variantes específicas de SO:

- `win-x64` funciona con sistemas operativos Windows de 64 bits. Esto incluye Windows 7 (SP1+), 8.1, 10, Server 2012 R2, 2016, así como las futuras versiones.
- `linux-arm` funciona en sistemas operativos GNU/Linux basados en ARM de 32 bits (ARMv7+). Esto incluye plataformas tales como Raspberry Pi 2 (y más recientes) con todos los sistemas operativos GNU/Linux disponibles para ellos (tal como Raspbian), en presentes y futuras versiones. Esta variante no funcionará con arquitecturas ARM antiguas, tal como ARMv6 encontrada en Raspberry Pi 0 y 1, tampoco funcionará con sistemas operativos que no implementen el entorno GNU/Linux requerido (tal como Android).
- `linux-arm64` funciona en sistemas operativos GNU/Linux basados en ARM de 64 bits (ARMv8+). Esto incluye plataformas tales como Raspberry Pi 3 (y más recientes) con todos los sistemas operativos GNU/Linux disponibles para ellos (tal como Debian), en presentes y futuras versiones. Esta variante no funcionará con sistemas operativos de 32 bits que no tengan las bibliotecas de 64 bits requeridas (tal como Raspbian), tampoco funcionará con sistemas operativos que no implementen el entorno GNU/Linux requerido (tal como Android).
- `linux-x64` funciona en sistemas operativos GNU/Linux de 64 bits. Esto incluye Alpine, CentOS/Fedora/RHEL, Debian/Ubuntu/Linux Mint, OpenSUSE/SLES y muchos otros, incluyendo sus derivados, en versiones actuales y futuras.
- `osx-x64` funciona en sistemas operativos OS X de 64 bits. Esto incluye 10.13, así como futuras versiones.

Por supuesto, incluso si no tienes un paquete de SO específico disponible para tu combinación SO-arquitectura, siempre puedes instalar el .NET Core runtime adecuado y ejecutar el sabor genérico de ASF, que también es la razón principal por la que existe en primer lugar. La compilación genérica de ASF no depende de la plataforma y se ejecutará en cualquier plataforma que tenga .NET Core runtime funcional. Es importante notar esto - ASF requiere .NET Core runtime, no un sistema operativo específico o arquitectura. Por ejemplo, si estás ejecutando Windows de 32 bits a pesar no haber una versión `win-x86` de ASF, simplemente puedes instalar .NET Core SDK en la versión `win-x86` y ejecutar ASF genérico sin problemas. Simplemente no podemos apuntar a cada combinación de SO-arquitectura que exista y sea usada por alguien, así que tenemos que trazar la línea en algún punto. x86 es un buen ejemplo de esa línea, ya que es una arquitectura obsoleta al menos desde 2004.

Para una lista completa de todas las plataformas y sistemas operativos soportados por .NET Core 5.0, visita las **[notas de lanzamiento](https://github.com/dotnet/core/blob/master/release-notes/5.0/5.0-supported-os.md)**.

---

## Requisitos de runtime

Si estás usando un paquete de sistema operativo específico entonces no necesitas preocuparte por los requisitos de runtime, porque ASF siempre se publica con el runtime requerido y actualizado que funcionará correctamente siempre que tengas los **[prerrequisitos de .NET Core](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** instalados y actualizados. En otras palabras, **no necesitas instalar .NET Core runtime o SDK**, ya que las compilaciones de SO específico solo requieren las dependencias nativas del SO (prerrequisitos) y nada más.

Sin embargo, si estás intentando ejecutar un paquete **genérico** de ASF entonces debes asegurarte de que tu .NET Core runtime soporta la plataforma requerida por ASF.

ASF como programa actualmente se basa en **.NET 5.0** (`net5.0`), pero podría usar una plataforma más nueva en el futuro. `net5.0` es soportado desde 5.0.100 SDK (5.0.0 runtime), pero ASF está configurado para usar el **último runtime al momento de la compilación**, así que debes asegurarte de que tienes el **[último SDK](https://dotnet.microsoft.com/download)** (o al menos runtime) disponible para tu máquina. La variante genérica de ASF podría negarse a ejecutar si tu runtime en más antiguo que el mínimo	(objetivo) conocido durante la compilación.

En caso de duda, revisa nuestros **[usos de integración continua](https://github.com/JustArchiNET/ArchiSteamFarm/actions/workflows/publish.yml?query=branch%3Amain)** para compilar y publicar versiones de ASF en GitHub. Puedes encontrar la salida `dotnet --info` en cada compilación como parte del paso de verificación de .NET.