## Zbiór (set) na różnych strukturach danych

### Lista

Mamy wskaźnik na jakiś "pierwszy" element, każdy element ma wskaźnik na jakiś inny/kolejny. Można mieć tez listę dwukierunkową, wtedy mamy większe zużycie pamięci ale trochę łatwiej się po tym iteruje.

![lista](img/set_lista.png?raw=true "obrazek")

Zalety:
 * wstawianie O(1), bo nie musimy sortować (sortowanie nic by nam nie dało). Wystarczy podpiąć nowy element pod wskaźnik początku listy, a jako następnik nowego elementu dać obecny pierwszy element.
 * nigdy nie musimy realokować elementów. Każdy element alokujemy raz i potem zawsze mamy do niego dostęp przez wskaźnik.
 * nie wymaga spójnego obszaru pamięci (możemy alokować elementy pojedynczo "po kątach").
 * wskaźniki na konkretne elementy które sobie gdzieś zapiszemy (iteratory) tracą ważność tylko jeśli kompletnie usuniemy element. Przepinanie wskaźników w innych elementach niczego nie psuje
 * zadanie o to nie pytało, ale można insertować i deletować po kilka sekwencyjnych elementów jednocześnie praktycznie za darmo i przenosić między listami, wystarczy poprzepinać wskaźniki.
 * trywialna w implementacji

Wady:
 * dostęp (sprawdzenie, usuwanie) złożoność O(n), bo musimy jechać po elementach pojedynczo, aż znajdziemy ten który nas interesuje.
 * nie wymusza spójnego obszaru pamięci (gorszy czas dostępu na fizycznej maszynie ze względu na cache)

Ogólnie list liniowych raczej nie używa się do implementacji zbioru jako takiego, bardziej jakieś kolejki zdarzeń.

### Tablica

Mamy zaalokowane z góry miejsce na N elementów i wstawiamy posortowane. Wtedy przesuwamy kolejne elementy, analogicznie przy usuwaniu zacieśniamy:

![tablica insert](img/set_tablica_insert.png?raw=true "obrazek")
![tablica delete](img/set_tablica_delete.png?raw=true "obrazek")

Jeśli skończy się miejsce przy insertowaniu, musimy zaalokować cały bloczek pamięci na M elementów (M > N) i przenieść ze starego kawałka elementy, a potem dopiero wstawiamy.

![tablica realloc](img/set_tablica_realloc.png?raw=true "obrazek")

Zalety:
 * sprawdzenie czy element jest w zbiorze: złożoność O(log n), bo jest posortowane i szukamy binarnie
 * elementy są spójne więc operacje są bardzo szybkie dzięki sprzętowemu cache'owi pamięci
 * wersja z realokacją to sensu stricte wektor a nie tablica (wektor = realnie tablica na stercie), a prawdziwą tablicę można trzymać na stosie 

Wady:
 * jeśli faktycznie mamy tablicę zamiast wektora to jest z góry ograniczenie na rozmiar (wektor realnie nie ma ograniczeń dzięki realokacji).
 * wymaga spójnego kawałka pamięci (może być ciężko taki znaleźć i zaalokować, zarówno na stosie jak i na stercie)
 * insert i delete potencjalnie psują zapisane wskaźniki (iteratory) do innych elementów niż ten który wstawiamy/usuwamy
 * insert i delete złożoność O(n)
 * powinno być zdefiniowane jakieś sortowanie (dla skomplikowanych obiektów to może nie być oczywiste). Sensu stricte nie musi ale są wtedy inne właściwości bardziej podobne do listy

### Hashmapa

Hashmapa to realnie tablica list ("kubełków") i łączy właściwości obu. Używa funkcji hashującej żeby przypisać element do danej listy (kubełka):

![hashmapa](img/set_hashmapa.png?raw=true "obrazek")

Przykład powyżej: mamy hashmapę na 6 kubełków i funkcję hashującą H. Dla jakiegoś elementu A funkcja hashująca H(A) przypisuje mu kubełek 0, więc trafia do kubełka 0 (tj. na listę pod indeksem 0 w tablicy).

Na obrazku dałem że modulo 6 żeby wartość hasha była niezależna od liczby kubełków, bo liczbę kubełków można (i trzeba) zmieniać. Powinna rosnąć w miarę dodawania elementów, żeby realnie każdy kubełek miał równą i małą liczbę elementów (np. jeśli mamy 100 elementów to niemożliwy ideał byłby taki, żeby mieć 100 kubełków po 1 elemencie. W praktyce robi się np. 150-200 kubełków i część zostaje pusta, niektóre mogą mieć po 2-3 elementy).

Funkcja hashująca też musi mieć pewne właściwości: rozdzielać w miarę równo po kubełkach i być obliczalna O(1) dla każdego elementu.

Zalety:
* wszystkie trzy operacje: member, insert, i delete są O(1) pod warunkiem, że kubełki są wypełniane równomiernie i prawie puste.
* jeśli są duże elementy to mamy zalety alokacji pojedynczo na stercie; tablicę list alokujemy z zaletami spójnego kawałka pamięci...

Wady:
* ...i analogicznie odpowiednio z ich wadami
* trzeba mieć funkcję hashującą i musi ona spełniać konkretne właściwości, bo złożoność wzrośnie
* pesymistycznie nawet przy "dobrym" hashu możemy dostać złożoność O(n), gdy mamy pecha w danych (np. wiele elementów dostanie wyliczony ten sam hash)
* z tych trzech struktur, hashmapa jest najtrudniejsza w implementacji.
