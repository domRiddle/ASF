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
    

Alternatywnie również jesteśmy w stanie użyć klawiszy tylko format (nadal z nowej linii między każdego wpisu). ASF w tym przypadku użyje odpowiedzi Steam (jeśli to możliwe), aby wypełnić właściwą nazwę. W przypadku wszelkiego rodzaju tagowania kluczy zalecamy, abyś sam wymieniał klucze, ponieważ pakiety wymieniane na Steam nie muszą być zgodne z logiką gier, które aktywują, więc w zależności od tego, co zrobił programista, możesz zobaczyć poprawną grę nazwy, niestandardowe nazwy pakietów (np. Humble Indie Bundle 18) lub wręcz błędne i potencjalnie nawet złośliwe (np. Half-Life 4).

    ABCDE-EFGHJ-IJKLM
    12345-67890-ZXCVB
    POIUY-KJHGD-QWERT
    ZXCVB-ASDFG-QWERT
    

Bez względu na to, w jakim formacie zdecydowałeś się trzymać, ASF zaimportuje twoje klucze ` </ 0>, zarówno podczas uruchamiania bota, jak i później w trakcie jego wykonywania. Po pomyślnym przeanalizowaniu pliku i ewentualnym pominięciu nieprawidłowych wpisów, wszystkie poprawnie wykryte gry zostaną dodane do kolejki tła, a sam plik <code> BotName.keys </ 0> zostanie usunięty z <code> config </ 0 > katalog.</p>

<h3>IPC</h3>

<p>Oprócz użycia wspomnianego wcześniej pliku kluczy, ASF udostępnia również <GamesToRedeemInBackground </ 0> <strong><a href="https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-api"> punkt końcowy API ASF </ 1>, który może być wykonywany przez dowolne narzędzie IPC, w tym nasz ASF-ui. Używanie IPC może być znacznie potężniejsze, ponieważ możesz sam dokonać właściwego analizowania, na przykład użyć niestandardowego ogranicznika zamiast być zmuszonym do wstawiania znaków tabulacji, lub nawet wprowadzać całkowicie własną niestandardową strukturę klawiszy.</p>

<hr />

<h2>Kolejka</h2>

<p>Pomyślnie zaimportowane gry są dodawane do kolejki. ASF automatycznie przechodzi przez kolejkę tła, dopóki bot jest podłączony do sieci Steam, a kolejka nie jest pusta. Klucz, który próbował zostać wykorzystany i nie spowodował ograniczenia <code> RateLimited </ 0>, został usunięty z kolejki, a jego status został poprawnie zapisany w pliku w katalogu <code> config </ 0> - <code> BotName.keys.used </ 0> jeśli klucz został użyty w procesie (np. <code> BrakDetaila </ 0>, <code> Kod BadActivation </ 0>, <code> DuplicateActivationCode </ 0>) lub <0 > BotName.keys.unused </ 0> inaczej. ASF celowo używa podanej nazwy gry, ponieważ nie ma gwarancji, że kluczowa nazwa zostanie zwrócona przez sieć Steam - w ten sposób możesz oznaczyć klucze używając nawet niestandardowych nazw, jeśli są potrzebne / poszukiwane.</p>

<p>Jeśli podczas procesu nasze konto osiągnie status <code> RateLimited </ 0>, kolejka zostanie tymczasowo zawieszona na pełną godzinę, aby czekać na zakończenie czasu odnowienia. Potem proces jest kontynuowany, gdzie zostawił, aż cała kolejka jest pusta.</p>

<hr />

<h2>Przykład</h2>

<p>Załóżmy, że masz listę 100 kluczy. Po pierwsze należy utworzyć nowy plik <code>BotName.keys.new` w katalogu `config` ASF. Możemy dołączane `nowe` rozszerzenie w celu niech ASF, wiem, że to nie powinien odebrać ten plik natychmiast w chwili, gdy jest tworzony (jak to jest nowy pusty plik, nie jest gotowy do importu, jeszcze).

