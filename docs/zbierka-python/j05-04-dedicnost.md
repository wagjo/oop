# Dedičnosť


## Úloha 5.4.1: Zamestnanci

1. Vytvorte triedu podľa nasledovného kódu:

    ```python
    class Zamestnanec:
        def __init__(self, meno, zakladna_mzda):
            ...
        def vypocitaj_mzdu(self) -> float:
            return self.zakladna_mzda
    ```

1. Vytvorte tieto triedy:

    - `Programator` - atribut `pocetProjektov`, + 20 % bonus za každý dokončený projekt
    - `Manazer` - atribut `pocetPodriadenych`, + pevný tímový bonus 1000 €, + 5% z platu každého podriadeného
    - `Stazista` - dostáva len 60 % základnej mzdy

1. Naimplementujte vypocitaj_mzdu pre jednotlivé typy zamestnancov

1. Vytvorte zoznam zamestnancov rôznych typov a vypíšte celkovú mzdu firmy.


## Úloha 5.4.2: Zvieratá

1. Vytvorte triedu `Zviera` a potomkov `Pes`, `Had`, `Orol`, `Delfin`, `Netopier`.

1. Každé zviera nech má:

    - `zvuk()`
    - `pohyb()` - pes „beží“, had „plazí sa“, orol „letí“, delfín „pláva“

1. Pre implementáciu pohybu použite triedy reprezentujúce daný typ pohybu:

    ```python
    class Lietajuce:
        def pohyb(self): return "letí"

    class Plavajuce:
        def pohyb(self): return "pláva"

    ...atď
    ```

1. Teda dedenie bude vyzerať napr. `Orol(Lietajuce, Zviera)` a `Delfin(Plavajuce, Zviera)`.

1. Napíšte príklad použitia.

## Úloha 5.4.3: Knihy a publikácie

1. Vytvorte triedu `Publikacia` so základnými atribútmi: nazov, rok_vydania.

1. Potom vytvorte triedu `Kniha`, ktorá zdedí Publikaciu a pridá autora a počet strán.

1. Použite super() v konštruktore.

1. Prepíšte metódu `__str__()`, aby zobrazovala všetky údaje.

1. Vytvorte ďalšiu triedu `Casopis`, ktorá pridá číslo vydania.


## Úloha 5.4.4: Jedlo – Nealko – Alkohol

1. Vytvorte triedy: 

    - Trieda `Jedlo`: názov, cena.

    - Trieda `Napoj`: dedí z Jedlo.

    - `NealkoNapoj`

    - `AlkoNapoj` (má percentá alkoholu).

1. Implementujte `__str__()` tak, aby vypísal všetky údaje.

1. Vytvorte funkciu, ktorá vypíše len alkoholické nápoje zo zoznamu.

1. Napíšte príklad použitia.


## Úloha 5.4.5: E-shop – Produkty

1. Vytvor trieda `Produkt`: nech má názov a cenu.

1. Vytvor podtriedy:

    - `Elektronika` – pridaj záruku v mesiacoch
    - `Oblecenie` – veľkosť a farba

1. Implementuj metódu `popis()`, ktorú v podtriedach prepíšeš.

1. Pridaj funkciu, ktorá zo zoznamu produktov vyfiltruje všetku elektroniku drahšiu než X.

