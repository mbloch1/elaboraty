# Metoda elementów skończonych

Idea MES (ang. FEM, finite element method) jest taka, żeby zamodelować problem ciągły jako skończoną liczbę podproblemów dyskretnych. Czyli dzielimy model na kawałeczki, rozwiązujemy każdy kawałeczek osobno, i składamy wszystko z powrotem.

### Konkretny przykład

Przykład z wikipedii: chcemy obliczyć przepływ prądu przez jakąś magnetyczną cewkę. Mamy kawałek żelastwa i obok miedziany drut:

![obrazek 1](img/fem_0.png?raw=true "obrazek")

Rozbijamy model to na siatkę punktów (albo trójkącików, jest to w jakimś tam sensie równoważne). Tutaj ważna kwestia: w interesujących miejscach (w tym przykładzie: magnes i jego otoczenie) możemy zrobić gęstą siatkę (tak gęstą jak nam się podoba), a gdzieś na bokach bardziej luźną. Dzięki temu możemy dostać bardzo precyzyjne obliczenia tam, gdzie ma to znaczenie, a oszczędzić pracy tam, gdzie nas to aż tak bardzo nie obchodzi. Inny przykład (patrz niżej): modelując zderzenie samochodu możemy mieć gęstszą siatkę z przodu i w kabinie z ludźmi, a rzadszą z tyłu albo w bagażniku.

![obrazek 2](img/fem_1.png?raw=true "obrazek")

Teraz dla każdego punktu liczymy interesujące nas właściwości (rozwiązujemy równanie różniczkowe), w tym wypadku przepływ prądu:
 * każdy punkt liczy tylko wartości w stosunku do punktów obok. Dzięki temu obliczenia mogą być bardzo proste (czasem nawet takie że można użyć takiego szkolnego wzoru z fizyki) i to bardzo przyspiesza obliczenia.
 * każdy punkt "wie", jakie ma właściwości (w tym przykładzie: czy jest częścią magnesu lub druta). Dzięki temu obliczenia dalej są poprawne (tj. używamy odpowiednich wartości przewodzenia, namagnesowania, czy czego tam potrzebuje nasz problem).
 * dla większości modeli powtarzamy obliczenia wielokrotnie, bo mamy równania różniczkowe zależne od czasu, tj. model pod spodem też się zmienia zależnie od uwarunkowań. Np. w tym naszym przykładzie pod wpływem prądu i magnesu może się na drucie wyindukować jakiś drugi prąd. Albo jeśli modelujemy obciążenia na moście to coś może się coś złamać i osłabić kolejne elementy i wtedy wali się kaskadowo cały most, a nie tylko ten 1 element.

![obrazek 3](img/fem_2.png?raw=true "obrazek")

Mając już policzone możemy zrobic jakąś ładną wizualizację dla użytkownika.

![obrazek 4](img/fem_3.png?raw=true "obrazek")

### Zastosowania

* przepływy (np. prądu jak powyżej, [powietrza/temperatury](https://www.youtube.com/watch?v=abV9UwqL_nA))
* analiza strukturalna w inżynierii materiałowej, tj. rozkłady sił (zderzenia samochodów, wytrzymałość budynków)
* ogólnie do modelowania procesów fizycznych trwających w czasie

### Obrazki

Większość tych wizualizacji w CADowych narzędziach może pod spodem korzystać z tej metody. Tutaj jakieś napięcie na moście

![obrazek 5](img/fem_4.png?raw=true "obrazek")

Modelowanie zderzenia samochodu. Można dostrzec że siatka z przodu jest gęstsza niż z tyłu (z tyłu widać indywidualne prostokąciki):

![obrazek 5](img/fem_6.png?raw=true "obrazek")

Jakiś przepływ płynów. Elementy są na tyle malutkie że ich nie widać:

![obrazek 6](img/fem_5.png?raw=true "obrazek")

Tutaj też jakaś aerodynamika (z lewej tunel aerodynamiczny, z prawej model komputerowy)

![obrazek 7](img/fem_7.jpg?raw=true "obrazek")
