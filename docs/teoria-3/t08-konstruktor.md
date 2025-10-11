# Teória 8: Inicializácia objektov, konštruktory

- trieda v java api ktora nema public konstruktory, LocalDate???
- konstruktory
- inicializacia objektu
- inicializacia cez referenciu/pomocou metody/pomocou konstruktora
- konstruktory, pretazovanie

- defaultny konstruktor, nullary/no argument konstruktor, parametrizovany konstruktor, copy konstruktor
- inicializacia defaultna, ak neuvediem inac
- inicializacia v deklaracii atributu - field initializers
- priame vytvorenie objektu vzdy iba cez new. new vykona alokaciu a potom inicializaciu

Konštruktor je špeciálna metóda volaná pri new, s menom triedy a bez návratového typu.
- delegovanie cez this(a, c, c)

- instancny inicializacny blok
- staticky inicializator


## Proces vytvárania objektu

Priame vytvorenie objektu sa realizuje vždy pomocou operátora `new`. Ak aj máme nejakú továrenskp metódu, ktorá nám vracia nový objekt, vnútri tej továrenskej metódy je niekde ukryté vytvorenie objektu pomocou `new`. Vytváranie objektu sa skladá z dvoj častí:

- Alokácia pamäti pre objekt
- Inicializácia objektu

Alokáciu pamäti má na starosti JVM a nemusíme sa o to starať. Inicializácia je však niečo, čo vieme ovplyvniť.


## Inicializácia objektu

Cieľom inicilizácie objektu je nastaviť všetky jeho atribúty na správne hodnoty. Inicializáciu môžeme previesť rôznymi spôsobmi.

### Priama inicializácia atribútov

Inicializovať atribúty triedy môžeme priamo pri ich deklarácii.

=== "Priama inicializácia atribútov triedy"

    ```java
    public class Rat {
        private int health = 100;

        public int getHealth() {
            return health;
        } 
    }

    // použitie
    Rat potkan = new Rat();
    assert potkan.getHealth() == 100 : "Mlady potkan ma mat zdravie 100";

    ```

Atribúty sa inicializujú v poradí, v akom sú deklarované v kóde.

V dnešnej časti si vysvetlíme, aké máme v Jave rôzne druhy metód.

## Základná syntax metódy

Metóda je funkcia, ktorá patrí určitej triede. Môže prijímať vstupy (argumenty), môže vrátiť výsledok (return value), a môže byť volaná z iných častí programu. Spolu s triedami sú metódy základom modularity a znovupoužiteľnosti kódu.

Metóda sa vždy píše vnútri konkrétnej triedy a má nasledovnú syntax:

=== "Syntax metódy"

    ```
    <modifikátor> <návratovýTyp> <nazovMetody>(<argumenty>) <throws> {
        // telo metódy
        return value; // ak metóda nie je void
    }
    ```

=== "Príklad metódy"

    ```java
    public int add(int a, int b) {
        return a + b;
    }
    ```

## Statické metódy

Statické metódy na volanie nepotrebujú konkrétny objekt. Statické metódy patria triede v ktorej sú definované a vedia pristupovať iba k statickým atribútom triedy. Typickým príkladom statickej metódy je vstupný bod programu `public static void main(String[] args)`.

Statické metódy sa volajú cez názov triedy, napríklad `Arrays.sort()`. Ak statickú metódu voláme vnútri tej istej triedy, stačí uviesť jej názov.

Hlavné využitie statických metód sú **pomocné funkcionality** (utilitky), **factory metódy** (továrenské metódy) a **vstupný bod programu** (main).

Príklady statických metód:

- `Math.random()`
- `Math.max()`
- `Arrays.fill()`
- `Integer.toString()`
- `Objects.requireNonNull()`

=== "Trieda so statickými metódami"

    ```java
    public class Vypocty {

        public static int sucetInt(int a, int b) {
            return a + b;
        }

        public static double priemer(double... cisla) {
            double suma = 0;
            for (double n : cisla) suma += n;
            return suma / cisla.length;
        }

        public static int faktorial(int n) {
            if (n <= 1)
                return 1;
            return n * faktorial(n - 1);
        }

        public static void main(String[] args) {
            int n = 10;
            System.out.print("Faktorial cisla %d je %d", n, faktorial(n));
        }

    }
    ```

