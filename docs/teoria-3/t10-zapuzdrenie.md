# Teória 10: Zapuzdrenie

Dnes začíname preberať hlavné koncepty objektovo orientovaného programovania. Začneme pri zapuzdrení.

TODO: invarianty
- interface versus implementation

A language mechanism for restricting direct access to some of the object's components.[7][8]
A language construct that facilitates the bundling of data with the methods (or other functions) operating on those data.

Information hiding   reduces software development risk by shifting the code's dependency on an uncertain implementation (design decision) onto a well-defined interface

## Zapuzdrenie

Zapuzdrenie (anglicky **encapsulation**) je jeden zo základných princípov objektovo-orientovaného programovania. Tento princíp zaisťuje, že objekty sú bezpečné a ich vnútorná implementácia je skrytá pred vonkajším svetom.

V Jave sa zapuzdrenie používa na:

- skrytie interných detailov objektu 
- ochranu jeho dát pred neoprávneným prístupom alebo modifikáciou

Zapuzdrenie umožňuje:

- Zvyšuje bezpečnosť
- Lepšia modularita 
- Jednoduchšia udržiavateľnosť kódu

Hlavné ciele zapuzdrenia:

- Ochrana dát: Zamedzenie priameho prístupu k interným premenným triedy.
- Skrytie implementácie: Vonkajší kód vidí iba rozhranie (metódy), nie vnútornú logiku.
- Flexibilita: Zmena internej implementácie bez ovplyvnenia vonkajšieho kódu.

## Zapuzdrenie atribútov

```java
public class Osoba {
    private int vek; // súkromný atribút

    // Getter
    public int getVek() {
        return vek;
    }

    // Setter
    public void setVek(int vek) {
        if (vek >= 0) { // validácia
            this.vek = vek;
        }
    }
}
```

### Statické atribúty

Statické atribúty sú prístupné cez názov triedy. Sú globálne a nepatria žiadnemu konkrétnemu objektu.

Viditeľnosť statických atribútov je daná modifikátorom prístupu, napr. `public`, `private`.

=== "Viditeľnosť statického atribútu"

    ```java
    class Example {
        // statický atribút
        static int staticVar = 20;

        void metoda() {
            System.out.println(staticVar);
        }
    }

    System.out.println(Example.staticVar);
    ```

### Inštančné atribúty

Inštančné atribúty sú prístupné cez názov objektu. Každý objekt má svoju vlastnú hodnotu týchto atribútov.

Inštančné atribúty sú viditeľné z metód priamo, alebo k nim vieme pristupovať cez premennú `this`.

Viditeľnosť inštančných atribútov mimo metód jej triedy je daná modifikátorom prístupu, napr. `public`, `private`.

=== "Viditeľnosť inštančného atribútu"

    ```java
    class Example {
        // inštančný atribút
        int instanceVar = 10;

        void metoda() {
            System.out.println(instanceVar);
            System.out.println(this.instanceVar);
        }
    }

    Example obj = new Example();
    System.out.println(obj.instanceVar);
    ```

### Parametre metód

Parametre metód sú viditeľné iba v rámci danej metódy. Po jej skončení parametre zanikajú.

=== "Viditeľnosť parametrov"

    ```java
    void metoda(int param) {
        System.out.println(param); // prístupné iba v metóde
    }
    ```

### Lokálne premenné

Lokálne premenné sú definované vnútri metód a sú viditeľné iba v rámci danej metódy alebo bloku.

=== "Viditeľnosť lokálnej premennej"

    ```java
    void metoda() {
        int localVar = 5; // lokálna premenná
        System.out.println(localVar); // prístupná iba v tejto metóde
    }
    ```

Lokálne premenné definované v rámci menšieho bloku sú viditeľné iba v tomto bloku!

=== "Lokálne premenná v rámci bloku"

    ```java
    void metoda() {
        if (true) {
            int blockVar = 10;
            System.out.println(blockVar); // OK
        }
        // System.out.println(blockVar); // Chyba, blockVar nie je viditeľná
    }
    ```

Premenná platí len v bloku, kde bola deklarovaná:

=== "Lokálna premenná v rámci cyklu"

    ```java
    for (int i = 0; i < 5; i++) {
        System.out.println(i);
    }
    System.out.println(i); // chyba, i neexistuje
    ```

### Zatienenie premenných

Ak má lokálna premenná alebo parameter rovnaké meno ako atribút, zatieni daný atribút (anglicky shadowing). Na prístup k atribútu sa používa kľúčové slovo `this` alebo názov triedy (pre statické atribúty).

=== "Príklad zatienenia inštančného atribútu"

    ```java
    class Example {
        int x = 10;

        void metoda(int x) {
            System.out.println(x); // Vypíše lokálnu premennú x
            System.out.println(this.x); // Vypíše inštančný atribút x
        }
    }
    ```

=== "Príklad zatienenia statického atribútu"

    ```java
    class Example {
        static int x = 10;

        void metoda(int x) {
            System.out.println(x); // Vypíše lokálnu premennú x
            System.out.println(Example.x); // Vypíše statický atribút x
        }
    }
    ```

Možné zatienenie v rovnakom rozsahu generuje chybu pri kompilovaní. Java nám teda nedovolí nazvať rovnako napr. parameter metódy a lokálnu premennú v tejto metóde.

### Inicializácia

Statické a inštančné atribúty sú automaticky inicializované, ak ich niekto neinicializuje vopred.

Lokálne premenné je potrebené inicializovať manuálne, Java ich neinicializuje automaticky.

Konštantné atribúty a premenné sa po inicializícii už viac nedajú meniť.

## Lokálna inferencia typov

Nakoľko je Java staticko typovaný jazyk, pri definícii premennej je potrebné uviesť jej typ. Od Java 10 je možné definovať premenú bez typu, ak kompilátor vie jednoznačne typ prideliť. Na takéto automatické pridelenie typu sa používa kľúčové slovo `var`.

=== "Lokálna inferencia typu"

    ```java
    var number = 42; // Kompilátor odvodí, že number je typu int
    var text = "Hello"; // Kompilátor odvodí, že text je typu String
    var list = new ArrayList<String>(); // Kompilátor odvodí ArrayList<String>
    ```

Kľúčové slovo `var` sa dá použiť v rôznych situáciách

=== "Rôzne spôsoby použitia `var`"

    ```java
    // V metóde
    void example() {
        var counter = 10; // int
        System.out.println(counter);
    }

    // V cykle for
    for (var i = 0; i < 5; i++) {
        System.out.println(i); // i je typu int
    }

    // V cykle for-each
    var names = List.of("Alice", "Bob");
    for (var name : names) {
        System.out.println(name); // name je typu String
    }

    // V try-with-resources
    try (var reader = new BufferedReader(new FileReader("file.txt"))) {
        System.out.println(reader.readLine()); // reader je typu BufferedReader
    }
    ```

    Pri použití `var` je dôležité, aby bola premenná inicializovaná

    ```java
    var x; // Chyba: musí byť inicializované

    var x = null; // Chyba: kompilátor nemôže určiť typ
    ```

## Názvy

Pripomeňme si konvencie pri tvorení názvov.

- Názvy tried: **PascalCase**
- Názvy metód, atribútov a premenných: **camelCase**
- Názvy konštánt: **SCREAMING_SNAKE_CASE**
- Názvy balíkov: malé písmená bez oddeľovačov, časti balíka oddelené bodkami

Názov musí začínať písmenom, podčiarníkom `_` alebo dolárom `$`.

Názov nesmie byť rezervované slovo (class, if, static...),


## Opakovanie

Prešli sme si základy jazyka Java. Od budúceho týždňa sa viac zameriame na objektovo orientované programovanie v tomto jazyku.
Čo sme si doteraz prebrali:

- Primitívne typy: `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`
- Neprimitívne typy - triedy, premenné sú odkazy - referencie
- Premenné a konštanty, príkazy a výrazy
- Operátory: Aritmetické, relačné, priradzovacie, bitové, logické
- Štandardný vstup a výstup: `System.out`, `System.in`
- Parsovanie textu: `java.util.Scanner`
- Formátovaný text: `printf`, `String.format`, `%d`, `%f`, `%s`
- Cykly: `for`, `for-each`, `while`, `do-while`
- Podmienky: `if-else`, `switch`, `?:`
- Polia: fixná veľkosť, homogénne prvky, `java.util.Arrays`, viacrozmerné polia
- Reťazce: `String`, `StringBuilder`, operácie s reťazcami, `toString`
- Výminky a správa chýb: `try-catch`, `try-with-resources`
- Obalené typy, literály, `null`, typová konverzia
- Triedy: samostatný `.java` súbor, obsahujú metódy, atribúty a konštruktory
- Balíky: organizácia tried, reverzná doména
- Objekt: inštancia triedy, operátor `new`, konštruktory a továrenske metódy
- Objekt: identita a hodnota, `==`, `equals`
- Metódy: inštančné a statické, majú modifikátor prístupu
- Metódy: preťaženie metód, varargs
- Metódy podľa použitia: továrenske, getter a setter metódy
- Konštruktor: defaultný, kopírovací, parametrický
- Atribúty: inštančné a statické, majú modifikátor prístupu
- Inicializácia: inicializačné bloky, konštruktory, priama inicializácia


## Zhrnutie teórie

- [x] Viditeľnosť (visibility) a rozsah premenných (scope)
    * [ ] Statické a inštančné atribúty majú modifikátor prístupu
    * [ ] K statickým atribútom pristupujem cez názov triedy
    * [ ] K inštančným atribútom pristupujem cez názov objektu
    * [ ] Vnútri triedy viem pristupovať k atribútom cez ich názov alebo pomocou `this` 
- [x] Parametre metód a lokálne premenné
    * [ ] Sú viditeľné iba v rámci metódy resp. bloku, v ktorom boli definované
- [x] Zatienenie atribútov (shadowing)
    * [ ] Ak má parameter alebo lokálna premenná rovnaký názov ako atribút
    * [ ] Na prístup k zatienenému atribútu použijeme `this` alebo názov triedy (pri statických atribútoch)
- [x] Inicializácia
    * [ ] Atribúty sa inicializujú automaticky, ak sa to neurobí manuálne
    * [ ] Lokálne premenné musíme inicializovať sami
- [x] Lokálna inferencia typov
    * [ ] Kľúčové slovo `var` pri definícii lokálnych premenných
    * [ ] Nesmieme inicializovať s `null`
- [x] Názvy
    * [ ] Názov musí začínať písmenom, podčiarníkom `_` alebo dolárom `$`
    * [ ] Názov nesmie byť rezervované slovo (`class`, `if`, `static`...)

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    PREMENNÉ

    Premenná má svoju  viditeľnosť (visibility) a rozsah (scope)
    - Atribúty majú modifikátor prístupu
    - K atribútom pristupujem cez názov triedy (statické) alebo objektu (inštančné)
    - Vnútri triedy viem pristupovať k atribútom cez ich názov alebo pomocou this

    Parametre metód a lokálne premenné
    - Viditeľné iba v rámci metódy resp. bloku, v ktorom boli definované

    Zatienenie atribútov (shadowing)
    - Ak má parameter alebo lokálna premenná rovnaký názov ako atribút
    - Na prístup k zatienenému atribútu použijeme this alebo názov triedy (pri statických atribútoch)

    Inicializácia
    - Atribúty sa inicializujú automaticky, ak sa to neurobí manuálne
    - Lokálne premenné musíme inicializovať sami

    Lokálna inferencia typov
    - Kľúčové slovo var pri definícii lokálnych premenných
    - Nesmieme inicializovať s null

    Názvy
    - Názov musí začínať písmenom, podčiarníkom _ alebo dolárom $
    - Názov nesmie byť rezervované slovo (class, if, static...)
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Viditeľnosť a rozsah (scope) premenných
    - Zatienenie atribútu - shadowing
    - Lokálna inferencia typu pomocou `var`
    - Možnosti pri názvoch premenných
