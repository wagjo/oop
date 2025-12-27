# Cvičenie 11: Polročný projekt

Na tomto cvičení sa rozdelíte do skupín po 3 a vyberiete si jedno zo zadaní.


## Pokyny

Na vypracovanie zadania máte 2 týždne. Po odovzdaní svoj projekt odprezentujete na hodine. Za projekt budete ohodnotení známkou.

Odovzdanie projektu po termíne znamená zníženie známky z projektu. Nesplnenie niektorých častí zadania znamená zníženie známky. Minimálne požiadavky:

- Java Projekt na GitHube
- Dokumentácia v Markdowne, README.md súbor
- Program spustiteľný v IntelliJ IDEA
- Program má textové menu, pomocou ktorého vieme ovládať jeho funkcionality a generovať výpisy
- Každý aspoň 3 netriviálne commity v troch rôznych dňoch (Jeden študent má mať aspoň 3 dni kedy urobil netriviálny commit na GitHub)
- Výsledná prezentácia vo forme PDF alebo PPT
- UML Class Diagram (diagram tried) v Mermaid, plus vložený do prezentácie
- Flow chart vybranej časti logiky programu (vývojový diagram) v Mermaid, plus vložený do prezentácie


### Rozdelenenie zodpovednosti

V rámci trojčlennej skupiny si medzi seba rozdeľte nasledovné zodpovednosti (Nie každé zadanie vyžaduje všetky zodpovednosti):

- Modelovanie hlavnej logiky programu do tried a metód
- Vstup a výstup do súboru a sťahovanie z internetu, formát a syntax súborov
- Užívateľské rozhranie - Práca s konzolou, menu programu
- UML Diagram tried, Vývojový diagram vybranej časti logiky programu
- Dokumentácia v markdowne, README.md, GitHub
- Vzorové vstupné alebo testovacie dáta

### Úloha do konca tohto cvičenia

**Do konca tohto cvičenia je potrebné rozdeliť sa do skupín, vybrať si zadanie, určiť predbežné rozdelenie zodpovednosti a vytvoriť repozitár na GitHube**

## Zadanie 1 - Textová adventúra

Toto zadanie je pre študentov, s ktorými mám aj OPGP.