## Inštančné metódy

Najbežnejšie metódy v Jave sú inštančné.
Inštančné metódy už pracujú s objektom, preto ho musíme pri volaní metódy uviesť. Tieto metódy majú prístup k všetkým atribútom svojej triedy.

=== "Trieda s inštančnými metódami"

    ```java
    public class Obdlznik {
        private int a;
        private int b;

        // konštruktor
        public Obdlznik(int a, int b) {
            this.a = a;
            this.b = b;
        }

        // inštančná metóda
        public int obvod() {
            return 2 * (this.a + this.b);
        }

        // inštančná metóda
        public String toString() {
            return String.format("%dx%d", a, b);
        }

        // inštančná metóda
        public boolean equals(Obdlznik other) {
            return this.a == other.a && this.b == other.b;
        }
    }
    ```

Každá inštančná metóda má v sebe definovanú **špeciálnu premennú `this`**, pomocou ktorej vieme pristupovať k objektu, nad ktorým bola daná metóda zavolaná.

=== "Spôsob volania inštančných metód"

    ```java
    Obdlznik obd = new Obdlznik(10, 15);
    System.out.printf("Obdod obdlznika %s je %d", obd, obd.obvod());
    ```

### Špeciálne inštančné metódy

Existuje niekoľko metód, ktoré majú v Jave špecíálny význam a môžete ich vo svojej triede vytvoriť. V predchádzajúcom príklade sme uviedli 2 z nich, metódy `toString()` a `equals()`.

Metóda `toString()` sa zavolá vždy, keď potrebujeme objekt previesť na reťazec, napríklad pri vypísaní objektu na obrazovku.

```java
// zavolá sa obd.toString() a výsledok sa vypíše
System.out.printf("Obdlznik %s", obd); 
```

Druhá metóda, `equals()` slúži na porovnanie dvoch objektov podľa hodnoty. Ako sme si už pár krát hovorili, objekty sa neporovnávajú pomocou `==`, pretože by sme porovnali ich identitu. Porovnanie podľa hodnôt si musíme naprogramovať sami, pretože každá trieda má svoju vlastnú predstavu o tom, čo je jej hodnota.

V prípade našej triedy `Obdlznik` by sme mohli povedať, že dve obdĺžniky sú rovnaké, ak sú rovnaké dĺžky ich strán. Podľa toho teda naimplementujeme aj metódu `equals()`

```java
public boolean equals(Obdlznik other) {
    return this.a == other.a && this.b == other.b;
}
```

Túto metódy potom vieme použit v porovnávaní objektov nasledovne:

```java
Obdlznik obd1 = new Obdlznik(10, 15);
Obdlznik obd2 = new Obdlznik(10, 15);
Obdlznik obd3 = new Obdlznik(20, 5);

if(obd1.equals(obd2)) {
    System.out.println("Obdĺžniky obd1 a obd2 sú rovnaké.");
}
if(obd2.equals(obd3)) {
    // tento kód sa nevykoná
    System.out.println("Obdĺžniky obd2 a obd3 sú rovnaké.");
}
```

## Preťaženie metód

V Jave môžeme mať viacero metód s rovnakým menom. Musia sa však líšiť v type alebo počte argumentov. Týmto spôsobom vieme volať rôzne verzie metód podľa toho, koľko alebo aké argumenty v metóde zašleme. Takáto definícia viacerých metód v rovnakým názvom sa nazýva **preťaženie**, anglicky **overloading**

Preťažené metódy sa musia vzájomne líšiť počtom argumentov alebo typmi argumentov. Na návratový typ sa neprihliada, teda ak má preťažená metóda iba iný návratový typ, je to chyba a program sa neskompiluje.

=== "Príklad preťaženia statických metód"

    ```java
    class Kalukacka {
        public static int add(int a, int b) {
            return a + b;
        }

        public static double add(double a, double b) {
            return a + b;
        }

        public static int add(int a, int b, int c) {
            return a + b + c;
        }
    }
    ```

## Getter a Setter metódy

