# Teória 15: Princípy OOP

Dnes si zopakujeme princípy objektovo orientovaného programovania a ukážeme si ich využitie na praktickom príklade.

Okrem toho si porovnáme rôzne kategórie vzťahov medzi triedami a zadefinujeme si 2 druhy prevodov medzi triedami objektov.

## Princípy OOP

Existujú 4 hlavné princípy objektovo orientovaného programovanie. Tu je ich stručný prehľad:

* **Zapuzdrenie** (anglicky encapsulation): Skrývanie interných dát objektu (private atribúty, modifikátory prístupu) a poskytovanie kontrolovaného prístupu cez verejné metódy (gettre/settre). Chráni dáta pred neoprávnenou zmenou. Atribúty modelujú HAS-A (objekt má) vzťah

* **Dedičnosť** (anglicky inheritance): Podtrieda zdedí vlastnosti a metódy z rodičovskej triedy, umožňuje znovupoužitie kódu. Modeluje IS-A (objekt je) vzťah.

* **Polymorfizmus**: Metódy sa správajú inak v závislosti od typu objektu. Poznáme statický polymorfizmus (preťažovanie metód - overloading), dynamický polymorfizmus (prekrývanie metód - overriding) a parametrický polymorfizmus (generické triedy)

* **Abstrakcia**: Skrývanie zložitých detailov (AKO sa to robí) a zverejnenie len podstatného (ČO trieda robí). Pomocou abstraktných tried alebo rozhraní. Rozhrania modelujú CAN-DO vzťah (objekt vie)

Tieto princípy sa navzájom dopĺňajú: Abstrakcia a dedičnosť umožňujú polymorfizmus, zapuzdrenie chráni dáta v dedenej hierarchii.

## Vzťahy medzi triedami

Ak existuje medzi dvoma objektami nejaký vzťah, hovoríme vo všeobecnosti, že ide o asociáciu. Asociácia nám hovorí, že objekty sú vzájomne nejako prepojené. Medzi príklady takýchto vzťahov patrí napr. *Učiteľ učí Žiaka*, *Pes žerie Granule*, *Auto tankuje Benzín*, atď.

Existujú aj silnejšie typy asociácií medzi objektami, a pomocou techník OOP ich vieme v návrhu programu vyjadriť a zadefinovať.

* **IS-A (je)** - vzťah dedičnosti, kedy trieda potomka dedí vlastnosti a správanie rodičovskej triedy. *Auto je Vozidlo*, *Pes je Zviera*, *Študent je Osoba*. V Jave dedíme pomocou kľúčového slova `extends`

    ```mermaid
    classDiagram
    direction LR
        class Student  {
        }
        class Osoba  {
        }
        Osoba <|-- Student
    ```

* **CAN-DO (vie)** - vzťah implementácie rozhrania, kedy trieda implementuje konkrétny typ správania, bez nutnosti dediť atribúty alebo vlastnosti nejakej spoločnej triedy. *Lietadlo vie Lietať*, *ElektrickeAuto sa vie Nabíjať*, *Kolekciu viem Iterovať v cykle*. V Jave implementujeme rozhranie pomocou kľúčového slova `implements`

    ```mermaid
    classDiagram
    direction LR
        class Lieta  {
        }
        class Lietadlo  {
        }
        Lieta <|.. Lietadlo
    ```


* **HAS-A (má v sebe)** - vzťah **kompozície**, kedy trieda obsahuje objekt inej triedy ako atribút, pričom tieto objekty bez seba nemôžu existovať. Ide o silnú väzbu medzi triedami, niečo ako *skladá sa z*. Príklady: *Človek má v sebe Srdce*, *Auto má v sebe Motor*, *Dom má v sebe Izby*

    ```mermaid
    classDiagram
    direction LR
        class Auto  {
        }
        class Motor  {
        }
        Auto *.. Motor
    ```


* **HAS-A (má)** - vzťah **agregácie**, kedy trieda obsahuje objekt inej triedy, ale tieto triedy sú na sebe nezávislé a vedia existovať aj samostatne. Ide o slabšiu väzbu medzi triedami. Príklady: *Garáž má Autá*, *Knižnica má Knihy*, *Auto má Pasažierov*. V Jave sa agregácia definuje tak isto pomocou atribútov.

    ```mermaid
    classDiagram
    direction LR
        class Kniznica  {
        }
        class Kniha  {
        }
        Kniznica o.. Kniha
    ```

