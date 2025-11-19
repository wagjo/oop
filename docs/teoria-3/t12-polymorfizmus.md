# Teória 12: Polymorfizmus

Tretí princíp objektovo orientovaného programovania sa volá Polymorfizmus. 

Polymorfizmus znamená „mnohotvárnosť“ — schopnosť, aby metóda mohla mať rôzne správanie v závislosti od triedy, teda typu objektu, ktorý bol pri volaní metódy použitý.

V Jave to znamená, že referencia typu rodiča môže ukazovať na objekt potomka a pri volaní metódy sa použije verzia metódy z reálneho typu objektu, nie z typu premennej.


## Polymorfizmus

Vo všeobecnosti existujú 3 druhy polymorfizmu. 

1. **Statický polymorfizmus**, tiež nazývaný ad-hoc polymorfizmus. Deje sa počas kompilácie a realizuje sa cez preťažovanie metód (method overloading)

1. **Dynamický polymorfizmus**, tiež nazývaný subtype polymorfizmus. Ten sa deje počas behu programu (runtime) a realizuje sa cez prekrytie metód (method overriding)

1. **Parametrický polymorfizmus**, tiež nazývaný generický polymorfizmus. Deje sa počas kompilácie a realizuje sa cez generické triedy.


## Statický polymorfizmus

<div class="md-has-sidebar" markdown>
<main markdown>


Statický, alebo **ad-hoc polymorfizmus** sa deje počas kompilácie. Realizuje sa cez **preťažovanie metód** - anglicky method overloading.

Pri tomto type polymorfizmu máme viacero metód s rovnakým názvom, a kompilátor rozhodne, ktorá metóda sa zavolá. Ak neexistuje metóda s daným typom argumentu, Java vyberie metódu s argumentom rodičovského typu, ak taký existuje.

To, ktorá verzia metódy sa použije vieme ovplyvniť explicitným pretypovaním parametrov pri volaní metódy.

 </main>

  <aside markdown>
V niektorých programovacích jazykoch, ako napr. Python, je možné preťažiť aj niektoré operátory, napríklad aritmetické operátory ako `+`, `-` a `*`. Java však satický polymorfizmus pre operátory nepodporuje.</aside>
</div>


=== "Statický polymorfizmus"

    ```java
    public class StaticPoly {
        public static void foo(String x) {
            System.out.println("String foo");
        }

        public static void foo(Object x) {
            System.out.println("Object foo");
        }

        public static void foo(int x) {
            System.out.println("int foo");
        }

        public static void foo(Number x) {
            System.out.println("Number foo");
        }

        public static void main(String[] args) {
            foo("Test");         // String
            foo(true);           // Object, lebo verziu s Boolean nemáme
            foo(4);              // int
            foo(4.0);            // Number, lebo verziu s Double nemáme
            foo((int)4.0);       // int, lebo sme explicitne pretypovali
            foo((Object)"Test"); // Object, lebo sme explicitne pretypovali
        }
    }

    ```

Rozhodovanie o tom, ktorá metóda sa zavolá, sa deje výlučne na základe kódu. Na skutočný typ objektu sa neprihliada, pretože rozhodovanie sa deje počas kompilácie.

=== "Statický polymorfizmus pozerá na kód, nie na typy objektov v pamäti"

    ```java
    public class StaticPoly {
        public static void foo(String x) {}

        public static void foo(Object x) {}

        public static void main(String[] args) {
            // staticky polymorfizmus pozerá na kód,
            // nie na reálne hodnoty v premenných
            String s = "Test";
            Object o = s;
            foo(o); // Object!
        }
    }
    ```

Niekedy môže nastať situácia, že vhodných verzií metód je viacero a kompilátor nevie jednoznačne rozhodnúť, ktorú verziu vybrať. V takom prípade sa program neskompiluje a kompilátor skončí s chybou, že volanie metódy je nejednoznačné (anglicky ambiguous). 

Takéto situácie môžu nastať pri metódach s viacerými argumentami, alebo v prípade, že existujú iba viaceré verzie s rodičovskými typmi argumentov. Riešením tohto problému je explicitné (manuálne) pretypovanie argumentov tak, aby kompilátor vedel vybrať metódu, ktorá sa použije.

=== "Nejednoznačné volanie preťažených metód"

    ```java
    public class StaticPoly {
        public static void foo(String x) {}
        public static void foo(Number x) {}

        public static void foo(int x, long y) {}
        public static void foo(long x, int y) {}

        public static void main(String[] args) {
            //foo(null);       // ERROR, vyhovuje aj String aj Number
            foo((String)null); // String
            
            // foo(1, 2);      // ERROR, vyhovujú obe verzie s dvoma argumentami
            foo(1, 2L);        // int, long
            foo(1, (long) 2);  // int, long
        }
    }
    ```

