# Teória 8: Inicializácia objektov, konštruktory


## Proces vytvárania objektu

Priame vytvorenie objektu sa realizuje vždy pomocou operátora `new`. Ak aj máme nejakú továrenskú metódu, ktorá nám vracia nový objekt, vnútri továrenskej metódy je niekde ukryté vytvorenie objektu pomocou `new`. Vytváranie objektu sa skladá z dvoch častí:

- Alokácia pamäti pre objekt
- Inicializácia objektu

Alokáciu pamäti má na starosti JVM a nemusíme sa o to starať. Inicializácia je však niečo, čo vieme ovplyvniť.


## Inicializácia objektu

Cieľom inicilizácie objektu je nastaviť všetky jeho atribúty na správne hodnoty. Inicializácia sa vykoná vždy pri vytváraní nového objektu pomocou operátora `new`. Inicializáciu môžeme previesť rôznymi spôsobmi.

### Priama inicializácia atribútov

<div class="md-has-sidebar" markdown>
<main markdown>

Inicializovať atribúty objektu môžeme priamo pri ich deklarácii. Je to podobné, ako sa inicializujú premenné v metódach.

 </main>

  <aside markdown>
Atribúty sa inicializujú v poradí, v akom sú deklarované v kóde.
</aside>
</div>


=== "Priama inicializácia atribútov objektu"

    ```java
    public class Rat {
        private int health = 100; // priama inicializácia

        public int getHealth() {
            return health;
        } 
    }

    // použitie
    Rat potkan = new Rat();
    assert potkan.getHealth() == 100 : "Mlady potkan ma mat zdravie 100";

    ```

### Inicializačný blok

<div class="md-has-sidebar" markdown>
<main markdown>

Inicializačný blok, alebo tiež nazývaný inštančný inicializátor, je blok kódu umiestnený priamo do kódu triedy, mimo metódu. Kód v takomto inicializačnom bloku sa vykoná vždy pri vytváraní novej triedy.

 </main>

  <aside markdown>
V triede môžeme mať inicializačných blokov viac. Vykonajú sa všetky, v poradí, v akom sú v kóde napísané.
</aside>
</div>


=== "Inicializačný blok"

    ```java
    public class Rat {
        private int health;
        private double damage;

        // inštančný inicializátor
        {
            health = 100;
            damage = 0.5;
        }

        public int getHealth() {
            return health;
        } 

        public double getDamage() {
            return damage;
        } 
    }

    // použitie
    Rat potkan = new Rat();
    assert potkan.getHealth() == 100 : "Mlady potkan ma mat zdravie 100";
    assert potkan.getDamage() > 0 : "Potkan má nenulový damage";
    ```

### Konštruktor

Ako posledný krok inicializácie sa zavolá konštruktor triedy. Konštruktor je špeciálna metóda, ktorá **nemá návratový typ a má rovnaký názov ako jeho trieda**.

=== "Inicializácia pomocou konštruktora"

    ```java
    public class Rat {
        private int health;
        private double damage;

        // konštruktor
        public Rat() {
            health = 100;
            damage = 0.5;
        }

        public int getHealth() {
            return health;
        } 

        public double getDamage() {
            return damage;
        } 
    }

    // použitie
    Rat potkan = new Rat();
    assert potkan.getHealth() == 100 : "Mlady potkan ma mat zdravie 100";
    assert potkan.getDamage() > 0 : "Potkan má nenulový damage";
    ```

**Konštruktor sa zavolá vždy**. Konštruktorov môžeme mať viac, vyberie a zavolá sa ten, ktorý sa najlepšie hodí argumentom, ktoré boli uvedené pri volaní operátora `new`.

V triede nemusíme mať napísaný žiaden konštruktor. Ak v kóde triedy nie je žiaden konštruktor, Java automaticky vytvorí konštruktor bez parametrov.

### Pravidlá inicializácie

Ak nejaký z atribútov triedy neinicializujeme, Java ho inicializuje automaticky na defaultnú hodnotu `0`, `false` alebo `null`, podľa toho o aký typ atribútu ide. Inicializácia atribútu v kóde teda nie je povinná a nemusíme ju tam mať uvedenú.

Čo sa stane, ak máme v triede viacero druhov inicializácie? Pri vytváraní objektu sa postupne vykonajú všetky dostupné inicializácie v tomto poradí:

1. Ako prvá sa vykoná priama inicializácia atribútov
1. Následne sa vykonajú všetky inicializačné bloky, v poradí, v akom sú v kóde triedy napísané
1. Zavolá sa konštruktor a vykoná sa jeho kód. O tom, ktorý konštruktor sa zavolá rozhodujú argumenty, ktoré volajúci zadal pri volaní operátora `new`