## Casting tried - prevod typov

 - **Upcasting (prevod nahor)**

    Upcasting je automatický prevod objektu z podtriedy (subclass) na typ nadradenej triedy (superclass).

    Je vždy bezpečný. Podtrieda vždy "je" aj nadradená trieda (podľa princípu "is-a"). Každý objekt podtriedy obsahuje všetko, čo má nadradená trieda.

    Používa sa veľmi často a automaticky, najmä pri práci s polymorfizmom

- **Downcasting (prevod nadol)**

    Downcasting je prevod objektu z nadradenej triedy späť na podtriedu.

    Je riskantný, pretože nie každý objekt nadradenej triedy je aj objektom danej podtriedy. 
    
    Ak sa to nepodarí, program vyhodí počas behu výnimku `ClassCastException`.

    Používa sa zriedka, ak potrebujeme pristúpiť k metódam alebo atribútom, ktoré sú špecifické pre podtriedu. Pred downcastingom sa odporúča vždy použiť operátor `instanceof`.

## Praktický príklad - Hierarchia vozidiel

```java title="Vozidlo.java"
// Abstraktná trieda Vozidlo - tu sa uplatňuje ABSTRAKCIA
// Nemôžeme vytvoriť priamo objekt typu Vozidlo, len jeho podtriedy
// Definuje spoločné vlastnosti a správanie pre všetky vozidlá
public abstract class Vozidlo {
    
    // ZAPUZDRENIE: Atribúty sú private - chránime dáta pred priamym prístupom
    private String znacka;    // napr. "Škoda", "BMW", "Audi"
    private int rokVyroby;    // rok výroby vozidla
    private double rychlost;  // aktuálna rýchlosť v km/h

    // Konštruktor - inicializuje spoločné vlastnosti
    public Vozidlo(String znacka, int rokVyroby) {
        this.znacka = znacka;
        this.rokVyroby = rokVyroby;
        this.rychlost = 0; // na začiatku stojí
    }

    // Gettre – verejné metódy na čítanie privátnych atribútov (zapuzdrenie)
    public String getZnacka() { return znacka; }
    public int getRokVyroby() { return rokVyroby; }
    public double getRychlost() { return rychlost; }
    
    // Setter pre rýchlosť - s jednoduchou kontrolou vstupu (chráni dáta)
    public void setRychlost(double rychlost) {
        if (rychlost >= 0) {  // INVARIANT - rýchlosť nemôže byť záporná
            this.rychlost = rychlost;
        }
    }

    // Abstraktná metóda - Každé vozidlo musí definovať, ako sa pohybuje
    // Podtriedy ju MUSIA prepísať (implementovať)
    public abstract void pohybujSa();

    // Bežná metóda - DEDIČNOSŤ: Všetky podtriedy ju zdedía takú, aká je
    public void zastav() {
        System.out.println(znacka + " zastavuje. Aktuálna rýchlosť: 0 km/h");
        rychlost = 0;
    }

    // Ďalšia bežná metóda - zobrazí info o vozidle (dedí sa)
    public void vypisInfo() {
        System.out.println("Značka: " + znacka + ", Rok výroby: " + rokVyroby + 
                        ", Aktuálna rýchlosť: " + rychlost + " km/h");
    }
}
```

=== "UML diagram triedy"
    ```mermaid
    classDiagram
    direction TB
        class Vozidlo  {
            <<abstract>>
            -String znacka
            -int rokVyrovy
            -double rychlost
            +getZnacka() String
            +getRokVyroby() int
            +getRychlost() double
            +setRychlost(double)
            +zastav()
            +vypisInfo()
            +pohybujSa()*
        }
    ```

```java title="Auto.java"
// Podtrieda Auto - DEDIČNOSŤ: extends Vozidlo - zdedí všetky metódy a premenné
public class Auto extends Vozidlo {

    // Dodatočné privátne atribúty
    private int pocetDveri;

    // Konštruktor podtriedy - volá konštruktor rodiča cez super()
    public Auto(String znacka, int rokVyroby, int pocetDveri) {
        super(znacka, rokVyroby);  // voláme konštruktor abstraktnej triedy
        this.pocetDveri = pocetDveri;
    }

    public int getPocetDveri() { return pocetDveri; }

    // POLYMORFIZMUS: Prepisujeme (override) abstraktnú metódu pohybujSa()
    @Override
    public void pohybujSa() {
        System.out.println(getZnacka() + " ide po ceste na 4 kolesách. Vrrrr!");
        setRychlost(80);  // auto ide rýchlo
    }
}
```

