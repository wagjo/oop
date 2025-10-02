# Teória 6: Operátory

Operátory v Jave sú špeciálne symboly alebo kľúčové slová, ktoré vykonávajú operácie nad premennými alebo hodnotami. Dnes sa bližšie pozrieme na niektoré z nich.

## Aritmetické operátory

Medzi aritmetické operátory patria sčítanie `+`, odčítanie `-`, násobenie `*`, delenie `/` a modulo `%`. 

<div class="md-has-sidebar" markdown>
<main markdown>
Dôležitou vlastnosťou aritmetických operátorov je, že ak sú oba operandy celé čísla, výsledok je tiež celé číslo a zaokrúhli sa smerom nadol.

=== "Celočíselné delenie"

    ```java
    System.out.println(7 / 2); // 3
    System.out.println(-7 / 2); // -3
    ```

 </main>
  <aside markdown>
Ak chceme číslo zaokrúhliť klasicky, môžeme použiť metódu `Math.round()`</div>


Ak chceme vo výsledku desatinné číslo, musí byť aspoň jeden z operandov tiež celé číslo.

=== "Delenie z desatinným čislom"
    
    ```java
    System.out.println(7.0 / 2); // 3.5
    ```

### Operátor modulo, `%`

<div class="md-has-sidebar" markdown>
<main markdown>
Operátor modulo `%` vracia zvyšok po celočíselnom delení. Napríklad 17 / 5 = 3 a zvyšok je 2, teda `17 % 5 == 2`. Argumenty modulu môžu byť aj desatinné čisla, v takom prípade bude desatinný aj zvyšok. Napríklad `5.5 % 2 == 1.5`. Oproti matematickému zvyšku, ktorý je vždy kladný, má Java modulo vo výsledku znamienko deleného čísla.
 </main>
  <aside markdown>
Na výpočet matematckého modulo vieme použiť metódu `Math.floorMod`
</div>


=== "modulo nie je matematický zvyšok"

    ```java
    System.out.println( 17 % 5 );   //  2
    System.out.println(-17 % 5 );   // -2
    System.out.println( 17 % -5 );  //  2
    System.out.println(-17 % -5 );  // -2
    ```

