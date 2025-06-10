### Czy można sortować przez porównania lepiej niż O(n log n)?

Nie można. Lepszą złożoność można osiągnąć poprzez ograniczenia wartości i np. sortowanie przez zliczanie, ale porównaniami nie da się lepiej niż O(n log n).

## Dowód

Mamy sobie tablicę na `n` elementów, powiedzmy `{a, b, c, d, ...}`.

Taka tablica ma `n! = 1 * 2 * ... * (n-1) * n` permutacji, z których jedna jest posortowana poprawnie.

Na przykład dla n = 4 mamy `4! = 1*2*3*4 = 24` permutacje:
```
 1) abcd
 2) abdc
 3) acbd
 4) acdb
 5) adbc
 6) adcb
 7) bacd
 8) badc
 9) bcad
10) bcda
11) bdac
12) bdca
13) cabd
14) cadb
15) cbad
16) cbda
17) cdab
18) cdba
19) dabc
20) dacb
21) dbac
22) dbca
23) dcab
24) dcba
```
Jedna z nich jest posortowana tak, jak chcemy. Na przykład dla tablicy {a=9, b=7, c=2, d=5} jest posortowana rosnąco w permutacji `cdba`.

Po porównaniu (na przykład) `a < b` odrzucamy wszystkie permutacje, w których `a` jest przed `b` lub wszystkie, w których `b` jest przed `a` (zależnie jak wyjdzie porównanie i czy sortujemy rosnąco czy malejąco).
Powiedzmy, że uzywamy tego przykładu powyżej i wyszło że `b < a`. Odrzucamy wszystko, gdzie `b` jest na prawo od `a`:
```diff
-  1) abcd
-  2) abdc
-  3) acbd
-  4) acdb
-  5) adbc
-  6) adcb
+  7) bacd
+  8) badc
+  9) bcad
+ 10) bcda
+ 11) bdac
+ 12) bdca
- 13) cabd
- 14) cadb
+ 15) cbad
+ 16) cbda
- 17) cdab
+ 18) cdba
- 19) dabc
- 20) dacb
+ 21) dbac
+ 22) dbca
- 23) dcab
+ 24) dcba
```
Idąc dalej, na przykład możemy teraz porównać `c < d` i powiedzmy, że wyszło że faktycznie `c < d`. Odrzucamy wszystkie permutacje (spośród pozostałych), gdzie `c` jest na prawo od `d`:
```diff
+  7) bacd
-  8) badc
+  9) bcad
+ 10) bcda
- 11) bdac
- 12) bdca
+ 15) cbad
+ 16) cbda
+ 18) cdba
- 21) dbac
- 22) dbca
- 24) dcba
```

Znowu odrzuciliśmy połowę.
Co do zasady, zawsze możemy dobrać liczby do porównania w ten sposób, żeby odrzucić połowę i tak powinniśmy robić (bo jeśli wybierzemy porównanie, które rozdzieli nam zbiór permutacji nie 50/50 tylko np. 70/30, albo w ogóle takie że w pesymistycznym przypadku odrzucimy tylko 1 permutację, to zawsze może nam porównanie wyjść "źle" i złożoność wtedy nam potencjalnie rośnie).

Dalej możemy sprawdzić `b < c`:
```diff
-  7) bacd
-  9) bcad
- 10) bcda
+ 15) cbad
+ 16) cbda
+ 18) cdba
```
Zostały tylko 3 permutacje, co nie dzieli się ładnie na 50/50. Zależnie, czy będziemy mieli pecha, jedna lub dwa porównania wystarczą. W naszym przypadku `b < d` wystarczyłoby (i skończymy w 4 porównaniach), natomiast możemy też porównać `a < d` i wtedy zostaną dwie permutacje i będziemy musieli wykonać jeszcze jedno porównanie, dając ostatecznie 5.

Ponieważ za każdym razem mamy dwa razy mniej możliwych permutacji, a na początku mieliśmy ich `n!`, to oznacza, że żeby została nam na koniec tylko 1 permutacja (tj. poprawne posortowanie) musimy wykonać `O(log₂(n!))` iteracji (porównań). `log₂(n!)` wynosi właśnie `n log n`.

W przykładzie mieliśmy 24, a wiadomo że `4 == log₂(16) < log₂(24) < log₂(32) == 5`, stąd zależnie od szczęścia musieliśmy użyć 4 lub 5.