=== "UML diagram triedy"
    ```mermaid
    classDiagram
    direction TB
        class Auto  {
            -int pocetDveri
            +getPocetDveri() int
            +pohybujSa()
        }
    ```

```java title="Bicykel.java"
// Podtrieda Bicykel
class Bicykel extends Vozidlo {

    public Bicykel(String znacka, int rokVyroby) {
        super(znacka, rokVyroby);
    }

    // POLYMORFIZMUS: Iná implementácia rovnakej metódy
    @Override
    public void pohybujSa() {
        System.out.println(getZnacka() + " sa hýbe šliapaním pedálov. Cink!");
        setRychlost(20);  // bicykel ide pomalšie
    }
}
```

```java title="Motorka.java"
// Podtrieda Motorka
class Motorka extends Vozidlo {

    public Motorka(String znacka, int rokVyroby) {
        super(znacka, rokVyroby);
    }

    // POLYMORFIZMUS: Ešte iná verzia
    @Override
    public void pohybujSa() {
        System.out.println(getZnacka() + " reve motorom na 2 kolesách. Vrrr!");
        setRychlost(120);  // motorka ide rýchlo
    }
}
```

=== "UML diagram tried"
    ```mermaid
    classDiagram
    direction TB
        class Vozidlo  {
            <<abstract>>
            -String znacka
            -int rokVyrovy
            -double rychlost
            +getZnacka() String
            +getRokVyroby() int
            +getRychlost() double
            +setRychlost(double)
            +zastav()
            +vypisInfo()
            +pohybujSa()*
        }
        class Auto  {
            -int pocetDveri
            +getPocetDveri() int
            +pohybujSa()
        }
        class Bicykel  {
            +pohybujSa()
        }
        class Motorka  {
            +pohybujSa()
        }
        Vozidlo <|-- Auto : extends
        Vozidlo <|-- Bicykel : extends
        Vozidlo <|-- Motorka : extends
    ```

```java title="ElektroPohon.java"
// ROZHRANIE (interface) - ďalšia forma ABSTRAKCIE
// Definuje schopnosť "elektrického pohonu" - môže ju mať viac typov vozidiel
interface ElektroPohon {
   
    // Abstraktná metóda - ten, kto implementuje interface, ju MUSÍ prepísať
    void nabiBateriu();
    }
}
```

=== "UML diagram rozhrania"
    ```mermaid
    classDiagram
    direction TB
        class ElektroPohon  {
            <<interface>>
            +nabiBateriu()*
        }
    ```

```java title="ElektroAuto.java"
// Nová podtrieda: Elektrické auto – DEDIČNOSŤ + IMPLEMENTÁCIA INTERFACE
class ElektroAuto extends Auto implements ElektroPohon {

    public ElektroAuto(String znacka, int rokVyroby, int pocetDveri) {
        super(znacka, rokVyroby, pocerDveri);
    }

    // POLYMORFIZMUS: Prepísaná metóda z Vozidlo (cez Auto)
    @Override
    public void pohybujSa() {
        System.out.println(getZnacka() + " ide ticho na elektrinu. Hummm!");
        setRychlost(100);
    }

    // Implementácia metódy z interface ElektroPohon
    @Override
    public void nabiBateriu() {
        System.out.println(getZnacka() + " sa nabíja na stanici. Batéria 100%");
    }
}
```

```java title="ElektroBicykel.java"
// Nová podtrieda: Elektrický bicykel
class ElektroBicykel extends Bicykel implements ElektroPohon {

    public ElektroBicykel(String znacka, int rokVyroby) {
        super(znacka, rokVyroby);
    }

    @Override
    public void pohybujSa() {
        System.out.println(getZnacka() + " na elektrinu bez šliapania. Wheee!");
        setRychlost(25);
    }

    @Override
    public void nabiBateriu() {
        System.out.println(getZnacka() + " sa nabíja zo zásuvky. Batéria plná");
    }
}
```