!!! tip "Učím sa s pomocou unelej inteligencie"

    V Jave modulo operátor nevracia matematický modulo. [Ako by som ho v Jave vedel vypočítať?](https://grok.com/share/c2hhcmQtMg%3D%3D_368dba13-2562-45d2-92fc-b90bda236569)

## Operátory priradenia

<div class="md-has-sidebar" markdown>
<main markdown>
Operátor priradenia v Java `=` kopíruje primitívnu hodnotu, alebo kopíruje referenciu (odkaz) na objekt. Nekopíruje objekt samotný!

Okrem tohto operátora existujú ešte skrátene prirazdovacie operátory, ktoré kombinujú aritmetické operácie s priradením: `+=`, `-=`, `*=`, `/=`, `%=`.
 </main>
  <aside markdown>
Skopírovanie objektu sa väčšinou robí pomocou metódy `clone()` alebo pomocou kopírovacieho konštruktora.
</div>

=== "Operátory priradenia"

    ```java
    int x = 10;
    x += 5;   // x = x + 5  -> 15
    x -= 3;   // x = x - 3  -> 12
    x *= 2;   // x = x * 2  -> 24
    x /= 4;   // x = x / 4  -> 6
    x %= 5;   // x = x % 5  -> 1
    ```

### Inkrement a dekrement

Špeciálnym typom operátorov priradenia sú inkrementačné a dekrementačné operátory. Zmenia hodnotu o 1, ale vieme v nich určiť, či výsledok výrazu má byť stará alebo nová hodnota.

| Názov                                           | Príklad | Výsledok / význam                                    |
| ----------------------------------------------- | ------- | ---------------------------------------------------- |
| Prefix inkrement (zväčší o 1, použije výsledok hneď)   | `++x`   | najprv zvýši `x`, potom vráti novú hodnotu           |
| Postfix inkrement (zväčší o 1, použije pôvodnú hodnotu) | `x++`   | najprv vráti starú hodnotu `x`, potom ju zvýši       |
| Prefix dekrement (zmenší o 1, použije výsledok hneď)   | `--x`   | najprv zníži `x`, potom vráti novú hodnotu           |
| Postfix dekrement (zmenší o 1, použije pôvodnú hodnotu) | `x--`   | najprv vráti starú hodnotu `x`, potom ju zníži       |


```java
int x = 5;
int y = x++;   // y == 5, x == 6
int z = ++x;   // z == 7, x == 7
```

!!! tip "Učím sa s pomocou unelej inteligencie"

    Som študent strednej školy, učím sa Javu. Vysvetli a ukáž na príkladoch [nástrahy operátorov inkrementu a dekrementu](https://grok.com/share/c2hhcmQtMg%3D%3D_7b395065-9d02-4b49-a8b2-5188a5585316).


## Relačné operátory

<div class="md-has-sidebar" markdown>
<main markdown>
Relačné operátory porovnávajú 2 hodnoty a vrátia `true` alebo `false`. Operátor rovnosti `==` porovnáva hodnoty, ak ide o primitívne typy, pri objektoch však porovnáva identitu objektov. 

 </main>
  <aside markdown>
  Na porovnanie hodnôt objektov sa používa metóda `equals()`.
</div>

```java
int a = 5, b = 8;

a == b   // false (rovnosť)
a != b   // true  (nerovnosť)
a > b    // false
a < b    // true
a >= b   // false
a <= b   // true
```

## Bitové operátory

<div class="md-has-sidebar" markdown>
<main markdown>
Bitové operátory pracujú na úrovni jednotlivých bitov. Keďže v Jave nemáme možnosť priamo pracovať s bitmi, vstupy a výstupy týchto operátorov sú vždy čísla. Je vhodné to chápať tak, že sa zoberie dané číslo, preverie sa na pole bitov, vykoná sa daná operácia a potom sa naspať pole bitov prevedie na číslo, ktoré sa vráti ako výsledok.
 </main>
  <aside markdown>
  Na vypísanie bitov vo forme reťazca vieme použiť metódu `Integer.toBinaryString()`
</div>

```java
int x = 5;   // 0101
int y = 3;   // 0011

x & y   // 0001 → 1 (bitový AND)
x | y   // 0111 → 7 (bitový OR)
x ^ y   // 0110 → 6 (bitový XOR)
~x      // invertovanie bitov → ...11111010
x << 1  // posun doľava → 1010 (10)
x >> 1  // posun doprava → 0010 (2)
x >>> 1 // logický posun doprava (dopĺňa nulami)
```

!!! tip "Učím sa s pomocou unelej inteligencie"

    Som študent strednej školy, učím sa Javu. Java nemá bitový typ ale má bitové operátory. Vysvetli mi [ako sa má v Jave robiť manipulácia s bitmi a na čo si dať pozor](https://grok.com/share/c2hhcmQtMg%3D%3D_2b07f528-d6a6-4399-be71-4168c9eb2430).

## Logické operátory

Logické operátory pracujú s boolean hodnotami. Ich výsledok je tiež boolean hodnota. 

- Logický AND - `&&`
- Logický OR - `||`
- Logický NOT - `!`

Ich hlavné využitie je pri zložených výrazoch v podmienkach. Začiatočníci si často pletú bitové a logické operátory, je však medzi nimi veľký rozdiel. Logické operátory majú tzv. **short-circuit**, ktorý predčasne ukončí výpočet, ak už vie, či výsledok určite bude `true` alebo `false`. Napríklad vo výraze `false && foo()` sa metóda `foo` ani nezavolá, pretože prvý operand je už `false`. Podobne aj pri logickom súčte OR môže nastať short-circuit ak prvý operand je `true`. (`true || foo()`)

!!! example "Príklad: Použitie short-circuit na kontrolu pred prístupom k pvrku poľa"

    ```java
    int[] pole = {1, 2, 3};
    int i = 2;

    if (i < pole.length && pole[i] == 3) {
        System.out.println("Na indexe " + i + " je číslo 3");
    }
    ```

    Ak by sme nepoužili short-circuit `&&`, druhá podmienka (`pole[i] == 3`) by sa vyhodnocovala aj keď `i` je mimo rozsah

!!! example "Príklad: Kontrola pred volaním metódy na `null` objekte"

    ```java
    String s = null;

    if (s != null && s.length() > 0) {
        System.out.println("Reťazec nie je prázdny");
    }
    ```

    Ak by sme nepoužili short-circuit `&&`, zavolalo by sa `s.length()` na `null` a vyhodila by sa NPE výnimka

!!! example "Príklad: Predčasné ukončenie podmienky OR"

    ```java
    boolean isAdmin = true;
    String uzivatel = "Fero";

    if (isAdmin || maUzivatelPristup(uzivatel)) {
        System.out.println("Vstup povolený");
    }
    ```

    Vďaka short-circuit `||` sa podmienka `maUzivatelPristup` vôbec neskontroluje, pretože `isAdmin` je už `true`. To šetrí výkon alebo zabráni zbytočnému volaniu metód.


## Operátor `instanceof`

Pomocou operátora `instanceof` vieme zistiť, či daný objekt je inštancia určitej triedy alebo implementuje konkrétne rozhranie (o rozhraniach niekedy nabudúce). Výsledok operácie je `boolean`, teda `true` alebo `false`.

=== "Sytax operátora `instanceof`"

    ```java
    objekt instanceof Typ
    ```

Dedičnosť sme si ešte nevysvetľovali, ale zatiaľ si povieme, že objekt akejkoľvek triedy v Jave je zároveň aj inštanciou triedy `Object`. Túto triedu vieme použiť ako typ, ak chceme aby premenná alebo argument metódy mohli mať akýkoľvek objekt.

!!! example "Príklad: Použitie operátora `instanceof`"

    Ako príklad použitia si ukážeme metódu, ktorá môže mať na vstupe akýkoľvek objekt a v tele bude zisťovať, či nie je nejakého konkrétneho typu.

    ```java
    public static void otestuj(Object o) {
        if(o instancof String)
            System.out.println("Je to String");
        if(o instancof Double)
            System.out.println("Je to čislo s desatinnou čiarkou");
        if(o instancof Number)
            System.out.println("Je to číslo");
    }
    
    public static void main(String[] args) {
        otestuj("Ahoj");
        otestuj(2);
        otestuj(2.2);
    }
    ```

## Poradie vykonávania operátorov

Ak máme vo výraze viacero operátorov, vykonávajú sa väčšinou zľava do prava. Operátory však majú rôznu prioritu a tie s vyššou prioritou sa vykonajú skôr. Poznáme to z matematiky, kedy násobenie sa vykoná pred súčtom alebo rozdielom.

Tak ako aj v matematike aj v Jave môžeme použiť zátvorky, ak chceme zmeniť poradie vykonania operácií. Zátvorky majú najvyššiu prioritu. Ďalšie operátory s vysokou prioritou sú inkrementy a dekrementy, násobenie a delenie. Na druhej strane operátory s najnižšou prioritou, ktoré sa vykonajú ako posledné, sú operátory priradenia a logické operátory.

!!! abstract "Dokumentácia"

    Pozrite si [prehľad priorít operátorov - Operator Precedence](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html) vo forme tabuľky.


## Zhrnutie teórie

- [x] Aritmetické operátory
    * [ ] sčítanie `+`, odčítanie `-`, násobenie `*`, delenie `/` a modulo `%`
    * [ ] Ak sú oba operandy celé čísla, výsledok je tiež celé číslo a zaokrúhli sa smerom nadol
    * [ ] Ak chceme vo výsledku desatinné číslo, musí byť aspoň jeden z operandov tiež celé číslo
    * [ ] Ak chceme číslo zaokrúhliť klasicky, môžeme použiť metódu `Math.round()`
    * [ ] Operátor modulo `%` vracia zvyšok po celočíselnom delení
    * [ ] Oproti matematickému zvyšku, ktorý je vždy kladný, má Java modulo vo výsledku znamienko deleného čísla. 
    * [ ] Na výpočet matematckého modulo vieme použiť metódu `Math.floorMod`
- [x] Operátory priradenia
    * [ ] Operátor priradenia v Java `=` kopíruje primitívnu hodnotu, alebo kopíruje referenciu (odkaz) na objekt. Nekopíruje objekt samotný!
    * [ ] Skopírovanie objektu sa väčšinou robí pomocou metódy clone() alebo pomocou kopírovacieho konštruktora
    * [ ] Skrátene prirazdovacie operátory kombinujú aritmetické operácie s priradením: `+=`, `-=`, `*=`, `/=`, `%=`. 
    * [ ] Inkrementačné a dekrementačné operátory zmenia hodnotu o 1, ale vieme v nich určiť, či výsledok výrazu má byť stará alebo nová hodnota
    * [ ] `++x` najprv zvýši `x`, potom vráti novú hodnotu
    * [ ] `x++` najprv vráti starú hodnotu `x`, potom ju zvýši
- [x] Relačné operátory
    * [ ] Relačné operátory porovnávajú 2 hodnoty a vrátia `true` alebo `false`
    * [ ] Operátor rovnosti `==` porovnáva hodnoty, ak ide o primitívne typy, pri objektoch však porovnáva identitu objektov
    * [ ] Na porovnanie hodnôt objektov sa používa metóda `equals()`
    * [ ] `==`, `!=`, `>`, `<`, `>=`, `<=`
- [x] Bitové operátory
    * [ ] Bitové operátory pracujú na úrovni jednotlivých bitov. 
    * [ ] Keďže v Jave nemáme možnosť priamo pracovať s bitmi, vstupy a výstupy týchto operátorov sú vždy čísla
    * [ ] Na vypísanie bitov vo forme reťazca vieme použiť metódu Integer.toBinaryString()§
    * [ ] `&` (AND), `|` (OR), `^` (XOR), `~` (NOT), `>>` (RIGHT SHIFT), `<<` (LEFT SHIFT), `>>>` (UNSIGNED RIGHT SHIFT)
- [x] Logické operátory
    * [ ] Logické operátory pracujú s boolean hodnotami. Ich výsledok je tiež `boolean` hodnota. 
    * [ ] `&&` (AND), `||` (OR), `!` (NOT)
    * [ ] Ich hlavné využitie je pri zložených výrazoch v podmienkach
    * [ ] Logické operátory majú tzv. **short-circuit**, ktorý predčasne ukončí výpočet, ak už vie, či výsledok určite bude `true` alebo `false`
- [x] Operátor `instanceof`
    * [ ] Pomocou operátora `instanceof` vieme zistiť, či daný objekt je inštancia určitej triedy alebo implementuje konkrétne rozhranie
    * [ ] Výsledok operácie je boolean, teda `true` alebo `false`.


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    OPERÁTORY

    Aritmetické
    +, -, *, /, %
    Ak sú obidva operandy celé čísla, výsledok je celé číslo zaokrúhlené nadol
    Modulo % má vo výsledku znamienko deleného čísla. 

    Priradenie
    = kopíruje primitívnu hodnotu alebo referenciu na objekt
    +=, -=, *=, /=, %= priradenie s operáciou

    Inkrement/Dekrement
    ++x najprv zvýši x, potom vráti novú hodnotu
    x++ najprv vráti starú hodnotu x, potom ju zvýši

    Relačné operátory
    == porovnáva primitívne hodnoty, pri objektoch porovnáva identitu
    ==, !=, <, >, <=, >=
    vysledok je boolean hodnota

    Bitové operátory
    Nepliesť si ich s logickými
    Bitové operátory pracujú na úrovni jednotlivých bitov. 
    Vstupy a výstupy týchto operátorov sú vždy čísla
    &, |, ^, ~, >>, <<, >>>

    Logické operátory
    Pracujú s boolean hodnotami
    &&, ||, !
    short-circuit - predčasné ukončenie, ak už vieme, či výsledok bude true alebo false

    instanceof - zistenie, či daný objekt je inštancia typu alebo implementuje rozhranie
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Aritmetické operátory, modulo, celočíselné delenie
    - Priradenie, správanie operátora =
    - Inkrement a dekrement, rozdiel v správaní ++x a x++
    - Relačné operátory, správanie operátora ==
    - Bitové operátory
    - Logické operátory - čo je short-circuit, aké má využitie
    - instanceof - na čo slúži
