# Aktywacja gier w tle

Aktywacja kluczy w tle jest specjalną, wbudowaną funkcją ASF która pozwala na importowanie dużej ilości kluczy (wraz z ich nazwami) które zostaną aktywowane na danym koncie. To jest szczególnie przydatne gdy masz dużą kluczy do aktywowania na raz i wiesz że osiągniesz **[stan](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#what-is-the-meaning-of-status-when-redeeming-a-key)** `RateLimited` zanim aktywują się wszystkie.

Funkcja aktywacji kluczy w tle została napisana żeby się wykonała w jednej pętli, co powoduje, że nie korzysta z `RedeemingPreferences`. Można używać razem (lub zamians) **[polecenia](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** `redeem`.

* * *

## Importuj

Proces importowania może odbyć się na dwa sposoby - poprzez plik, lub IPC.

### Plik

ASF rozpozna w katalogu `config` plik o nazwie `BotName.keys`, gdzie `BotName` jest nazwą bot. Ten plik ma oczekuje i stałej struktury nazwa gry z cd-key, oddzielone od siebie znakiem tabulacji i kończy znakiem nowej linii, aby wskazać następnego wpisu. Jeśli wiele kart są używane, a następnie pierwszy wpis jest uważana za nazwę gry, ostatni wpis jest uważany za cd-key i wszystko pomiędzy nimi jest ignorowana. Na przykład:

    POCZTOWY 2 ABCDE-EFGHJ-IJKLM
    Domino Craft VR 12345-67890-ZXCVB
    A tydzień cyrk grozy POIUY-KJHGD-QWERT
    Terraria ThisIsIgnored ThisIsIgnoredToo    ZXCVB-ASDFG-QWERT
    

Alternatywnie również jesteśmy w stanie użyć klawiszy tylko format (nadal z nowej linii między każdego wpisu). ASF in this case will use Steam's response (if possible) to fill the right name. For any kind of keys tagging, we recommend that you name your keys yourself, as packages being redeemed on Steam do not have to follow logic of games that they're activating, so depending on what the developer has put, you might see correct game names, custom package names (e.g. Humble Indie Bundle 18) or outright wrong and potentially even malicious ones (e.g. Half-Life 4).

    ABCDE-EFGHJ-IJKLM
    12345-67890-ZXCVB
    POIUY-KJHGD-QWERT
    ZXCVB-ASDFG-QWERT
    

Regardless which format you've decided to stick with, ASF will import your `keys` file, either on bot startup, or later during execution. After successful parse of your file and eventual omit of invalid entries, all properly detected games will be added to the background queue, and the `BotName.keys` file itself will be removed from `config` directory.

### IPC

In addition to using keys file mentioned above, ASF also exposes `GamesToRedeemInBackground` **[ASF API endpoint](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api)** which can be executed by any IPC tool, including our ASF-ui. Using IPC might be more powerful, as you can do appropriate parsing yourself, such as using a custom delimiter instead of being forced to a tab character, or even introducing your entirely own customized keys structure.

* * *

## Kolejka

Pomyślnie zaimportowane gry są dodawane do kolejki. ASF automatically goes through its background queue as long as bot is connected to Steam network, and the queue is not empty. A key that was attempted to be redeemed and did not result in `RateLimited` is removed from the queue, with its status properly written to a file in `config` directory - either `BotName.keys.used` if the key was used in the process (e.g. `NoDetail`, `BadActivationCode`, `DuplicateActivationCode`), or `BotName.keys.unused` otherwise. ASF intentionally uses your provided game's name since key is not guaranteed to have a meaningful name returned by Steam network - this way you can tag your keys using even custom names if needed/wanted.

If during the process our account hits `RateLimited` status, the queue is temporarily suspended for a full hour in order to wait for cooldown to disappear. Potem proces jest kontynuowany, gdzie zostawił, aż cała kolejka jest pusta.

* * *

## Przykład

Załóżmy, że masz listę 100 kluczy. Po pierwsze należy utworzyć nowy plik `BotName.keys.new` w katalogu `config` ASF. Możemy dołączane `nowe` rozszerzenie w celu niech ASF, wiem, że to nie powinien odebrać ten plik natychmiast w chwili, gdy jest tworzony (jak to jest nowy pusty plik, nie jest gotowy do importu, jeszcze).

Teraz można otworzyć nasz nowy plik i Kopiuj Wklej listę naszych 100 kluczy, ustalające format, w razie potrzeby. Po poprawki nasz plik `BotName.keys.new` będzie miał dokładnie 100 (lub 101, z ostatni znak nowego wiersza) wierszy, każdy wiersz o strukturze `GameName\tcd-key\n`, gdzie `\t` jest znak tabulacji i `\n` jest znak nowego wiersza.

Teraz jesteś gotowy, aby zmienić nazwę tego pliku od `BotName.keys.new` do `BotName.keys` w celu niech ASF, wiem, że jest on gotowy do odbioru. W chwili, gdy to zrobisz, ASF automatycznie importować plik (bez konieczności ponownego uruchomienia komputera) i usunąć go później, potwierdzając, że wszystkie nasze gry były analizowane i dodane do kolejki.

Zamiast przy użyciu pliku `BotName.keys`, można również użyć IPC API punktu końcowego, lub nawet połączenie obu Jeśli chcesz.

Po pewnym czasie może uzyskać generowane pliki `BotName.keys.used` i `BotName.keys.unused`. Those files contain results of our redeeming process. For example, you could rename `BotName.keys.unused` into `BotName2.keys` file and therefore submit our unused keys for some other bot, since previous bot didn't make use of those keys himself. Or you could simply copy-paste unused keys to some other file and keep it for later, your call. Keep in mind that as ASF goes through the queue, new entries will be added to our output `used` and `unused` files, therefore it's recommended to wait for the queue to be fully emptied before making use of them. If you absolutely must access those files before queue is fully emptied, you should firstly **move** output file you want to access to some other directory, **then** parse it. This is because ASF can append some new results while you're doing your thing, and that could potentially lead to loss of some keys if you read a file having e.g. 3 keys inside, then delete it, totally missing the fact that ASF added 4 other keys to your removed file in the meantime. If you want to access those files, ensure to move them away from ASF `config` directory before reading them, for example by rename.

It's also possible to add extra games to import while having some games already in our queue, by repeating all above steps. ASF will properly add our extra entries to already-ongoing queue and deal with it eventually.

* * *

## Uwagi

Background keys redeemer uses `OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.