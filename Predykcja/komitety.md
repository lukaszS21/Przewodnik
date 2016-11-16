# Komitety

W tym miejscu opiszemy metody konstrukcji reguł predykcyjnych oparte o komitety innych reguł (które będziemy nazywać mniejszymi regułami, przy czym słowo ,,mniejsze'' nie odnosi się do złożoności obliczeniowej ale do bycia częścią większej reguły).

Rozważymy dwa podejścia, oba związane są z tzw. *Bias-Variance Trade-off*. Błąd predykcji rozkłada się czynniki, z których dwa to obciążenie (bias) i wariancja (variance).

Mając to na uwadze, skonstruujemy metodę predykcji opartą o komitet innych metod, tak by:

* Bagging, łączenie mniejszych reguł w większą regułę decyzyjną ma na celu obniżenie wariancji reguły decyzyjnej. Najczęściej osiąga się to kosztem wprowadzenie obciążenia. Technika sprawdza się dla wyjściowych modeli o małym obciążeniu.

* Boosting, łączenie mniejszych reguł w większą regułę decyzyjną ma na celu obniżenie obciążenia. Najczęściej osiąga się to kosztem zwiększonej wariancji. Technika sprawdza się dla wyjściowych modeli o małej wariancji.

## Bagging

Bagging to skrót od *Bootstrap aggregating*. Ta technika polega na obniżeniu wariancji reguły przez uśrednianie wielu małych reguł decyzyjnych. Sprawdza się dla klasyfikatorów o niskim obciążeniu, które mają tendencje do przeuczania się. Takich jak drzewa decyzyjne, sieci neuronowe czy modele liniowe o dużej liczbie cech.

Technikę opisać można następująco.

1. Ze zbioru $(X, y)$ o wielkości $n$ wylosuj ze zwracaniem bootstrapową próbę $(X, y)^{b,*}$. 
2. Na próbie bootstrapowej zbuduj regułę decyzyjną $f(X)^{b, *}$
3. Powtórz kroki 1-2 wiele razy, np. B razy (np. B=1000),
4. Uśrednij / uzgodnij wyniki wszystkich B reguł decyzyjnych.

### Lasy losowe

Techniką wyrastającą z baggingu są lasy losowe, obecnie znacznie częściej stosowane niż standardowy bagging.
Nazwa lasy losowe jest znakiem towarowym zastrzeżonym przez Leo Breiman i Adele Cutler.

W ogólności bagging może być zastosowany do dowolnej reguły decyzyjnej, ale okazuje się, że dobre wyniki szybko można uzyskać bazując na drzewach decyzyjnych.

Algorytm lasów losowych jest następujący

1. Ze zbioru $(X, y)$ o wielkości $n$ wylosuj ze zwracaniem bootstrapową próbę $(X, y)^{b,*}$. 
2. Na próbie bootstrapowej zbuduj drzewo decyzyjne $D^{*, b}$. Proces budowania drzewa jest opisany w poprzednim rozdziale, jedyna modyfikacja polega na tym, że w każdym węźle losowane jest $r$ zmiennych i tylko te zmienne są rozpatrywane jako kandydaci do dzielenia. Autorzy sugerują wybieranie $r=\sqrt{p}$ dla problemów klasyfikacji i $r=p/3$ dla problemów regresji.
3. Powtórz kroki 1-2 B razy, np. B=1000 razy.
4. Uzgodnij (uśrednij) wyniki predykcji z użyciem $D^{*, b}$ drzew.

Lasy losowe, poza samą regułą decyzyjną, pozwalają na wyznaczenie kilku użytecznych współczynników.

1. Out-of-Bag error-rate (OOB). Ponieważ każda z prób bootstrapowych zawiera średnio $1-exp(-1) \approx 63.2\%$ obserwacji, pozostałe $36.8\%$ obserwacji można wykorzystać dla każdego drzewa do walidacji modelu. 

2. Variable importance - ważności zmiennych. Ważność zmiennych w lasach losowych można wyznaczać na kilka sposobów, najpopularniejsze są dwa:
  * Gini (MeanDecreaseGini), jako miarę ważności zmiennej $X_i$ wyznacza się sumę, po wszystkich węzłach wykorzystujących zmienną $X_i$, zmiany współczynnika Gini impurity. Gdzie dla węzła $r$ mamy $i(r) = 1-p_1^2 -p_2^2$. Zmiana tej miary to $I(r) = i(r) - p_L i(r_L) - p_R i(r_R)$.
  * Permutacyjny (MeanDecreaseAccuracy), jako miarę ważności zmiennej $X_i$ wyznacza się różnicę średniego błędu OOB pomiędzy lasem z wszystkimi zmiennymi a lasem zbudowanym na zbiorze danych gdzie $X_i$ zostało permutowane.

3. Proximities - podobieństwa. Mając wyuczony las losowy, można go wykorzystać by ocenić miarę podobieństwa obserwacji. Wyznacza się ją jako częstość węzłów w lesie, w których dane dwie obserwacje występują razem.

4. Imputacja wartości brakujących. Mając miarę podobieństwa pomiędzy obserwacjami, obserwacje brakujące są uzupełniane przez ważoną średnią z sąsiednich wierszy (sąsiednich w sensie proximities). 

5. Identyfikacja obserwacji odstających. Za obserwacje odstającą uznaje się obserwację o niskich wartościach podobieństwa (proximities) do innych obserwacji z danego klastra (klasy). Czyli 
$$
out(i) = \frac{n}{\sum_{d(k) = j}prox^2(m, k)}
$$
gdzie $j$ to klasa obserwacji $m$ a $n$ to liczba wszystkich obserwacji.


## Boosting




## Porównanie Baggingu i Boostingu

Bagging się łatwo zrównoleglą (każdą próbę bootstrapową można analizować niezależnie).

Bagging nie pomaga przy stabilnych modelach.

Boosting jest wrażliwy na obserwacje odstające.

Boosting buduje kolejne modele w sposób sekwencyjny.
