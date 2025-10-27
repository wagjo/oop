# Cvičenie 7: Triedy a metódy

Dnešné cvičenie je zamerané na opakovanie a upevnenie znalostí o tvorbe tried a metód v Jave.


## Vytváranie tried

Trieda (anglicky Class) v Jave je základný stavebný blok programu. Každá verejná trieda musí byť v samostatnom súbore, ktorý má rovnaký názov, ako daná trieda. Názov triedy začína veľkým písmenom.

Triedu vieme priradiť do balíka (anglicky package), ktorý slúži na organizáciu tried v našom projekte. Balíku je na disku reprezentovaný adresárom, resp. adresárovou cestou. Trieda musí byť v adresári, ktorý zodpovedá jej balíku.

Ak máme triedu `sk.spse.Krabica`, tak platia nasledovné pravidlá:

- Názov balíka, v ktorom je trieda, je `sk.spse`
- Názov triedy je `Krabica`
- Súbor s triedou má názov `Krabica.java`
- Súbor s triedou musí byť na adresárovej ceste `sk\spse`
- Na veľkosti písmen závisí


## Vytváranie metód

Metódy sú funkcie, ktoré patria triede, a vedia pracovať s atribútmi objektu (inštančné metódy), alebo patria triede samotnej (statické metódy)

Definíciu metód píšeme do vnútra triedy. Každá metóda má okrem názvu aj návratový typ a zoznam vstupných argumentov.

Pravidlá:

- Ak trieda nevracia žiadnu hodnotu, ako návratový typ uvedieme kľúčové slovo `void`
- Metóda má svoj modifikátor prístupu (`public`, `private`, ...)
- Statickú metódu deklarujeme s kľúčovým slovom `static`
- Vnútri inštančnej metódy môžeme použiť `this` ako odkaz na aktuálny objekt. Statické metódy nemajú `this` k dispozícii.

Pri písaní kódu v IntelliJ IDEA si môžeme pomôcť automatickým formátovaním, ktoré vykonáme klávesami ++"Ctrl"+"Alt"+"L"++.


## Trieda `Potravina`

!!! example "Úloha 7.1: Trieda Potravina"

    Vytvorte triedu `sk.spse.strava.Potravina`, ktorá bude obsahovať atribúty `nazov`, `cena` a `kalorie` a konštruktor, ktorý nastaví atribúty:

    ```java
    // deklarácia balíka
    package sk.spse.strava;

    // deklarácia triedy
    public class Potravina {
        
        // atribúty
        private String nazov;
        private double cena;
        private int kalorie;
        
        // konštruktor
        public Potravina(String nazov, double cena, int kalorie) {
            this.nazov = nazov;
            this.cena = cena;
            this.kalorie = kalorie;
        }
    }
    ```

Je zaužívaným pravidlom nemať atribúty verejné, ale radšej vytvoriť tzv. getter metódy, ktoré vrátia hodnotu atribútov triedy. Takto máme väčšiu kontrolu nad hodnotami atribútov a nad tým, kto môže s atribútmi pracovať.

!!! example "Úloha 7.2: Getter metódy"

    V triede `sk.spse.strava.Potravina` vytvorte nasledovné getter metódy

    - `getNazov()`
    - `getCena()`
    - `getKalorie()`

Ak chceme hodnoty objektu vypisovať do konzoly, je vhodné v triede napísať metódu `toString()`, v ktorej vieme uviesť textovú reprezentáciu objektu.

!!! example "Úloha 7.3: metóda toString"

    V triede `sk.spse.strava.Potravina` vytvorte metódu `toString()`. 
    
    Táto metóda nech vráti reťazec tvaru `<nazov> (<cena>eur <kalorie> kcal)`

## Trieda `Jedalnicek`

