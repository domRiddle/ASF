# Sicherheit

## Verschlüsselung

ASF unterstützt derzeit die folgenden Verschlüsselungsmethoden als Parameter für `ECryptoMethod`:

| Wert | Name                        |
| ---- | --------------------------- |
| 0    | PlainText                   |
| 1    | AES                         |
| 2    | ProtectedDataForCurrentUser |
| 3    | EnvironmentVariable         |
| 4    | Datei                       |

Die genaue Beschreibung und Unterschiede zwischen diesen sind nachfolgend verfügbar.

Um ein verschlüsseltes Password zu generieren, z. B. um es mit `SteamPassword` zu verwenden, müssen Sie den `encrypt` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** mit der gewünschten Verschlüsselungsmethode und Ihrem ursprünglichen klartext Passwort ausführen. Danach setzen Sie die verschlüsselte Zeichenfolge, die Sie als `SteamPassword` Bot-Konfigurationseigenschaft haben, und ändern Sie schließlich `PasswordFormat` auf diejenige, die Ihrer gewählten Verschlüsselungsmethode entspricht. Einige Formate benötigen keinen `encrypt` Befehl, z. B. `EnvironmentVariable` oder `File`. Fügen Sie einfach den entsprechenden Pfad an.

---

### `PlainText`

Dies ist die einfachste und zugleich unsicherste Methode das Passwort zu speichern, festgelegt durch `PasswordFormat` mit einem Wert von `0`. ASF erwartet, dass die Zeichenkette ein Klartext ist – ein Passwort in seiner direkten Form. Sie ist am einfachsten zu benutzen und zu 100% kompatibel mit allen Setups, daher ist sie eine Standardmethode zum Speichern von Geheimnissen, völlig unsicher für eine sichere Speicherung.

---

### `AES`

