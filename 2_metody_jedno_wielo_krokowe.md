# Metody jedno- a wielo-krokowe; rząd metody

## Jaki problem w ogóle rozwiązujemy takimi metodami?

Metody, o których będzie mowa **służą do rozwiązywania równań różniczkowych**, tj. **mamy jakąś nieznaną funkcję i znamy tylko wzór na jej pochodną** oraz pewne warunki początkowe lub brzegowe.

Upraszczając do funkcji 2D gdzie mamy f(x) = y, na początku:
* jesteśmy w stanie wyliczyć ze wzoru pochodnej, w którą stronę funkcja będzie szła w danym punkcie (na przykładach to będą ilustrowały strzałeczki),
* mamy jakiś "punkt zaczepienia" żeby mieć skąd zacząć.

![obrazek](img/rk_1.png?raw=true "obrazek")

Przy użyciu "strzałeczek", tj. pochodnej, jesteśmy w stanie prześledzić wykres funkcji. Pamiętamy, że w miarę jak podążamy za strzałeczką i rysujemy wykres, strzałeczka może zmieniać kierunek (tj. pochodna może przyjmować różne wartości) i tym samym wykres może "falować".

![obrazek](img/rk_2.png?raw=true "obrazek")

Problem jest tu taki, że **nie jest łatwo śledzić strzałeczkę**.

## Podstawy: Metoda Eulera

W metodzie Eulera wybieramy pewną długość "kroku" i idziemy o taką wartość na osi X w kierunku strzałeczki.

Przykład. Mamy sobie warunki początkowe:
* `f(0) = 1`
* `f'(x) = f(x)`

Czyli w naszym wypadku `f'(0) = f(0)` z drugiego warunku, a `f(0) = 1` z pierwszego. Pochodna dla x=0 wynosi 1, czyli jeśli pójdziemy o krok długości `h` z punktu x=0 wzdłuż osi X, to wzdłuż osi Y musimy pójść o `1*h`.

![obrazek](img/rk_3_h2_1.png?raw=true "obrazek")

### Metoda Eulera, h=2

Załóżmy, że nasz krok wynosi `h=2`. W pierwszym kroku idziemy o `h` jednostek na osi X (tj. 0 -> 2), i o `f'(0) * h = 1 * 2 = 2` na osi Y (tj. 1 -> 3):

![obrazek](img/rk_3_h2_2.png?raw=true "obrazek")

Jesteśmy w x=2, i wyszło nam że f(2) = 3. Ze wzoru w drugim warunku początkowym (który się nie zmienia, podstawiamy tylko inne x) wiemy, że f'(2) = f(2), czyli 3. Zatem teraz idąc `h` jednostek na osi X, idziemy o `3*h` jednostek na osi Y.
Ponieważ założyliśmy sobie `h=2`, to robiąc krok z `x=2` na `x= 2+h = 4`, na osi Y pójdziemy o `f'(2)*h = 3*2 = 6` jednostek w górę:

![obrazek](img/rk_3_h2_3.png?raw=true "obrazek")

Wyszło nam, że f(4) = 9. Można tak sobie liczyć dalej, ale popatrzmy, co się dzieje, jeśli weźmiemy inny krok.

### Metoda Eulera, h=1

Znowu zaczynamy z tego samego miejsca:

![obrazek](img/rk_3_h2_1.png?raw=true "obrazek")

Ale tym razem idziemy w prawo tylko o `h=1` i w górę o `f'(0)*h = 1*1 = 1`:

![obrazek](img/rk_3_h1_1.png?raw=true "obrazek")

Dostaliśmy, że f(1) = 2. Idźmy kolejny krok. Tym razem w górę o `f'(1) * h = 2*1 = 2`:

![obrazek](img/rk_3_h1_2.png?raw=true "obrazek")

Porównajmy jak się to zachowało dla różnych kroków:
* dla `h=2` wyszło nam, że f(2) = 3
* dla `h=1` wyszło nam, że f(2) = 4

Analogicznie idąc dalej, robiąc kolejny krok długości h=1 w prawo i `f'(2)*h = f(2)*h = 4*1 = 4` w górę, mamy że f(3) = 8, a potem f(4) = 16:

![obrazek](img/rk_3_h1_3.png?raw=true "obrazek")

Wartości dla x=0, 1, 2, 3, 4 odpowiednio dające y=1, 2, 4, 8, 16 powinny wyglądać znajomo. Czy nasza nieznana funkcja f(x) to tak naprawdę `2^x`?

### Metoda Eulera, h=0.5

Spróbujmy jeszcze mniejszy krok. Możemy zrobić go taki mały jak chcemy, ale weźmy h=0.5:

![obrazek](img/rk_3_h2_1.png?raw=true "obrazek")

![obrazek](img/rk_3_h05_1.png?raw=true "obrazek")

![obrazek](img/rk_3_h05_2.png?raw=true "obrazek")

![obrazek](img/rk_3_h05_3.png?raw=true "obrazek")

### Metoda Eulera, podsumowanie

Porównajmy sobie wszystkie trzy warianty kroku (h=0.5 zielony, h=1 czerwony, h=2 niebieski):

![obrazek](img/rk_3_h0.png?raw=true "obrazek")