!!! example "Úloha 7.4: Jedálniček"

    Vytvorte triedu `sk.spse.strava.Jedalnicek`, ktorá bude obsahovať atribúty `ranajky`, `obed` a `vecera`, všetko polia Potravín, a konštruktor, ktorý nastaví atribúty:

    ```java
    package sk.spse.strava;

    public class Jedalnicek {
        private Potravina[] ranajky;
        private Potravina[] obed;
        private Potravina[] vecera;

        public Jedalnicek(Potravina[] ranajky,  Potravina[] obed, Potravina[] vecera) {
            this.ranajky = ranajky;
            this.obed = obed;
            this.vecera = vecera;
        }
    }
    ```

Podobne ako aj pri triede `Potravina` aj trieda `Jedalnicek` potrebuje metódu `toString`

!!! example "Úloha 7.5: metóda toString"

    V triede `sk.spse.strava.Jedalnicek` vytvorte metódu `toString()`. 
    
    Táto metóda nech vráti reťazec tvaru `Raňajky: <ranajky>\nObed: <obed>\nVečera: <večera>`.

    Pri vypisovaní jedál si pomôžte metódou `Arrays.toString`.

!!! example "Úloha 7.6: výpočet celkovej ceny a kalórii"

    Do triedy `sk.spse.Jedalnicek` vytvorte nasledovné metódy:

    - `cena`, ktorá vráti celkovú cenu jedál v jedálničku
    - `kaloria`, ktorá vráti celkové kalórie jedál

## Trieda `Main`

Máme vytvorené 2 triedy, ktoré nám reprezentujú potravinu a denný jedálniček. Teraz si vyskúšame ich použitie.

!!! example "Úloha 7.7: metóda `main`"

    Vytvorte triedu `sk.spse.strava.Main` a v nej metódu `main`, ktorá vytvorí a vypíše jednoduchý jedálniček

    ```java
    Potravina rozok = new Potravina("Rožok", 0.12, 143);
    Potravina horalka = new Potravina("Horalky", 0.75, 270);
    Potravina treska = new Potravina("Treska Exclusiv", 1.89, 330);
    Potravina jablko = new Potravina("Jablko Evelina", 0.52, 140);
    Potravina banan = new Potravina("Banán", 0.22, 180);
    Potravina pizza = new Potravina("Pizza Ristorante", 2.19, 780);
    Potravina pepsi = new Potravina("Pepsi Cola", 0.86, 100);
    Potravina vifonka = new Potravina("Vifon Kuracia", 0.49, 40);
    Potravina burger = new Potravina("McCrispy", 5.10, 457);
    Potravina hranolky = new Potravina("Stredné hranolky", 2.50 , 327);

    Potravina[] ranajky = {rozok, rozok, treska};
    Potravina[] obed = {pepsi, pizza};
    Potravina[] vecera = {jablko, banan, vifonka};

    Jedalnicek jedalnicek = new Jedalnicek(ranajky, obed, vecera);

    System.out.printf("Môj dnešný jedálniček:\n%s", jedalnicek);
    System.out.printf("\nCelková cena: %.2feur", jedalnicek.cena());
    System.out.printf("\nCelkové kalórie: %dkcal", jedalnicek.kalorie());
    ```

    Mali by ste dostať nasledovný výstup:

    ```
    Môj dnešný jedálniček:
    Raňajky: [Rožok (0.12eur 143kcal), Rožok (0.12eur 143kcal), Treska Exclusiv (1.89eur 330kcal)]
    Obed: [Pepsi Cola (0.86eur 100kcal), Pizza Ristorante (2.19eur 780kcal)]
    Večera: [Jablko (0.52eur 140kcal), Banán (0.22eur 180kcal), Vifon Kuracia (0.49eur 40kcal)]
    Celková cena: 6.41eur
    Celkové kalórie: 1856kcal
    ```

## Úlohy na precvičenie

!!! example "Úloha 7.8: Najdrahšia a najkalorickejšia potravina"

    V trieda `Jedalnicek` vytvorte metódu, ktorá vráti najdrahšiu potravinu a inú metódu, ktorá vráti najkalorickejšiu potravinu.

    Metódy použite v metóde `main` a vypíšte tieto naj potraviny.