=== "UML diagram tried"
    ```mermaid
    classDiagram
    direction TB
        class Vozidlo  {
            <<abstract>>
            -String znacka
            -int rokVyrovy
            -double rychlost
            +getZnacka() String
            +getRokVyroby() int
            +getRychlost() double
            +setRychlost(double)
            +zastav()
            +vypisInfo()
            +pohybujSa()*
        }
        class Auto  {
            -int pocetDveri
            +getPocetDveri() int
            +pohybujSa()
        }
        class Bicykel  {
            +pohybujSa()
        }
        class Motorka  {
            +pohybujSa()
        }
        class ElektroPohon  {
            <<interface>>
            +nabiBateriu()*
        }
        class ElektroAuto  {
            +nabiBateriu()
            +pohybujSa()
        }
        class ElektroBicykel  {
            +nabiBateriu()
            +pohybujSa()
        }
        Vozidlo <|-- Auto : extends
        Vozidlo <|-- Bicykel : extends
        Vozidlo <|-- Motorka : extends
        Auto <|-- ElektroAuto : extends
        ElektroPohon <|.. ElektroAuto : implements
        Bicykel <|-- ElektroBicykel : extends
        ElektroPohon <|.. ElektroBicykel : implements
    ```


```java title="Garaz.java"
// Hlavná trieda - tu sa najviac prejaví POLYMORFIZMUS
class Garaz {
    
    // ATRIBUTY (zapuzdrenie)
    private String nazovGaraze;    // napr. "Moja garáž"
    private int kapacita;          // maximálny počet vozidiel
    private List<Vozidlo> vozidla; // GENERICS: len Vozidlo a jeho podtriedy!

    // KONŠTRUKTOR
    public Garaz(String nazovGaraze, int kapacita) {
        this.nazovGaraze = nazovGaraze;
        this.kapacita = kapacita;
        this.vozidla = new ArrayList<>(); // Inicializujeme prázdny zoznam
    }

    // METÓDY – správanie garáže
    
    // Pridanie vozidla – s kontrolou kapacity
    public boolean pridajVozidlo(Vozidlo v) {
        if (vozidla.size() < kapacita) {
            vozidla.add(v);  // Polymorfizmus: pridávame akékoľvek Vozidlo
            System.out.println(v.getZnacka() + " bolo pridané do garáže.");
            return true;
        } else {
            System.out.println("Garáž plná! Nemôžem pridať " + v.getZnacka());
            return false;
        }
    }

    // Odstránenie vozidla podľa značky
    public boolean odstranVozidlo(String znacka) {
        for (Vozidlo v : vozidla) {
            if (v.getZnacka().equalsIgnoreCase(znacka)) {
                vozidla.remove(v);
                System.out.println(znacka + " bolo odstránené z garáže.");
                return true;
            }
        }
        System.out.println("Vozidlo so značkou " + znacka + " sa nenašlo.");
        return false;
    }

    // Spustenie všetkých vozidiel – POLYMORFIZMUS v praxi!
    public void spustiVsetkyVozidla() {
        System.out.println("Spúšťame vozidlá v garáži " + nazovGaraze);
        for (Vozidlo v : vozidla) {  // Prechádzame generickým zoznamom
            v.vypisInfo();
            v.pohybujSa(); // Polymorfizmus: každé vozidlo sa pohybuje inak
            v.zastav();
            System.out.println("---");
        }
    }

    // Nabíjanie všetkých elektrických vozidiel – polymorfizmus cez interface
    public void nabiVsetkyElektricke() {
        System.out.println("\n=== Nabíjame elektrické vozidlá ===\n");
        for (Vozidlo v : vozidla) {
            // Kontrola, či implementuje interface
            if (v instanceof ElektroPohon) {  
                // Downcasting z Vozidla na ElektroPohon
                // môžeme si to dovoliť, 
                // lebo sme to vyššie v podmienke otestovali
                ElektroPohon ep = (ElektroPohon) v;  
                ep.vypisElektrickuInfo();
                ep.nabiBateriu();
                System.out.println("---");
            }
        }
    }

    // Getter na info o garáži
    public void vypisInfoOGarazi() {
        System.out.println("Garáž: " + nazovGaraze + 
                           ", Počet vozidiel: " + 
                           vozidla.size() + "/" + kapacita);
    }
}
```