## Dynamický polymorfizmus

Dynamický, alebo **subtypový polymorfizmus** sa deje počas behu programu (anglicky runtime). Realizuje sa cez **prekrývanie metód** (method overriding).

Pri tomto type polymorfizmu sa využíva dedičnosť. Máme triedy rodičovské (superclass) a triedy potomkov (subclass), ktoré prekrývajú metódy svojich rodičov.

V kóde síce vždy máme pri premennej uvedený aj jej typ, ale pri volaní polymorfickej metódy sa na typ uvedený v kóde neprihliada. Java sa počas behu programu pozrie na objekt a zavolá metódu, ktorá zodpovedá skutočnému typu objektu, na ktorým sa volá metóda.

=== "Dynamický polymorfizmus"

    ```java
    class ParentPoly {
        public void foo() {
            System.out.println("Parent foo");
        }
    }

    class ChildPoly extends ParentPoly {
        @Override
        public void foo() {
            System.out.println("Child foo");
        }
    }

    public class DynamicPoly {
        public static void zavolajFoo(ParentPoly p) {
            // nemusí sa vždy volať metóda ParentPoly.foo(),
            // keďže ju potomkovia môžu prekryť
            p.foo();
        }

        public static void main(String[] args) {
            ParentPoly x = new ChildPoly();
            zavolajFoo(x); // Child foo
        }
    }
    ```

<div class="md-has-sidebar" markdown>
<main markdown>

Pokiaľ pri statickom polymorfizme záležalo na type argumentov pri volaní metódy, pri dynamickom polymorfizme záleží iba na type objektu, nad ktorým sa volá daná metóda. (Statické metódy nepodporujú prekrytie metód a teda ani dynamický polymorfizmus).

Dynamický polymorfizmus je mierne pomalší ako iné typy polymorfizmu alebo normálne volanie metód, nakoľko JVM musí použiť tzv. tabuľku virtuálnych metód (VMT, vtable, virtual method table). Pomocou tejto tabuľky vie počas behu programu rozhodnúť, ktorú verziu metódy zavolať.

!!! example "Príklad použitia dynamického polymorfizmu"

    ```java
    public class Animal {
        public void makeSound() {
            System.out.println("Živočích vydáva zvuk");
        }
    }

    public class Dog extends Animal {
        @Override
        public void makeSound() {
            System.out.println("Pes šteká: Haf!");
        }
    }

    public class Cat extends Animal {
        @Override
        public void makeSound() {
            System.out.println("Mačka mňauká: Miau!");
        }
    }

    public class Main {
        public static void main(String[] args) {
            Animal myPet;  // Referencia typu Animal

            myPet = new Dog();  // Ale objekt je typu Dog
            myPet.makeSound();  // Výstup: Pes šteká: Haf! (runtime rozhodnutie)

            myPet = new Cat();
            myPet.makeSound();  // Výstup: Mačka mňauká: Miau!
        }
    }
    ```

 </main>

  <aside markdown>
Takýto typ dynamického polymorfizmu sa volá *single dispatch*, nakoľko sa pozerá iba na typ jedného objektu. Niektoré programovacie jazyky umožňujú dynamický polymorfizmus aj nad kombináciou viacerých objektov, tzv. *multiple dispatch*. Java však túto funkcionalitu neposkytuje.
</aside>
</div>


## Parametrický polymorfizmus

Parametrický, alebo **generický polymorfizmus** sa deje počas kompilácie, podobne ako statický. Realizuje sa cez generické triedy a jeho cieľom je zvýšiť bezpečnosť kontroly typov pri kompilácii.

Parametrický polymorfizmus sa používa v prípadoch, kedy klasický typ by bol príliš obmedzujúci a znemožňoval by znovupoužitie triedy.

=== "Príliš obmedzujúci typ"

    Problém: príliš obmedzujúci HAS-A typ znemožňujúci znovupoužitie

    Ak by sme chceli prvky iného typu, museli by sme vytvoriť novú triedu

    ```java
    public class Pair {
        String x;
        String y;

        Pair(String x, String y) {
            this.x = x;
            this.y = y;
        }

        public String getX() {
            return x;
        }

        public String getY() {
            return y;
        }

        public static void main(String[] args) {
            Pair p = new Pair("Hello", "World");
            // Pair p = new Pair(3, 4); // ERROR
            String x = p.getX();
        }
    }
    ```

=== "Príliš všeobecný typ"

    Použitie triedy `Object` je príliš všeobecné a prináša slabú typovú kontrola pri HAS-A vzťahoch

    Getter metódy nevedia, akého typu sú prvky. Musíme použiť explicitné pretypovanie.

    ```java
    public class Pair {
        Object x;
        Object y;

        Pair(Object x, Object y) {
            this.x = x;
            this.y = y;
        }

        public Object getX() {
            return x;
        }

        public Object getY() {
            return y;
        }

        public static void main(String[] args) {
            Pair p = new Pair("Hello", "World");
            //String x = p.getX(); // ERROR
            String x = (String)p.getX();
        }
    }
    ```