!!! example "Úloha 7.9: Nové potraviny"

    Vymyslite si 10 nových potravín, s čo najreálnejšími cenami a kalóriami. Vytvorte pole všetkých potravín a nazvite ho `sklad`.

!!! example "Úloha 7.10: Menu"

    Vytvorte triedu `sk.spse.strava.Menu`, v ktorej budete mať statickú metódu `generujMenu(Potraviny[] sklad, double maxCena, int maxKalorie)`

    Táto metóda má vrátiť pole Potravín tak, že bude náhodne vyberať potraviny so zoznamu, až kým nie je dosiahnutá celková cena alebo celkový počet kalórii. Výsledný zoznam potravín nesmie prekročiť uvedené maximálne hodnoty.

    Metódu použite, vhodne zavolajte a vypíšte výsledok.

!!! example "Úloha 7.11: Denné menu"

    Do triedy `sk.spse.strava.Menu` pridajte metódu `generujDenneMenu(Potraviny[] zoznam, double maxCena, int maxKalorie)`, ktorá bude fungovať podobne ako v predchádzajúcom príklade, ale vráti nie pole potravín, ale jeden objekt triedy `Jedalnicek`, pričom potraviny rozloží rovnomerne medzi raňajky, obed a večeru.


!!! example "Úloha 7.12: Týždenný jedálniček"

    Vytvorte triedu `sk.spse.strava.TyzdennyJedalnicek`, ktora bude obsahovať 7 jedálničkov na každý deň.

    Na uloženie jedálničkov použite pole. Vytvorte vhodný konštruktor a metódu `toString`

    Vytvorte týždenný jedálniček a vypíšte ho do konzoly.

    Zistite celkovú cenu za celý týždeň.

!!! example "Úloha 7.13: Nutričný poradca"

    V triede `sk.spse.Main` vytvorte statickú metódu `nutricnyPoradca`, ktorá má na vstupe jedálniček a vypíše, čí je počet kalórii v norme, alebo či je kalórii nedostatok alebo prebytok. Norma je v rozmedzí 2000-3000 kalórii.

    Zavolajte nutričného poradcu na každý deň týždenného jedálnička

## Domáca úloha

!!! example "Domáca úloha"

    Vytvorte triedu `Osoba` s atribútmi meno, pohlavie a rok narodenia. Vytvorte konštruktor pre vytváranie objektov danej triedy.

    V triede vytvorte getter metódy pre všetky atribúty.

    V triede vytvorte metódu toString.

    V triede vytvorte metódu `getVek`, ktorá vráti aktuálny vek osoby. Na zistenie aktuálneho roka môžete použiť `LocalDate.now().getYear()`.

    V triede vytvorte statickú metódu `String getGeneracia(int rokNarodenia)`, ktorá vráti názov generácie podľa roku narodenia, napr. Generácia X, Zoomer, atď. Zoznam generácii môžete nájsť na stránke [https://sk.wikipedia.org/wiki/Generácia_(pokolenie_ľudí)](https://sk.wikipedia.org/wiki/Gener%C3%A1cia_(pokolenie_%C4%BEud%C3%AD)). Rozpoznajte aspoň 5 generácií.

    Súbor s triedou vhodne nazvite podľa pravidiel Javy a zašlite cez Edupage v danej domácej úlohe do budúceho cvičenia.

## Zhrnutie cvičenia

- [x] Opakovanie vytvárania tried a balíkov
- [x] Opakovanie vytvárania metód

!!! note "Poznámky do zošita"
    Toto cvičenie si do zošita nemusíte písať žiadne poznámky

!!! warning "Skúšanie a kontrola vedomostí"

    Vypracovať domácu úlohu do budúceho cvičenia

    Okruhy otázok na test:

    - Vedieť vytvoriť a použiť balíky, triedy a metódy