- Textová adventúra, zobrazí popis miesnosti, predmety v miestnosti a predmety v inventári
- Akcie: Pohyb medzi miestnosťami (go vychod), použitie predmetu z inventára (use maceta), vzatie predmetu z miestnosti do inventára (take kluc)
- Hra musí vedieť pracovať s mapou v .json súbore [hra12.json](http://oop.wagjo.com/assets/hra12.json)
- Vytvoriť vlastnú mapu s aspoň 5 miestnosťami, 2 koncami a 3 predmetmi, ktoré budú mať využitie


## Zadanie 2 - Knižnica

Program na správu knižnice, kde pomocou textového užívateľského rozhrania je možné pracovať s inventárom kníh.

Funkcionality:

- Pomocou tried namodelovať knižnicu, inventár kníh a knihu
- Možnosť pridať knihu, vyradiť knihu
- Možnosť vypožičať knihu, vrátiť knihu
- Analytika: Výpis všetkých kníh
- Výpis vypožičaných kníh, alebo kníh po výpožičnej dobe

- Možnosť zoradiť knihy podľa kategórie (beletria, dobrodružné, detské, ...)

## Zadanie 3 - Lekáreň

Program na správu lekárne, kde pomocou textového užívateľského rozhrania je možné pracovať so skladom s liekmi.

Funkcionality:

- Pomocou tried namodelovať lekáreň, sklad liekov, liek
- Každý liek má názov, účinnú látku, dátum expirácie a cenu
- Ovládanie: Naskladniť liek, predať liek pacientovi
- Možnosť vyradiť lieky po expiracii
- Analytika: Vypísat všetky lieky, vypísat lieky po expirácii
- Možnosť zoradiť výpis podľa kategórii. (účinná latka)


## Zadanie 4 - PC Servis

Program na správu PC Servisu, kde pomocou textového užívateľského rozhrania je možné pracovať s PC komponentami.

Funkcionality:

- Pomocou tried namodelovať servis, sklad komponentov a PC zostavu.
- PC komponenty - kategórie CPU, RAM, SSD, GPU, MB, ZDROJ, CASE
- Každý komponent má názov, cenu, kategóriu.
- Možnosť manuálne vytvoriť PC zostavu
- Možnosť automaticky vytvoriť zostavu do určitej sumy
- Možnosž predať zostavu - odstránenie komponentov so skladu
- Analytika: Výpis zostavy. 
- Analytika: Výpis inventára podľa kategórií


## Zadanie 5 - Analýza ceny zlata

Program na analýzu ceny zlata. Práca s .csv súborom.

Funkcionality:

- Načitať dáta z https://datahub.io/core/gold-prices alebo z iného aktuálnejšieho zdroja
- Pomocou tried namodelovať dataset a položky
- Výpis mesačných cien v danom roku.
- Výpis priemeru a mediánu v danom roku.
- Výpis maxima (hodnota a dátum). Výpis maxima v danom roku.
- Vypis majetku ku dnešnému dňu na základe toho, kedy ste zlato kúpili (zadáte napríklad, že ste kúpili 10oz zlata v 2010-10 a vypočíta aktuálnu hodnotu majetku)


## Zadanie 6 - Nutričná kalkulačka

Program na nutričnú kalkulačku, pomocou ktorej si viete zostaviť jedálniček a ohodnotiť jeho vhodnosť

Funkcionality:

- Pomocou tried namodelovať nutričnú kalkukačku, katalóg jedál, jedlo, jedálniček.
- Jedlo má názov, kcal, bielkoviny, tuky, uhľohydráty
- Možnosť vytvoriť denný jedálniček a vypočítať príjem kalórií a nutrientov
- Možnosť ohodnotiť jedálniček podľa porovnania s ideálnym denným príjmom kalórií a makronutrientov pre daný typ osoby (pohlavie, vek, aktivita)


## Zadanie 7 - TODO aplikácia

Program na správu poznámok - TODO aplikácia.

Funkcionality:

- Možnosž spravovaž viacero TODO
- Každé TODO ma názov a položky
- Položka má názov, popis, flag či je splnená a či nie a optional deadline (dokedy je potrebné položku splniž).
- Pomocou tried vhodne namodelovať vyššie uvedené koncepty.
- Analytika: Vypis názvov TODO. Výpis položiek v danom TODO.
- Analytika: Zoradiť položky podľa názvu, deadline, pričom splnené dať na koniec.
- Moznosť vytvoriť a vymazať TODO a položky
- Možnosť označiť položku ako splnenú.


## Zadanie 8 - Analýza filmov

Toto zadanie je pre študentov, s ktorými mám aj OPGP.

Funkcionality:

- Analýza filmov ako sme robili na OPGP v Pythone
- Načítať filmy z JSON súboru https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json
- Pomocou tried vhodne namodelovať databázu a film.
- Vyhľadávanie podľa herca, alebo názvu filmu
- Výpis filmov v danom žánri v danom roku.
- Výpis stránkovať, teda po určitom počte riadkov počkať na stlačenie klávesy a potom pokračovať ďalšou "stranou"


## Zadanie 9 - Mesačný rozpočet

Program na správu mesačného rozpočtu. Práca so súborom.

Funkcionality:

- Pomocou tried vhodne namodelovať mesačný rozpočet a položky
- Mesacny rozpočet obsahuje položky
- Položka má názov a kategóriu
- Možnosť pridať a ubrať položky. Ku každej položke v rozpočte uviesť cenu
- Analytika: Výpočet celkových výdavkov, výpis položiek podľa ceny a kategórie.
- Uloženie rozpočtu do súboru a načítanie rozpočtu zo súboru CSV alebo JSON.
- Aspoň 5 súborov s rôznymi rozpočtami (slobodný muž, slobodná žena, dôchodca, rodina, poslanec).


## Zadanie 10 - Inflácia

Program na analýzu inflácie. Práca so súborom.

Funkcionality:

- Infláciu načítať z .csv zo stránky https://datahub.io/core/inflation 
- Vhodne namodelovať pomocou tried dataset a položku (krajina, inflácia).
- Možnosť zobraziť inflácie pre krajinu alebo inflácie všetkých krajin za určitý rok
- Možnosť vypísať rebríček krajín podľa inflácie na základe zadaného časového obdobia
- Výpis stránkovať, teda po určitom počte riadkov počkať na stlačenie klávesy a potom pokračovať ďalšou "stranou"
