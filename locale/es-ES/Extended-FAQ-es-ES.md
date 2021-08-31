# Preguntas frecuentes adicionales

Nuestras preguntas frecuentes adicionales cubren preguntas menos comunes y sus respuestas. Para asuntos más comunes, por favor visita nuestras **[preguntas frecuentes básicas](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ-es-es)**.

---

### ¿Quién ha creado ASF?

ASF fue creado por **[Archi](https://github.com/JustArchi)** en octubre 2015. En caso de que te lo preguntes, soy un **[usuario de Steam](https://steamcommunity.com/profiles/76561198006963719)** igual que tú. Aparte de jugar juegos, también me encanta poner en uso mis habilidades y determinación, lo que puedes explorar ahora mismo. No hay una gran empresa involucrada aquí, ningún equipo de desarrolladores y ningún presupuesto de $1M para cubrir todo eso - solo yo, arreglando cosas que no están rotas.

Sin embargo, ASF es un proyecto de código abierto, y no puedo expresar lo suficiente que no estoy detrás de todo lo que puedes ver aquí. Tenemos algunos **[otros](https://github.com/JustArchiNET?q=ASF-)** proyectos de ASF que están siendo desarrollados casi exclusivamente por otros desarrolladores. Incluso el proyecto principal de ASF tiene muchos **[colaboradores](https://github.com/JustArchiNET/ArchiSteamFarm/graphs/contributors)** que me ayudaron a hacer todo esto posible. Además, hay varios servicios de terceros que apoyan el desarrollo de ASF, especialmente **[GitHub](https://github.com)**, **[JetBrains](https://www.jetbrains.com)** y **[Crowdin](https://crowdin.com)**. Tampoco puedes olvidarte de todas las impresionantes bibliotecas y herramientas que hicieron ASF posible, tal como **[Rider](https://www.jetbrains.com/rider)** que usamos como entorno de desarrollo integrado (nos encantan las adiciones de **[ReSharper](https://www.jetbrains.com/resharper)**) y especialmente **[SteamKit2](https://github.com/SteamRE/SteamKit)** sin el cual ASF no existiría en primer lugar. ASF no estaría donde está hoy sin mis **[patrocinadores](https://github.com/sponsors/JustArchi)**, **[mecenas](https://www.patreon.com/JustArchi)** y varios donadores, apoyándome en todo lo que estoy haciendo aquí.

¡Gracias a todos por ayudar en el desarrollo de ASF! Son increíbles.

---

### ¿Por qué fue creado ASF en primer lugar?

ASF fue creado con el propósito principal de ser una herramienta de recolección de Steam completamente automatizada para Linux, sin necesidad de ninguna dependencia externa (como el cliente de Steam). De hecho, estos siguen siendo su propósito y enfoque principales, porque mi concepto de ASF no ha cambiado desde entonces y lo sigo usando exactamente de la misma manera que en el 2015. Por supuesto, ha habido **muchos** cambios desde entonces, y estoy muy contento de ver lo lejos que ha progresado ASF, mayormente gracias a sus usuarios, porque nunca habría codificado ni la mitad de las características si fuera solo para mí.

Es agradable observar que ASF nunca se hizo para competir con otros programas similares, especialmente **[Idle Master](https://www.steamidlemaster.com)**, porque ASF nunca fue diseñado para ser una aplicación de escritorio/usuario, y todavía no lo es hoy. Si analizas el propósito principal de ASF descrito antes, entonces verás cómo Idle Master es **lo contrario** de todo eso. Aunque es seguro que actualmente puedes encontrar programas similares a ASF, nada era lo bastante bueno para mí en ese entonces (y aún nada lo es hoy), así que creé mi propio software, de la forma que yo quería. Con el tiempo los usuarios han migrado a ASF principalmente debido a la robustez, estabilidad y seguridad, pero también por todas las características que he desarrollado a lo largo de estos años. Hoy, ASF es mejor que nunca.

---

### Muy bien, ¿dónde está la trampa? ¿Qué ganas al compartir ASF?

No hay trampa, creé ASF **para mí** y lo compartí con el resto de la comunidad con la esperanza de que sea útil. Exactamente lo mismo pasó en 1991, cuando Linus Torvalds **[compartió su primer kernel Linux](https://groups.google.com/forum/#!msg/comp.os.Minix/dlNtH7RRrGA/SwRavCzVE7gJ)** con el resto del mundo. No hay malware oculto, minería de datos, minería de criptomonedas o cualquier otra actividad que me genere ningún beneficio monetario. El proyecto ASF está respaldado por donaciones no obligatorias enviadas por usuarios felices como tú. Puedes usar ASF exactamente de la misma forma que yo lo uso, y si te gusta, puedes comprarme un café, mostrando tu gratitud por lo que hago.

También estoy usando ASF como un perfecto ejemplo de un moderno proyecto C# que siempre busca la perfección y mejores prácticas, sea con tecnología, administración de proyectos o el código mismo. Es mi definición de "cosas hechas bien", así que si por cualquier motivo logras aprender algo útil de mi proyecto, eso solo me hará más feliz.

---

### ¿Cómo puedo verificar que los archivos descargados son auténticos?

Como parte de nuestras versiones en GitHub, usamos un proceso de verificación muy similar al usado por **[Debian](https://www.debian.org/CD/verify)**. En cada versión oficial a partir de ASF V5.1.3.3, además de los archivos `zip` también puedes encontrar los archivos `SHA512SUMS` y `SHA512SUMS.sign` Descárgalos para fines de verificación junto con los archivos `zip` de tu elección.

Primero, debes usar el archivo `SHA512SUMS` para verificar que la suma de verificación `SHA-512` de los archivos `zip` seleccionados coincide con la que nosotros calculamos. En Linux, puedes usar la utilidad `sha512sum` para ese fin.


```
$ sha512sum -c --ignore-missing SHA512SUMS
ASF-linux-x64.zip: OK
```

En Windows, podemos hacer eso desde powershell, aunque tienes que verificar manualmente con `SHA512SUMS`:

```
PS > Get-Content SHA512SUMS | Select-String -Pattern ASF-linux-x64.zip

f605e573cc5e044dd6fadbc44f6643829d11360a2c6e4915b0c0b8f5227bc2a257568a014d3a2c0612fa73907641d0cea455138d2e5a97186a0b417abad45ed9  ASF-linux-x64.zip


PS > Get-FileHash -Algorithm SHA512 -Path ASF-linux-x64.zip

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA512          F605E573CC5E044DD6FADBC44F6643829D11360A2C6E4915B0C0B8F5227BC2A2575... ASF-linux-x64.zip
```

De esta manera nos aseguramos de que cualquier cosa que haya sido escrita en `SHA512SUMS` coincide con los archivos resultantes y de que no fueron manipulados. Sin embargo, esto aún no prueba que el archivo `SHA512SUMS` que comprobaste realmente proviene de nosotros. Para eso, usaremos el archivo `SHA512SUMS.sign`, que contiene la firma digital PGP que prueba la autenticidad de `SHA512SUMS`. Para ello podemos usar la utilidad `gpg`, tanto en **[Linux](https://gnupg.org/download/index.html)** como en **[Windows](https://gpg4win.org)** (en Windows cambia el comando `gpg` por `gpg.exe`).

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Can't check signature: No public key
```

Como puedes ver, el archivo contiene una firma válida, pero de origen desconocido. Necesitarás importar la **[clave pública](https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc)** de ArchiBot con la que firmamos las sumas `SHA-512` para una validación completa.

```
$ curl https://raw.githubusercontent.com/JustArchi-ArchiBot/JustArchi-ArchiBot/main/ArchiBot_public.asc -o ArchiBot_public.asc
$ gpg --import ArchiBot_public.asc
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key A3D181DF2D554CCF: public key "ArchiBot <ArchiBot@JustArchi.net>" imported
gpg: Total number processed: 1
gpg:               imported: 1

```

Finalmente, puedes verificar de nuevo el archivo `SHA512SUMS`:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF
```

Esto ha verificado que `SHA512SUMS.sign` contiene una firma válida de nuestra clave `224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF` para el archivo `SHA512SUMS` con el que lo has verificado.

Te estarás preguntando de dónde proviene la última advertencia. Has importado exitosamente nuestra clave, pero aún no has decidido confiar en ella. Aunque esto no es obligatorio, también podemos cubrirlo. Normalmente esto incluye verificar a través de diferentes canales (por ejemplo, llamada telefónica, mensaje SMS) que la clave es válida, luego firmar la clave con la tuya propia para confiar en ella. Para este ejemplo, puedes considerar esta entrada de la wiki como el mencionado (muy débil) diferente canal, ya que la clave original proviene del **[perfil de ArchiBot](https://github.com/JustArchi-ArchiBot)**. En cualquier caso, asumiremos que confías en la clave tal como está.

Primero, **[genera tu propia clave privada](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Generating_an_OpenPGP_Key)**, si aún no tienes una. Usaremos `--quick-gen-key` como ejemplo rápido.

```
$ gpg --batch --passphrase '' --quick-gen-key "$(whoami)"
gpg: /home/archi/.gnupg/trustdb.gpg: trustdb created
gpg: key E4E763905FAD148B marked as ultimately trusted
gpg: directory '/home/archi/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/archi/.gnupg/openpgp-revocs.d/8E5D685F423A584569686675E4E763905FAD148B.rev'
```

Ahora puedes firmar nuestra clave con la tuya para confiar en ella:

```
$ gpg --sign-key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF

pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
sub  cv25519/E527A892E05B2F38
     created: 2021-05-22  expires: never       usage: E
[ unknown] (1). ArchiBot <ArchiBot@JustArchi.net>


pub  ed25519/A3D181DF2D554CCF
     created: 2021-05-22  expires: never       usage: SC
     trust: unknown       validity: unknown
 Primary key fingerprint: 224D A6DB 47A3 935B DCC3  BE17 A3D1 81DF 2D55 4CCF

     ArchiBot <ArchiBot@JustArchi.net>

Are you sure that you want to sign this key with your
key "archi" (E4E763905FAD148B)

Really sign? (y/N) y
```

Y listo, después de confiar en nuestra clave, `gpg` ya no debería mostrar la advertencia al hacer la verificación:

```
$ gpg --verify SHA512SUMS.sign SHA512SUMS
gpg: Signature made Mon 02 Aug 2021 00:34:18 CEST
gpg:                using EDDSA key 224DA6DB47A3935BDCC3BE17A3D181DF2D554CCF
gpg: Good signature from "ArchiBot <ArchiBot@JustArchi.net>" [full]
```

Observa que el indicador de confianza `[unknown]` cambia a `[full]` una vez que has firmado nuestra clave con la tuya.

¡Felicidades, has verificado que la versión que descargaste no ha sido manipulada! 👍

---

### Es el 1º de abril y el idioma de ASF se cambió a algo extraño, ¿qué está pasando?

¡FELISIDADS X DSQBRIR NUESTR UEBO D PASKUA DL DIA D LOZ INOZENTS! Si no estableciste la opción `CurrentCulture`, entonces el 1º de abril ASF utilizará el idioma **[LOLcat](https://es.wikipedia.org/wiki/Lolcat)** en lugar del idioma de tu sistema. Si deseas desactivar ese comportamiento, puedes establecer `CurrentCulture` al idioma que te gustaría usar. También puedes activar nuestro huevo de pascua incondicionalmente, estableciendo `CurrentCulture` al valor `qps-Ploc`.