Oczywiście nasza nieznana funkcja f(x) to e^x i do niej zbliżamy się biorąc coraz mniejszy krok, ale widać, że przy użyciu ciężko o dobre obliczenia. Na przykład f(4) = e^4 = 54, czyli bardzo duży błąd biorąc pod uwagę że nam wychodziło 9, 16 czy 25.63 zależnie od kroku.
Niezależnie jak mały krok weźmiemy, rząd wielkości błędu f(x) prawdziwego a obliczonego przez nas zawsze będzie jakoś tam proporcjonalny do x.

Dzieje się tak dlatego że metoda Eulera jest:
 * **jednokrokowa**, bo kompletnie nas nie obchodziły wartości pochodnej z kroków poprzednich. Za każdym razem szliśmy tylko o wartość obliczoną z obecnego punktu, po czym wyrzucaliśmy tę wartość do kosza i już więcej z niej nie korzystaliśmy.
 * **pierwszego rzędu**, bo w każdym kroku liczyliśmy tylko jedną pochodną.

Zwiększenie krokowości i rzędu powoduje, że błąd jest mniejszy (i to nie o ileś tam procent tylko tak jakby o całą klasę złożoności).

### Metody wielokrokowe

Tutaj idea jest taka, żeby wziąć wartości z poprzednich kroków i jakoś je użyć przy liczeniu obecnego:

![obrazek](img/rk_4_0.png?raw=true "obrazek")

Dwa kroki temu mieliśmy wartość pochodnej powiedzmy 2 (tj. poszliśmy po skosie w górę).
Jeden krok temu mieliśmy wartość pochodnej powiedzmy -5 (tj. poszliśmy po skosie w dół).
Teraz mamy pochodną powiedzmy 0 (tj. w metodzie Eulera poszlibyśmy zaraz "płasko" w prawo).

Natomiast w metodzie 3-krokowej, pójdziemy na osi Y nie o `h * 0`, jak by wynikało z pochodnej obliczonej w obecnym kroku, tylko o jakieś `h * (1*a + -2*b + 0*c)`, tj. weźmiemy pod uwagę 3 ostatnie pochodne.
Ogólnie, metoda n-krokowa weźmie pod uwagę n ostatnich pochodnych. Skąd się bierze współczynniki a, b, c... to w sumie nie wiem, ale z tego co kojarzę to muszą spełniać jakieś tam warunki - wtedy błędy się jakoś tam niwelują i jest lepiej niż jednokrokowo, inaczej chyba może wyjść nawet gorzej.

### Metody wyższego rzędu

Tutaj idea jest taka, że robimy jeden krok, ale przymierzamy się do niego kilka razy. Tutaj dosyć chaotyczny obrazek z wikipedii, zielone to moje uwagi (i zielone strzałeczki tylko pokazują do czego jest komentarz a nie wartości pochodnej):

![obrazek](img/rk_rk.png?raw=true "obrazek")

* (1) dalej mamy jakąś ustaloną wartość kroku `h`. Metoda z obrazka **dalej jest jednokrokowa**, tj. gdy już przejdziemy krok to **wszystko co się stało do tej pory wyrzucimy do śmieci i policzymy z kolejnego punktu od zera**.
* (2, 4a) jesteśmy w punkcie `y0` po lewej i liczymy `f'(x0)`, wyszło nam `k1`. **Udajemy, że idziemy pół kroku** i liczymy pochodną w punkcie `{x0 + h / 2, y0 + k1 * h / 2}`, oznaczmy ją jako k2.
* (3) **NIE idziemy dalej**, ani o pełen krok ze startowego ani o drugie pół z półmetka. Tylko udawaliśmy i **dalej jesteśmy w punkcie startowym**.
* (4b) znowu udajemy, że idziemy **pół kroku, z punktu startowego (nie z tego połowicznego z punktu (2))**, tj. z {x0, y0}, ale **przy użyciu pochodnej z tamtego połowicznego punktu,** tj. do {x0 + h / 2, y0 + k2 * h / 2}, i **znowu liczymy pochodną w tamtym miejscu**, niech to będzie k3.
* (5) idziemy z punktu startowego pełen krok przy użyciu pochodnej k3. Jesteśmy **dużo bliżej prawdziwego wykresu (niebieskiego), niż idąc którąkolwiek z poprzednich pochodnych**
* to **dalej jest metoda jednokrokowa**, więc wszystkie te połowiczne pochodne wywalamy do śmieci i kolejny krok będziemy liczyć od zera. 

Tutaj znowu współczynniki (to że idziemy raz pół kroku, drugi raz pół kroku, trzeci raz cały krok) wymyślili jajogłowi i **muszą spełniać jakieś tam ważne warunki**. **Ta konkretna metoda jest 4-go rzędu, bo jeden krok robimy 4 przymiarkami**.
Są też oczywiście metody innych rzędów i inne metody 4 rzędu, natomiast **ta konkretna w przykładzie to Algorytm Runge-Kutty (RK)** i jest najbardziej "klasyczna" i jest takim trochę szablonem dla podobnych metod z innymi współczynnikami. Analogicznie jak w metodach wielokrokowych całość jest po to, że redukujemy "klasę złożoności" błędu. Polecam wiki: https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods

![obrazek](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b1/Comparison_of_the_Runge-Kutta_methods_for_the_differential_equation_%28red_is_the_exact_solution%29.svg/1920px-Comparison_of_the_Runge-Kutta_methods_for_the_differential_equation_%28red_is_the_exact_solution%29.svg.png)