Considered secure by today standards, **[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)** way of storing the password is defined as `ECryptoMethod` of `1`. ASF expects the string to be a **[base64-encoded](https://en.wikipedia.org/wiki/Base64)** sequence of characters resulting in AES-encrypted byte array after translation, which then should be decrypted using included **[initialization vector](https://en.wikipedia.org/wiki/Initialization_vector)** and ASF encryption key.

Die obige Methode garantiert Sicherheit, solange der Angreifer den ASF-Verschlüsselungsschlüssel nicht kennt, der sowohl zur Entschlüsselung als auch zur Verschlüsselung von Passwörtern verwendet wird. ASF erlaubt es dir, den Schlüssel mit dem **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** `--cryptkey` anzugeben, den Du für maximale Sicherheit verwenden solltest. Wenn Du dich dazu entscheidest es wegzulassen, verwendet ASF seinen eigenen Schlüssel, der **bekannt** und fest in der Anwendung programmiert ist, was bedeutet, dass jeder die ASF-Verschlüsselung umkehren und ein entschlüsseltes Passwort erhalten kann. Es erfordert immer noch etwas Aufwand und ist nicht so einfach zu bewerkstelligen, aber möglich, deshalb solltest Du fast immer `AES` Verschlüsselung mit Ihrem eigenen `--cryptkey` verwenden, den Du geheim halten solltest. Die in ASF verwendete AES-Methode bietet eine zufriedenstellende Sicherheit und es ist ein Kompromiss zwischen der Einfachheit von `PlainText` und der Komplexität von `ProtectedDataForCurrentUser`, aber es wird dringend empfohlen, sie mit benutzerdefinierten `--cryptkey` zu verwenden. Bei ordnungsgemäßer Verwendung garantiert eine angemessene Sicherheit für eine sichere Lagerung.

---

### `ProtectedDataForCurrentUser`

Die derzeit sicherste Art, die von ASF angeboten wird, das Passwort zu verschlüsseln und viel sicherer als die oben beschriebene Methode `AES`, ist definiert als `ECryptoMethod`, mit einem Wert von `2`. Der Hauptvorteil dieser Methode ist gleichzeitig der größte Nachteil – anstelle der Verwendung eines Verschlüsselungsschlüssels (wie in `AES`) werden die Daten mit den Anmeldeinformationen des aktuell angemeldeten Benutzers verschlüsselt, was bedeutet, dass es möglich ist die Daten **nur** auf dem Computer zu entschlüsseln, auf dem sie verschlüsselt wurden, und darüber hinaus **nur** durch den Benutzer der die Verschlüsselung ausgestellt hat. Dadurch wird sichergestellt, dass, wenngleich Sie Ihre komplette `Bot.json` mit verschlüsseltem `SteamPassword` an jemand anderen schicken, er das Passwort nicht ohne direkten Zugriff auf Ihren PC entschlüsseln kann. Dies ist eine ausgezeichnete Sicherheitsmaßnahme, hat aber gleichzeitig den großen Nachteil, am wenigsten kompatibel zu sein, da das mit dieser Methode verschlüsselte Passwort mit jedem anderen Benutzer und Geräte – einschließlich **Ihrem eigenen** – inkompatibel sein wird, wenn Sie sich z. B. für eine Neuinstallation des Betriebssystems entscheiden. Dennoch ist es eine der besten Methoden Passwörter zu speichern und wenn Sie sich um die Sicherheit über `PlainText` sorgen und nicht jedes Mal ein Passwort setzen wollen; dann ist dies Ihre beste Wahl, solange Sie nicht von einem anderen Gerät als Ihrem eigenen auf Ihre Konfiguration zugreifen müssen.

**Bitte beachten Sie, dass diese Option derzeit nur auf Windows Betriebssystemen zur Verfügung steht.**

---

### `EnvironmentVariable`

RAM-basierter Speicher definiert als `ECryptoMethod` von `3`. ASF liest das Passwort aus der Umgebungsvariable mit dem angegebenen Namen im Passwortfeld (z. B. `SteamPassword`). For example, setting `SteamPassword` to `ASF_PASSWORD_MYACCOUNT` and `PasswordFormat` to `3` will cause ASF to evaluate `${ASF_PASSWORD_MYACCOUNT}` environment variable and use whatever is assigned to it as the account password.

---

### `Datei`

Datei-basierter Speicher (möglicherweise außerhalb des ASF-Konfigurationsordners) definiert als `ECryptoMethod` von `4`. ASF liest das Passwort aus dem Dateipfad – spezifiziert im Passwortfeld (z. B. `SteamPassword`). The specified path can be either absolute, or relative to ASF's "home" location (the folder with `config` directory inside, taking into account `--path` **[command-line argument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments#arguments)**). This method can be used for example with **[Docker secrets](https://docs.docker.com/engine/swarm/secrets)**, which create such files for usage, but can also be used outside of Docker if you create appropriate file yourself. For example, setting `SteamPassword` to `/etc/secrets/MyAccount.pass` and `PasswordFormat` to `4` will cause ASF to read `/etc/secrets/MyAccount.pass` and use whatever is written to that file as the account password.

Denken Sie daran, sicherzustellen, dass die Datei mit dem Passwort nicht von unbefugten Benutzern gelesen werden kann, da dies den gesamten Zweck der Verwendung dieser Methode zunichtemacht.

---

## Empfehlung

Wenn die Kompatibilität für dich kein Problem ist und Du mit der Funktionsweise der `ProtectedDataForCurrentUser` Methode einverstanden bist, ist diese Option **empfohlen** um das Passwort in ASF zu speichern, da sie die beste Sicherheit bietet. Die Methode `AES` ist eine gute Wahl für Leute, die Ihre Konfigurationen immer noch auf jedem beliebigen Computer verwenden möchten, während `PlainText` die einfachste Art ist das Passwort zu speichern, wenn es Ihren nichts ausmacht, dass jeder danach in der JSON-Konfigurationsdatei suchen kann.

Bitte bedenke, dass diese 3 Methoden als **unsicher** betrachtet werden, wenn ein Angreifer Zugriff auf Ihren PC hat. ASF muss in der Lage sein das verschlüsselte Passwort zu entschlüsseln und wenn das auf deiner Maschine laufende Programm dazu in der Lage ist, dann kann auch jedes andere auf dem selben Gerätlaufende Programm dies tun. `ProtectedDataForCurrentUser` ist die sicherste Variante, da **auch andere Benutzer, die den gleichen PC verwenden, ihn nicht entschlüsseln können**, aber es ist trotzdem möglich die Daten zu entschlüsseln, wenn jemand in der Lage ist Ihre Login-Informationen und Maschineninformationen zusätzlich zur ASF-Konfigurationsdatei zu stehlen.

For advanced setups, you can utilize `EnvironmentVariable` and `File`. They have limited usability, the `EnvironmentVariable` will be a good idea if you'd prefer to obtain password through some kind of custom solution and store it in memory exclusively, while `File` is good for example with **[Docker secrets](https://docs.docker.com/engine/swarm/secrets)**. Both of them are unencrypted however, so you basically move the risk from ASF config file to whatever you pick from those two.

In addition to encryption methods specified above, it's possible to also avoid specifying passwords entirely, for example as `SteamPassword` by using an empty string or `null` value. ASF fragt Sie nach Ihrem Passwort, wenn es benötigt wird, und speichert es nirgendwo, außer im Speicher des aktuell laufenden Prozesses, bis Sie es schließen. Es ist zwar die sicherste Methode im Umgang mit Passwörtern (diese werden nirgendwo gespeichert), aber auch die lästigste, da Sie Ihr Passwort bei jedem ASF-Lauf manuell eingeben müssen (wenn es erforderlich ist). Wenn das kein Problem für dich ist ist dies die beste Wahl in Bezug auf Sicherheit.

---

## Entschlüsselung

ASF unterstützt keine Möglichkeit, bereits verschlüsselte Passwörter zu entschlüsseln, da Entschlüsselungsmethoden nur intern für den Zugriff auf die Daten innerhalb des Prozesses verwendet werden. Falls Sie die Verschlüsselung rückgängig machen wollen (z. B. zum Verschieben von ASF auf andere Geräte), wenn `ProtectedDataForCurrentUser` verwendet wird, dann wiederholen Sie einfach die Prozedur von Anfang an in der neuen Umgebung.

---

## Hashing

ASF unterstützt derzeit die folgenden Hashmethoden als Parameter für `EHashingMethod`:

| Wert | Name      |
| ---- | --------- |
| 0    | PlainText |
| 1    | SCrypt    |
| 2    | Pbkdf2    |

Die genaue Beschreibung und Unterschiede zwischen diesen sind nachfolgend verfügbar.

Um ein Hash zu generieren, z. B. um es mit `IPCPassword` zu verwenden, müssen Sie den `hash` **[Befehl](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-de-DE)** mit der gewünschten Hash-Methode und Ihrem ursprünglichen Klartext-Passwort ausführen. Danach setzen Sie die gehashte Zeichenfolge, die Ihnen als `IPCPassword` ASF-Konfigurationsvariable vorliegt, und schließlich ändern Sie `PasswordFormat` auf dasjenige, das Ihrer gewählten Hash-Methode entspricht.

---

### `PlainText`

Dies ist die einfachste und zugleich unsicherste Methode das Passwort zu speichern, festgelegt durch `EHashingMethod`, mit dem Wert von `0`. ASF wird eine Hash generieren, die mit der ursprünglichen Eingabe übereinstimmt. Sie ist am einfachsten zu benutzen und zu 100% kompatibel mit allen Setups, daher ist sie eine Standardmethode zum Speichern von Geheimnissen, völlig unsicher für eine sichere Speicherung.

---

### `SCrypt`

Considered secure by today standards, **[SCrypt](https://en.wikipedia.org/wiki/Scrypt)** way of hashing the password is defined as `EHashingMethod` of `1`. ASF verwendet die Implementierung `SCrypt` mit `8` Blöcken, `8192` Iterationen, `32` Hashlänge und Verschlüsselungsschlüssel als **[Salt](https://www.security-insider.de/was-ist-ein-salt-a-1052450/), um das Array von Bytes zu erzeugen. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.</p>

ASF erlaubt es Ihnen, den Schlüssel (**[Salt](https://www.security-insider.de/was-ist-ein-salt-a-1052450/)) mit dem **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** `--cryptkey` anzugeben, den Sie für maximale Sicherheit verwenden sollten. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure. Bei ordnungsgemäßer Verwendung garantiert eine angemessene Sicherheit für eine sichere Speicherung.</p>

---

### `Pbkdf2`

Considered weak by today standards, **[Pbkdf2](https://en.wikipedia.org/wiki/PBKDF2)** way of hashing the password is defined as `EHashingMethod` of `2`. ASF wird die `Pbkdf2` Implementierung mit `10000` Iterationen verwenden `32` Hashlänge und Verschlüsselungsschlüssel als **[Salt](https://www.security-insider.de/was-ist-ein-salt-a-1052450/), mit `SHA-256` als hmac Algorithmus, um das Array von Bytes zu erzeugen. The resulting bytes will then be encoded as **[base64](https://en.wikipedia.org/wiki/Base64)** string.</p>

ASF erlaubt es Ihnen, den Schlüssel (**[Salt](https://www.security-insider.de/was-ist-ein-salt-a-1052450/)) mit dem **[Befehlszeilenargument](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-de-DE)** `--cryptkey` anzugeben, den Sie für maximale Sicherheit verwenden sollten. If you decide to omit it, ASF will use its own key which is **known** and hardcoded into the application, meaning hashing will be less secure.</p>

---

## Empfehlung

Wenn Sie eine Hashing-Methode verwenden möchten, um einige Geheimnisse zu speichern, wie `IPCPassword`, empfehlen wir, `SCrypt` mit benutzerdefinierten **[Salt](https://www.security-insider.de/was-ist-ein-salt-a-1052450/) zu verwenden, da es eine ausgezeichnete Sicherheit gegen brute force Attacken bietet. `Pbkdf2` wird nur aus Kompatibilitätsgründen angeboten, vor allem weil wir bereits eine funktionierende (und erforderliche) Implementierung für andere Anwendungsfälle auf der Steam Plattform haben (z. B. Familienansicht-Pin). Es wird immer noch als sicher angesehen, aber im Vergleich zu anderen Möglichkeiten schwach (z. B. `SCrypt`).</p>