Sú teda situácie, kedy v triede pri HAS-A vzťahoch (atribúty triedy) je použitie konkrétnej triedy príliš obmedzujúce a použitie typu `Object` by malo za následok slabú typovú kontrolu. V takýchto prípadoch je riešením generická trieda.

Pomocou generickej triedy sa typy HAS-A vzťahov tzv. parametrizujú, a každý objekt generickej triedy má svoj vlastný typ atribútov. **Parametrizovaný typ sa pri definícii triedy uvedie do lomených zátvoriek** a pri vytváraní objektov a premenných sa potom do lomených zátvoriek uvedie, aký konkrétny typ sa použije pre daný objekt alebo premennú.

=== "Príklad použitia generickej triedy"

    Namiesto konkrétneho typu, napr. `String` sa použije typová premenná `T`

    ```java
    public class Pair<T> {
        T x;
        T y;

        Pair(T x, T y) {
            this.x = x;
            this.y = y;
        }

        public T getX() {
            return x;
        }

        public T getY() {
            return y;
        }

        public static void main(String[] args) {
            Pair<String> p1 = new Pair<>("Hello", "World");
            String s = p1.getX();

            Pair<Integer> p2 = new Pair<>(3, 4);
            int i = p2.getX();
        }
    }
    ```

=== "Generická trieda s viacerými parametrizovanými typmi"

    Ak máme viac parametrizovaných typov, oddelíme ich čiarkami

    ```java
    public class Pair<T, U> {
        T x;
        U y;

        Pair(T x, U y) {
            this.x = x;
            this.y = y;
        }

        public T getX() {
            return x;
        }

        public U getY() {
            return y;
        }

        public static void main(String[] args) {
            Pair<String, Integer> p1 = new Pair<>("Hello", 3);
            
            String s = p1.getX();
            int i = p2.getY();
        }
    }
    ```

<div class="md-has-sidebar" markdown>
<main markdown>

Generické typy sa v Jave používajú hlavne pri kolekciách, ako napríklad zoznamy, množiny, zásobníky, a pod. 

Parametrizované typy v Jave existujú iba pri kompilácii, počas behu programu sa parametrizovaný typ nahradí typom `Object`. Toto nahradenie sa odborne volá **type erasure**, teda vymazanie typu. Počas behu programu teda nevieme zistiť konkrétnu triedu parametrizovaného typu.

!!! info "Pokročilé techniky: Generické metódy"
    
    Okrem generických typov Java podporuje tiež generické metódy. Va takýchto prípadoch sa neparametrizuju typy v celej triede, ale iba argumenty a návratový typ v rámci jednej metódy.

    ```java title="Príklad generickej metódy na výmenu dvoch prvokov v poli akéhokoľvek typu"
    public static <T> void swap(T[] array, int i, int j) {
        T temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    ```

 </main>

  <aside markdown>
Parametrizované typy sa dajú ohraničiť, ak chceme obmedziť možné konktétne typy iba na určitú podmnožinu typov. Vymazanie typov by potom prebiehalo trochu ináč. Táto pokročilá technika však je nad rámec dnešnej lekcie.
</aside>
</div>

## Zhrnutie teórie

- [x] Polymorfizmus - mnohootvárnosť
    * [ ] Metóda môže mať rôzne správanie v závislosti od triedy, teda typu objektu, ktorý bol pri volaní metódy použitý
    * [ ] Statický polymorfizmus - cez preťažovanie metód
    * [ ] Dynamický polymorfizmus - cez prekrytie metód
    * [ ] Parametrický polymorfizmus - cez generické triedy
- [x] Statický polymorfizmus - ad-hoc polymorfizmus
    * [ ] Deje sa počas kompilácie
    * [ ] Realizuje sa cez preťažovanie metód - method overloading
    * [ ] Máme viacero metód s rovnakým názvom, a kompilátor rozhodne, ktorá metóda sa zavolá
    * [ ] Ak žiadna verzia metódy nevyhovuje, hľadá sa verzia s argumentom rodičovského typu
    * [ ] Výber vieme ovplyvniť explicitným pretypovaním parametrov pri volaní metódy
    * [ ] Rozhodovanie o tom, ktorá metóda sa zavolá, sa deje výlučne na základe kódu. Na skutočný typ objektu sa neprihliada, pretože rozhodovanie sa deje počas kompilácie
    * [ ] Ak je vhodných verzií metód viacero, a nevieme určiť špecifickejšiu, program sa neskompiluje a chybou, že volanie metódy je nejednoznačné (anglicky ambiguous)