## Konštruktory

<div class="md-has-sidebar" markdown>
<main markdown>

Ako sme písali, konštruktor (anglicky construcvtor) je špeciálna metóda, ktorá sa zavolá pri vytváraní objektu. Konštruktor sa dá preťažiť, teda konštruktorov môžeme mať v triede viacero. Zavolá sa vždy ten, ktorý vyhovuje argumentom zadaným pri vytváraní objektu.

Podľa typu argumentov rozdeľujeme konštruktory nasledovne:

- *defaultný konštruktor* - nemá žiadne argumenty, volá sa tiež nulárny konštruktor (nullary constructor)
- *kopírovací konštruktor* - má jeden argument, typu svojej triedy. Používa sa na kopírovanie objektu
- *parametrizovaný konštruktor* - má jeden alebo viac argumentov

**Ak v kóde triedy nemáme napísaný žiaden konštruktor, Java automaticky vygeneruje defaultný konštruktor**. 

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa programovanie v Jave. [Vysvetli mi konštruktory, uveď typy, použitie, príklady a na čo si pri písaní konštruktorov dávať pozor.](https://grok.com/share/c2hhcmQtMg%3D%3D_37d89c9b-333b-4159-8600-c8d54c8ccaec)


 </main>

  <aside markdown>
Konštruktor môže tiež zavolať iný konštruktor, pomocou špeciálnej metódy `this()`. Musí to však byť prvý príkaz v konštruktore.
</aside>
</div>


=== "Preťaženie konštruktora"

    ```java
    public class Rat {
        private int health;
        private double damage;

        // defaultný konštruktor
        public Rat() {
            health = 100;
            damage = 0.5;
        }

        // kopírovací konštruktor
        public Rat(Rat other) {
            health = other.health;
            damage = other.damage;
        }

        // parametrizovaný konštruktor
        public Rat(double damage) {
            health = 100;
            this.damage = damage;
        }

        public int getHealth() {
            return health;
        } 

        public double getDamage() {
            return damage;
        } 
    }

    // použitie
    Rat potkan1 = new Rat();
    Rat potkan2 = new Rat(2);
    Rat potkan3 = new Rat(potkan2);
    ```

### Modifikátory prístupu

<div class="md-has-sidebar" markdown>
<main markdown>

Podobne ako metódy tak aj konštruktory majú svoj modifikátor prístupu. Sú prípady, kedy volanie konštruktora nechceme povoliť hocikomu a vytvoríme private konštruktor. V takom prípade sa objekt nedá vytvárať priamo cez `new`, ale v triede musíme mať továrenskú metódu, ktorá nám vytvorí nový objekt.

=== "Privátny konštruktor a továrenske metódy"

    ```java
    public class Rat {
        private int health;
        private double damage;

        // privátny konštruktor
        private Rat(double damage) {
            health = 100;
            this.damage = damage;
        }

        // továrenské metódy
        public static Rat newRat() {
            return new Rat(0.5);
        }

        public static Rat newEliteRat() {
            return new Rat(2);
        }
    }

    // použitie
    Rat potkan1 = Rat.newRat();
    Rat potkan2 = Rat.newEliteRat();
    ```

Klasickým príkladom triedy s privátnymi konštruktormi je trieda [`java.time.LocalDate`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/LocalDate.html), ktorá nemá verejné konštruktory a pri vytváraní jej objektov musíme použiť niektorú z jej továrenských metód.

 </main>

  <aside markdown>
Privátne konštruktory sa používajú napríklad v prípadoch, ked chceme mať kontrolu nad tým, koľko objektov trieda môže mať, alebo chceme vrátiť skôr vytvorené objekty.
</aside>
</div>


## Statická inicializácia

Podobne ako môže mať trieda statické metódy, tak môže mať aj statické atribúty triedy. Používajú sa väčšinou pre definovanie konštánt, napr. trieda 
Integer má statický atribút [`Integer.MAX_VALUE`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Integer.html#MAX_VALUE)

Statické atribúty vieme inicializovať podobne ako aj klasické. Je tu však veľký rozdiel, a to ten, že statické atribúty sa inicializujú iba jeden krát a to pri prvom použití triedy. Statické atribúty vieme inicializovať dvoma spôsobmi.

### Priama inicializícia statických atribútov

Podobne ako klasické atribúty, aj statické atribúty vieme inicializovať pri ich delkarácii.

=== "Priama inicializácia statických atribútov triedy"

    ```java
    public class Rat {
        // priama inicializícia statického atribútu triedy
        public static final double NORMAL_DAMAGE = 0.5;

        private int health;
        private double damage;

        public Rat() {
            health = 100;
            this.damage = NORMAL_DAMAGE;
        }
    }
    ```

### Statický inicializačný blok

<div class="md-has-sidebar" markdown>
<main markdown>

Statický inicializačný blok, alebo tiež nazývaný statický inicializátor, je inicializátor začínajúci kľúčovým slovo `static`. Vykoná sa jeden krát pri prvom použití triedy. V statickom inicializačnom bloku nemôžeme používať atribúty objektu ani volať jeho metódy (môžeme volať iba statické)

=== "Statický inicializačný blok"

    ```java
    public class Rat {
        public static final double NORMAL_DAMAGE;
        public static final double ELITE_DAMAGE;

        private int health;
        private double damage;

        // Statický inicializačný blok
        static {
            NORMAL_DAMAGE = 0.5;
            ELITE_DAMAGE = 2;
        }

        public Rat() {
            health = 100;
            this.damage = NORMAL_DAMAGE;
        }
    }
    ```

 </main>

  <aside markdown>
Podobne ako pri inštančných inicializátoroch, aj statické inicializátory sa vykonajú všetky, v poradí, v akom sú v kóde napísané.
</aside>
</div>


## Zhrnutie teórie

- [x] Proces vytvárania objektu
    * [ ] Nový objekt vytváram pomocou operátora `new`
    * [ ] Alokácia pamäti
    * [ ] Inicializácia objektu
- [x] Inicializácia objektu, v nasledovnom poradí
    * [ ] Priama inicializácia atribútov (pri ich deklarácii)
    * [ ] Inicializačný blok (môžem ich mať viac) - inštančný inicializátor
    * [ ] Konštruktor, volá sa vždy. (Ak trieda konštruktor nemá, vytvorí sa automaticky)
    * [ ] Ak atribút nie je inicializovaný, inicializuje sa automaticky na `0`, `null` alebo `false`
- [x] Konštruktor
    * [ ] Konštruktor je špeciálna metóda, ktorá nemá návratový typ a má rovnaký názov ako jeho trieda
    * [ ] Defaultný konštruktor (nulárny konštruktor) - nemá žiadne argumenty
    * [ ] Kopírovací konštruktor - má jeden argument, typu svojej triedy
    * [ ] Parametrizovaný konštrutor - má jeden alebo viac argumentov
    * [ ] Ak v kóde triedy nemáme napísaný žiaden konštruktor, Java automaticky vygeneruje defaultný konštruktor
    * [ ] Konštruktor má modifikátor prístupu. Ak je `private`, vytvorenie objektu musí prebehnúť vnútri továrenskej metódy
- [x] Statická inicializácia triedy, v nasledovnom poradí
    * [ ] Priama inicializácia statických atribútov
    * [ ] Statický inicializačný blok (môžem ich mať viac) - statický inicializátor
    * [ ] Statická inicializácia inicializuje statické atribúty, jeden krát, pri prvom použití triedy.

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    VYTVÁRANIE OBJEKTU

    Objekt vytváram pomocou operátora new, ktorý vykoná:
    1. Alokáciu pamäte
    2. Inicializáciu objektu

    Inicializácia objektu sa vykoná v tomto poradí:
    1. Priama inicializácia atribútov
    2. Inicializačný blok - inštančný inicializátor
    3. Konštruktor, podľa toho aké argumenty má new

    Ak nie je atribút inicializovaný, Java ho inicializuje automaticky na 0, null alebo false

    Konštruktor je špeciálna metóda, ktorá nemá návratový typ a má rovnaký názov ako jeho trieda
    Defaultný konštruktor (nulárny konštruktor) - bez argumentov
    Kopírovací konštruktor - má jeden argument, typu svojej triedy
    Parametrizovaný konštrutor - má jeden alebo viac argumentov
    Ak v kóde triedy nemáme napísaný žiaden konštruktor, vytvorí sa automaticky defaultný konštruktor
    Konštruktor má modifikátor prístupu. Ak je private, musíme mať továrenskú metódu

    Statická inicializácia triedy - inicializuje statické atribúty, v tomto poradí:
    1. Priama inicializácia statických atribútov
    2. Statický inicializačný blok

    Statická inicializácia sa vykoná jeden krát, pri prvom použití triedy.
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Proces vytvárania objektu
    - Aké druhy inicializácie objektu poznáme
    - Poradie, v akom sa inicializácia vykonáva
    - Čo je konštruktor
    - Druhy konštruktorov
    - Statický inicializátor, aké druhy poznáme
    - Kedy sa vykoná inštančný inicializátor a kedy statický