Teraz można otworzyć nasz nowy plik i Kopiuj Wklej listę naszych 100 kluczy, ustalające format, w razie potrzeby. Po poprawki nasz plik `BotName.keys.new` będzie miał dokładnie 100 (lub 101, z ostatni znak nowego wiersza) wierszy, każdy wiersz o strukturze `GameName\tcd-key\n`, gdzie `\t` jest znak tabulacji i `\n` jest znak nowego wiersza.

Teraz jesteś gotowy, aby zmienić nazwę tego pliku od `BotName.keys.new` do `BotName.keys` w celu niech ASF, wiem, że jest on gotowy do odbioru. W chwili, gdy to zrobisz, ASF automatycznie importować plik (bez konieczności ponownego uruchomienia komputera) i usunąć go później, potwierdzając, że wszystkie nasze gry były analizowane i dodane do kolejki.

Zamiast przy użyciu pliku `BotName.keys`, można również użyć IPC API punktu końcowego, lub nawet połączenie obu Jeśli chcesz.

Po pewnym czasie może uzyskać generowane pliki `BotName.keys.used` i `BotName.keys.unused`. Te pliki zawierają wyniki naszego procesu wymiany. Na przykład, możesz zmienić nazwę pliku ` BotName.keys.unused </ 0> na plik <code> BotName2.keys </ 0>, a następnie przesłać nasze nieużywane klucze do innego bota, ponieważ poprzedni bot nie użył te klucze sam. Lub możesz po prostu skopiować i wkleić nieużywane klucze do jakiegoś innego pliku i zachować go na później, twój telefon. Należy pamiętać, że gdy ASF przechodzi przez kolejkę, nowe wpisy zostaną dodane do naszych wyjściowych <code> używanych </ 0> i <code> nieużywanych </ 0> plików, dlatego zaleca się, aby poczekać na pełne opróżnienie kolejki zanim skorzystasz z nich. Jeśli bezwzględnie musisz uzyskać dostęp do tych plików przed całkowitym opróżnieniem kolejki, powinieneś najpierw <strong> przenieść </ 0> plik wyjściowy, do którego chcesz uzyskać dostęp do innego katalogu, <strong> następnie </ 0> go przeanalizować. Dzieje się tak dlatego, że ASF może dodawać nowe wyniki, gdy robisz coś, co może prowadzić do utraty niektórych kluczy, jeśli czytasz plik zawierający np. 3 klucze wewnątrz, a następnie usuń je, całkowicie pomijając fakt, że ASF dodał 4 inne klucze do usuniętego pliku w międzyczasie. Jeśli chcesz uzyskać dostęp do tych plików, upewnij się, że zostały przeniesione z katalogu ASF <code> config </ 0> przed ich odczytaniem, na przykład przez zmianę nazwy.</p>

<p>Możliwe jest również dodanie dodatkowych gier do zaimportowania, podczas gdy niektóre gry są już w naszej kolejce, powtarzając wszystkie powyższe kroki. ASF poprawnie doda nasze dodatkowe wpisy do już trwającej kolejki i ostatecznie sobie z tym poradzi.</p>

<hr />

<h2>Uwagi</h2>

<p>Background keys redeemer uses <code>OrderedDictionary` under the hood, which means that your cd-keys will have preserved order as they were specified in the file (or IPC API call). This means that you can (and should) provide a list where given cd-key can only have direct dependencies on cd-keys listed above, but not below. For example, this means that if you have DLC `D` that requires game `G` to be activated firstly, then cd-key for game `G` should **always** be included before cd-key for DLC `D`. Likewise, if DLC `D` would have dependencies on `A`, `B` and `C`, then all 3 should be included before (in any order, unless they have dependencies on their own).

Not following the scheme above will result in your DLC not being activated with `DoesNotOwnRequiredApp`, even if your account would be eligible for activating it after going through its entire queue. If you want to avoid that then you must make sure that your DLC is always included after the base game in your queue.