- [x] Dynamický polymorfizmus - subtypový polymorfizmus
    * [ ] Deje sa počas behu programu - runtime
    * [ ] Realizuje sa cez prekrývanie metód - method overriding
    * [ ] V kóde síce vždy máme pri premennej uvedený aj jej typ, ale pri volaní polymorfickej metódy sa na typ uvedený v kóde neprihliada
    * [ ] Počas behu programu sa zavolá metóda, ktorá zodpovedá skutočnému typu objektu, na ktorým sa volá metóda
    * [ ] Záleží iba na type objektu, nad ktorým sa volá daná metóda, nie na argumentoch
    * [ ] Statické metódy nepodporujú prekrytie metód a teda ani dynamický polymorfizmus
    * [ ] Dynamický polymorfizmus je mierne pomalší ako iné typy polymorfizmu alebo normálne volanie metód, kvôli tabuľke virtuálnych metód
- [x] Parametrický polymorfizmus - generický polymorfizmus
    * [ ] Deje sa počas kompilácie
    * [ ] Realizuje sa cez generické triedy, ktorý parametrizujú typy atribútov
    * [ ] Používa v prípadoch, kedy klasický typ by bol príliš obmedzujúci a znemožňoval by znovupoužitie triedy
    * [ ] Sú situácie, kedy v triede pri HAS-A vzťahoch (atribúty triedy) je použitie konkrétnej triedy príliš obmedzujúce a použitie typu `Object` by malo za následok slabú typovú kontrolu
    * [ ] Parametrizovaný typ sa pri definícii triedy uvedie do lomených zátvoriek a pri vytváraní objektov a premenných sa potom do lomených zátvoriek uvedie, aký konkrétny typ sa použije pre daný objekt alebo premennú
    * [ ] Generické typy sa v Jave používajú hlavne pri kolekciách, ako napríklad zoznamy, množiny, zásobníky, a pod
    * [ ] Parametrizované typy v Jave existujú iba pri kompilácii, počas behu programu sa parametrizovaný typ nahradí typom `Object`. Toto nahradenie sa odborne volá type erasure, teda vymazanie typu



!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    POLYMORFIZMUS - mnohotvárnosť
    
    Metóda môže mať rôzne správanie v závislosti od triedy, teda typu objektu, ktorý bol pri volaní metódy použitý


    1. Statický polymorfizmus - ad-hoc polymorfizmus

    Deje sa počas kompilácie 
    Realizuje sa cez preťažovanie metód - method overloading
    Viacero metód s rovnakým názvom - kompilátor rozhodne, ktorá sa zavolá
    Výber vieme ovplyvniť explicitným pretypovaním pri volaní
    Rozhodovanie sa deje na základe kódu. Na skutočný typ objektu sa neprihliada
    Ak je rovnako vhodných metód viacero, kompilátor skončí chybou, že volanie je nejednoznačné (anglicky ambiguous)


    2. Dynamický polymorfizmus - subtypový polymorfizmus

    Deje sa počas behu programu - runtime
    Realizuje sa cez prekrývanie metód - method overriding
    Pri volaní polymorfickej metódy sa na typ uvedený v kóde neprihliada
    Počas behu programu sa zavolá metóda, ktorá zodpovedá skutočnému typu objektu
    Záleží iba na type objektu, nad ktorým sa volá daná metóda, nie na argumentoch
    Statické metódy nepodporujú prekrytie metód ani dynamický polymorfizmus
    Je mierne pomalší kvôli tabuľke virtuálnych metód


    3. Parametrický polymorfizmus - generický polymorfizmus

    Deje sa počas kompilácie
    Realizuje sa cez generické triedy, ktorý parametrizujú typy atribútov
    Používa sa keď klasický typ je príliš obmedzujúci a znemožňoval by znovupoužitie triedy
    Použitie typu Object by ale malo za následok slabú typovú kontrolu - radšej generickú triedu
    Parametrizovaný typ sa pri definícii triedy uvedie do lomených zátvoriek
    Pri vytváraní objektov sa do lomených zátvoriek uvedie, aký konkrétny typ sa použije
    Používa sa hlavne pri kolekciách, (zoznamy, množiny, zásobníky, a pod)
    Parametrizované typy v Jave existujú iba pri kompilácii, počas behu programu sa použije typ Object. 
    Toto nahradenie sa odborne volá type erasure, teda vymazanie typu
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Polymorfizmus - čo to je
    - Statický polymorfizmus, kedy sa deje a cez čo sa realizuje
    - Kedy kompilátor vyhodí pri statickom polymorfizme chybu
    - Dynamický polymorfizmus, kedy sa deje a cez čo sa realizuje
    - Prečo je pomalší
    - Parametrický polymorfizmus, kedy sa deje a cez čo sa realizuje
    - Čo je type erasure