Pri návrhu tried je v drvivej väčšine prípadov zlým dizajnom, ak atribúty triedy sprístupníme hocikomu, teda ak im dáme modifikátor `public`. Takmer stále je najlepšie mať všetky atribúty súkromné, teda dať im modifikátor `private`.

Počiatočné hodnoty atribútov triedy sa nastavujú v konštruktori (viac o konštruktoroch nabudúce). Ak však máme existujúci objekt a chceme zistiť hodnotu niektorého z jeho atribútov, priamo to nepôjde a musíme v takom objekte vytvoriť inštančné metódy, ktoré vrátia hodnoty atribútov. Takýmto metódam sa hovorí getter metódy, pretože sa ich názov začína slovkom `get`.

Podobne aj v prípade, ak chceme dovoliť, aby sa dali niektoré z atribútov meniť, je potrebné vytvoriť na to určené metódy. Tie sa zvyknú volať setter metódy, pretože ich názov začína slovom `set`.

=== "Príklad getter a setter metód"

    ```java
    public class Obdlznik {
        private int a;
        private int b;

        // konštruktor
        public Obdlznik(int a, int b) {
            this.a = a;
            this.b = b;
        }

        // getter metóda
        public int getA() {
            return a;
        }

        // getter metóda
        public int getB() {
            return b;
        }

        // setter metóda
        public void setA(int a) {
            if (a <= 0) {
                throw new IllegalArgumentException("Hodnota musí byť kladná");
            }
            this.a = a;
        }

        // setter metóda
        public void setB(int b) {
            if (b <= 0) {
                throw new IllegalArgumentException("Hodnota musí byť kladná");
            }
            this.b = b;
        }
    }
    ```

Pomocou getter a setter metód oddelíme vnútornú implementáciu triedy (aké atribúty sme použili) od spôsobu, aké operácie povolíme nad objektom vytvárať. V getter a setter metódach môžeme mať dodatočnú logiku, napríklad konverzie hodnôt alebo kontroly argumentov. Tým vieme zaručiť lepšiu konzistenciu stavu objektu. Ak by sme zverejnili atribúty pre všetkých, nemáme kontrolu nad tým, aké zmeny tam iný môžu urobiť.

Takéto skrývanie stavu vo vnútri triedy sa v programovaní volá **enkapsulácia**. Detailne sa jej budeme venovať na inej hodine.

## Továrenské metódy

Niekedy je vhodné dať užívateľovi našej triedy možnosť vytvoriť objekty triedy aj pomocou statických metód. Statické metódy, ktoré vracajú inštanciu svojej triedy sa volajú **továrenské metódy**, anglicky **factory methods**.

Továrenské metódy oddeľujú proces vytvárania objektov od kódu, ktorý ich používa.

Továrenské metódy nemusia vždy vytvoriť objekt pomocou operátora new, niekedy vrátia už existujúci predpripravený objekt, alebo iným spôsobom získaju inštanciu triedy. Továrenské metódy s používajú kvôli väčšej flexibilite, efektivite a lepším možnostiam riadiť priebeh tvorby objektu.

Príklady továrenských metód v knižniciach Java:

- `LocalDate.now()` vytvorí nový objekt s aktuálnym dátumom
- `URI.create()` vytvorí URI identifikátor
- `Integer.valueOf()` vytvorí číslo z reťazca znakov

Názvy továrenských metód zvyknú byť `getInstance`, `newInstance`, `from`, `valueOf`, `create`, `copyOf` a podobne.

=== "Príklad továrenskej metódy"

    ```java
    public class Obdlznik {
        private int a;
        private int b;

        // konštruktor
        public Obdlznik(int a, int b) {
            this.a = a;
            this.b = b;
        }

        // továrenská metóda
        public static Obdlznik createNaSirku(int a, int b) {
            if (a > b) 
                new Obdlznik(a, b); 
            else
                new Obdlznik(b, a); 
        }
    }
    ```

## Metódy v knižniciach Java API

Uvedieme si zopár príkladov metód z knižníc Java API

- [`java.net.URL`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/net/URL.html), trieda pre reprezentáciu URL odkazov, obsahujúca množstvo getter metód
- [`java.lang.String`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html) obsahuje množstvo továrenských metód
- [`java.util.Arrays`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html) obsahuje statické metódy, utility pre prácu s poliami
- [`java.time.LocalDate`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/LocalDate.html) obsahuje getter aj továrenské metódy