=== "UML diagram tried"
    ```mermaid
    classDiagram
    direction TB
        class Vozidlo  {
            <<abstract>>
            -String znacka
            -int rokVyrovy
            -double rychlost
            +getZnacka() String
            +getRokVyroby() int
            +getRychlost() double
            +setRychlost(double)
            +zastav()
            +vypisInfo()
            +pohybujSa()*
        }
        class Auto  {
            -int pocetDveri
            +getPocetDveri() int
            +pohybujSa()
        }
        class Bicykel  {
            +pohybujSa()
        }
        class Motorka  {
            +pohybujSa()
        }
        class ElektroPohon  {
            <<interface>>
            +nabiBateriu()*
        }
        class ElektroAuto  {
            +nabiBateriu()
            +pohybujSa()
        }
        class ElektroBicykel  {
            +nabiBateriu()
            +pohybujSa()
        }
        class Garaz  {
            -String nazovGaraze
            -int kapacita
            -List vozidla
            +pridajVozidlo(Vozidlo): boolean
            +odstranVozidlo(String): boolean
            +spustiVsetkyVozidla()
            +nabiVsetkyElektricke()
            +vypisInfoOGarazi()
        }
        Vozidlo <|-- Auto : extends
        Vozidlo <|-- Bicykel : extends
        Vozidlo <|-- Motorka : extends
        Auto <|-- ElektroAuto : extends
        ElektroPohon <|.. ElektroAuto : implements
        Bicykel <|-- ElektroBicykel : extends
        ElektroPohon <|.. ElektroBicykel : implements
        Vozidlo --o Garaz
    ```

=== "UML diagram triedy Garaz"
    ```mermaid
    classDiagram
    direction TB
        class Vozidlo  {
            <<abstract>>
            -String znacka
            -int rokVyrovy
            -double rychlost
            +getZnacka() String
            +getRokVyroby() int
            +getRychlost() double
            +setRychlost(double)
            +zastav()
            +vypisInfo()
            +pohybujSa()*
        }
        class Garaz  {
            -String nazovGaraze
            -int kapacita
            -List vozidla
            +pridajVozidlo(Vozidlo): boolean
            +odstranVozidlo(String): boolean
            +spustiVsetkyVozidla()
            +nabiVsetkyElektricke()
            +vypisInfoOGarazi()
        }
        Vozidlo --o Garaz
    ```



```java title="Main.java"
public class Main {
    public static void main(String[] args) {
        
        // Vytvoríme garáž s kapacitou 5 vozidiel
        Garaz mojaGaraz = new Garaz("Domáca garáž", 5);

        mojaGaraz.vypisInfoOGarazi();

        // Pridávame rôzne vozidlá (polymorfizmus)
        mojaGaraz.pridajVozidlo(new Auto("Škoda Octavia", 2020, 5));
        mojaGaraz.pridajVozidlo(new Motorka("Honda", 2019));
        mojaGaraz.pridajVozidlo(new ElektroAuto("Tesla Model 3", 2024, 5));
        mojaGaraz.pridajVozidlo(new Bicykel("Dema", 2022));
        mojaGaraz.pridajVozidlo(new ElektroBicykel("Specialized", 2023));

        // Skúsime pridať šieste – malo by zlyhať
        mojaGaraz.pridajVozidlo(new Auto("BMW", 2023));

        mojaGaraz.vypisInfoOGarazi();

        // Testujeme správanie
        mojaGaraz.spustiVsetkyVozidla();

        mojaGaraz.nabiVsetkyElektricke();

        // Odstránime jedno vozidlo
        mojaGaraz.odstranVozidlo("Honda");
        mojaGaraz.spustiVsetkyVozidla();
    }
}
}
```

## Zhrnutie teórie

- [x] OOP Princípy
    * [ ] Zapuzdrenie (anglicky encapsulation): Skrývanie interných dát objektu (private atribúty, modifikátory prístupu) a poskytovanie kontrolovaného prístupu cez verejné metódy (gettre/settre)
    * [ ] Dedičnosť (anglicky inheritance): Podtrieda zdedí vlastnosti a metódy z rodičovskej triedy, umožňuje znovupoužitie kódu
    * [ ] Polymorfizmus: Metódy sa správajú inak v závislosti od typu objektu. Poznáme statický polymorfizmus (preťažovanie metód - overloading), dynamický polymorfizmus (prekrývanie metód - overriding) a parametrický polymorfizmus (generické triedy)
    * [ ] Abstrakcia: Skrývanie zložitých detailov (AKO sa to robí) a zverejnenie len podstatného (ČO trieda robí). Pomocou abstraktných tried alebo rozhraní.
