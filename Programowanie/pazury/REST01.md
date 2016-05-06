# Jak uruchomić RESTowy serwer?

Zacznijmy od przykładu. Uruchomimy lokalnie serwer na porcie 4348.

Samo włączenie pakietu `opencpu` startuje serwer na losowym porcie. Dlatego poniżej w drugiej linii zamykamy uruchomiony serwer i uruchamiamy go na wskazanym porcie `4348`.

```
library(opencpu)
opencpu$stop()
opencpu$start(4348)
```

Serwer został uruchomiony w tle. Jak to rozpoznać? Np. za pomocą programu *Network Utility* można sprawdzić listę otwartych portów (ilustracja poniżej).

![Lista otwartych portów](figure/aplikacja1.png)

Domyślnie serwer uruchamia się na interface `localhost`. Możemy więc odpytywać ten serwer przez przeglądarkę internetową.

Testowa strona do budowania zapytań od serwera jest dostępna pod adresem `http://localhost:4348/ocpu/test/`. Pozwala ona w prosty sposób budować zapytania GET i POST.

![Testowa aplikacja www](figure/aplikacja2.png)