## Zhrnutie teórie

- [x] Metóda je funkcia patriaca určitej triede
- [x] Statické metódy
    * [ ] Nepatria objektu ale triede ako takej
    * [ ] Volajú sa cez názov triedy, napríklad `Arrays.sort()`
    * [ ] Hlavné využitie statických metód sú pomocné funkcionality (utilitky), factory metódy (továrenské metódy) a vstupný bod programu (main)
- [x] Inštančné metódy
    * [ ] Najbežnejšie metódy v Jave
    * [ ] Inštančné metódy už pracujú s objektom, preto ho musíme pri volaní metódy uviesť.
    * [ ] Tieto metódy majú prístup k všetkým atribútom svojej triedy.
    * [ ] Každá inštančná metóda má v sebe definovanú špeciálnu premennú `this`
- [x] Špeciálne inštančné metódy
    * [ ] `toString()` sa zavolá vždy, keď potrebujeme objekt previesť na reťazec, napríklad pri vypísaní objektu na obrazovku.
    * [ ] `equals()` slúži na porovnanie dvoch objektov podľa hodnoty
- [x] Preťaženie metód
    * [ ] V Jave môžeme mať viacero metód s rovnakým menom
    * [ ] Preťažené metódy sa musia vzájomne líšiť počtom argumentov alebo typmi argumentov
    * [ ] Definícia viacerých metód v rovnakým názvom sa nazýva preťaženie, anglicky *overloading*
    * [ ] Na návratový typ sa neprihliada
- [x] Getter a Setter metódy
    * [ ] Najlepšie je mať všetky atribúty triedy súkromné (private) a ovládať ich cez metódy
    * [ ] Getter metódy slúžia na zistenie hodnoty nejakého atribátu v objekte
    * [ ] Setter metódy slúžia na zmenu hodnoty v objekte
- [x] Továrenské metódy
    * [ ] Statické metódy, ktoré vracajú inštanciu svojej triedy sa volajú továrenské metódy, anglicky factory methods.
    * [ ] Továrenské metódy oddeľujú proces vytvárania objektov od kódu, ktorý ich používa
    * [ ] Továrenské metódy s používajú kvôli väčšej flexibilite, efektivite a lepším možnostiam riadiť priebeh tvorby objektu.
    * [ ] Názvy továrenských metód zvyknú byť `getInstance`, `newInstance`, `from`, `valueOf`, `create`, `copyOf` a podobne.

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    METÓDY

    Statické metódy
    - Nepatria objektu ale triede ako takej
    - Volajú sa cez názov triedy, napríklad Arrays.sort()
    - Využitie: utilitky, factory metódy a vstupný bod programu

    Inštančné metódy
    - pracujú s objektom, volanie obj.metoda()
    - majú prístup k všetkým atribútom svojej triedy
    - majú v sebe definovanú špeciálnu premennú this, pomocou ktorej pristupujeme k objektu

    Špeciálne inštančné metódy toString() a equals()

    Preťaženie metód
    - Môžeme mať viacero metód s rovnakým menom
    - Preťažené metódy sa musia vzájomne líšiť počtom argumentov alebo typmi argumentov
    - Definícia viacerých metód v rovnakým názvom sa nazýva preťaženie, anglicky overloading
    - Na návratový typ sa neprihliada

    Getter metódy slúžia na zistenie hodnoty nejakého atribútu v objekte
    Setter metódy slúžia na zmenu hodnoty v objekte

    Továrenské metódy - factory methods
    Sú to statické metódy, ktoré vracajú inštanciu svojej triedy
    Továrenské metódy oddeľujú proces vytvárania objektov od kódu, ktorý ich používa
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Rozdiel medzi statickými a inštančnými metódami
    - Ako voláme statické a ako voláme inštančné metódy
    - Na čo slúži špeciálna premenná this
    - Na čo máme Getter a Setter metódy
    - Čo je preťaženie metód, aké sú podmienky pre preťažené metódy
    - Čo sú továrenské metódy a na čo sa používajú