- [x] Vzťahy medzi triedami - druhy asociácie
    * [ ] Všeobecná asociácia - prepojenie - Učiteľ učí Žiaka, Pes žerie Granule, Auto tankuje Benzín, atď.
    * [ ] IS-A (je) - vzťah dedičnosti, kedy trieda potomka dedí vlastnosti a správanie rodičovskej triedy. Auto je Vozidlo, Pes je Zviera, Študent je Osoba. V Jave dedíme pomocou kľúčového slova extends
    * [ ] CAN-DO (vie) - vzťah implementácie rozhrania, kedy trieda implementuje konkrétny typ správania. Lietadlo vie Lietať, ElektrickeAuto sa vie Nabíjať, Kolekciu viem Iterovať v cykle. V Jave implementujeme rozhranie pomocou kľúčového slova implements
    * [ ] HAS-A (má v sebe) - vzťah kompozície, kedy trieda obsahuje objekt inej triedy ako atribút, pričom tieto objekty bez seba nemôžu existovať. Ide o silnú väzbu medzi triedami, niečo ako skladá sa z. Príklady: Človek má v sebe Srdce, Auto má v sebe Motor, Dom má v sebe Izby
    * [ ] HAS-A (má) - vzťah agregácie, kedy trieda obsahuje objekt inej triedy, ale tieto triedy sú na sebe nezávislé a vedia existovať aj samostatne. Ide o slabšiu väzbu medzi triedami. Príklady: Garáž má Autá, Knižnica má Knihy, Auto má Pasažierov. V Jave sa agregácia definuje tak isto pomocou atribútov.
- [x] Casting tried - prevod typov
    * [ ] Upcasting je automatický prevod objektu z podtriedy (subclass) na typ nadradenej triedy (superclass).
    * [ ] Downcasting je prevod objektu z nadradenej triedy späť na podtriedu. Je riskantný, pretože nie každý objekt nadradenej triedy je aj objektom danej podtriedy. 
    * [ ] Ak sa downcasting nepodarí, program vyhodí počas behu výnimku ClassCastException
    * [ ] Pred downcastingom sa odporúča vždy použiť operátor instanceof.

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Druhy asociácie medzi triedami

    Všeobecná asociácia - prepojenie - Učiteľ učí Žiaka, Pes žerie Granule
    
    IS-A (je) - vzťah dedičnosti - extends. Auto je Vozidlo, Pes je Zviera
    
    CAN-DO (vie) - vzťah implementácie rozhrania - implements. 
    Lietadlo vie Lietať, ElektrickeAuto sa vie Nabíjať
    
    HAS-A (má v sebe) - vzťah kompozície - skladá sa z. Silná väzba, 
    triedy bez seba nevedia existovať. Pomocou atribútov. 
    Človek má v sebe Srdce, Dom má v sebe Izby
    
    HAS-A (má) - vzťah agregácie. Slabšia väzba, trieda obsahuje iné objekty, 
    ale vedia existovať aj samostatne. Pomocou atribútov. Knižnica má Knihy, Auto má Pasažierov

    CASTING Tried

    Upcasting je automatický prevod objektu z podtriedy (subclass) na typ nadradenej 
    triedy (superclass). Používa sa pri polymorfizme.

    Downcasting je prevod objektu z nadradenej triedy späť na podtriedu.

    Je riskantný a ak sa nepodarí, program vyhodí výnimku ClassCastException.

    Používa sa zriedka, ak potrebujeme metódy podtriedy. 
    Pred downcastingom treba použiť operátor instanceof.
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Akým spôsobom Java podporuje základné princípy OOP
    - Druhy asociácie medzi triedami
    - Rozdiel medzi Agregáciou a Kompozíciou
    - Značky v UML class diagrame pre jednotlivé typy vzťahov
    - Upcasting a Downcasting, rozdiely, použitie
