# Cvičenie 6: Práca s reťazcami znakov

Dnešné cvičenie si ukážeme, ako sa v Jave pracuje s reťazcami znakov. Ukážeme si spôsoby vytvárania a manipulácie s reťazcami. Taktiež si predstavíme rôzne pomocné metódy na vyhľadávanie a transformáciu reťazcov.

## String

V Jave sú reťazce znakov reprezentované ako objekty triedy `String`. Táto trieda je definovaná v balíku `java.lang`.

Reťazce znakov v Jave majú tieto vlastnosti:

- Sú objektami triedy `String`, nie sú primitívne
- Sú nemenné, po vytvorení sa objekt nedá meniť
- Môžu byť použité ako hodnoty v príkaze `switch`
- Reťazec má dĺžku a index, viem pristupovať k jednotlivým znakom

Reťazce vieme vytvoriť rôznymi spôsobmi.

```java
String s1 = "Hello world!"; // pomocou literálu

String s2 = new String("Hello world!"); // pomocou operátora new

char[] pole = {'H', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '!'};
String s3 = new String(pole); // z poľa znakov

int i = -37;
String s4 = Integer.toString(i); // pomocou metódy toString
```

Pri tvorbe reťazca môžeme používať aj metódu `format()` na formátovanie reťazca, ktorá sa správa podobne ako metóda `printf()`, akurát namiesto vypísania na obrazovku vytvorí string.

```java
String meno = "Fero";
int vek = 25;
String s = String.format("Meno: %s, vek: %d", meno, vek);
```

Pri vytváraní reťazcov si vieme pomôcť aj s operátormi `+`, `+=` alebo s metódou `repeat()`.

```java
String s1 = "Hello " + "world"; // "Hello world"
s1 += "!"; // "Hello world!"

String s2 = "ha".repeat(5); // "hahahahaha"
```

!!! abstract "Dokumentácia"

    Pre viac detailov si pozrite oficiálnu dokumentáciu triedy [`java.lang.String`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/String.html)


## Základná práca s reťazcami

Dĺžku reťazca viem zistiť pomocou metódy `length()`. K jednotlivým znakom viem pristupovať pomocou indexu a metódy `charAt()`

```java
String s = "Hello world!";
System.out.println(s.length()); // 12
System.out.println(s.charAt(4)); // 'o'
```

Reťazce medzi sebou viem navzájom porovnať rôznymi spôsobmi.

```java
String s1 = "Hello";
String s2 = "world!";

boolean rovnake = s1.equals(s2); // Majú reťazce rovnaké hodnoty?

boolean rovnake = s1.equalsIgnoreCase(s2); // porovnanie bez ohladu na veľkosť písmen
```

Ak chceme porovnať reťazce lexikograficky (podľa abecedy), použijeme na to metódu `compareTo()`

```java
System.out.println("abc".compareTo("abc")); // 0  (rovnaké)
System.out.println("abc".compareTo("abd")); // -1 (c < d)
System.out.println("abd".compareTo("abc")); //  1 (d > c)
System.out.println("abc".compareTo("ab"));  //  1 (dlhší je väčší)
System.out.println("ab".compareTo("abc"));  // -1 (kratší je menší)
```

Toto porovnávanie sa používa vždy keď sa reťazce zoradzujú, napr. pri kolekciách ako je pole.

```java
String[] words = {"pear", "apple", "orange"};
Arrays.sort(words); 
// zoradí pomocou compareTo: ["apple", "orange", "pear"]
```

Či je reťazec prázdny alebo obsahuje iba biele znaky (whitespaces) vieme zistiť pomocou metódy `isBlank()`. Na zistenie, či je reťazec prázdny používame metódu `isEmpty()`. Z reťazca vieme vytvoriť podreťazec pomocou metódy `substring()`.

```java
String s = "Hello world!";
String s2 = s.substring(6, 11); // "world"

if (s.isBlank()) {
    System.out.println("Reťazec je prázdny");
}
```

## Vyhľadávanie v reťazci

V reťazci vieme vyhľadávať, či sa tam nachádza nejaký znak alebo reťazec znakov. Metóda `indexOf()` nám zistí prvú pozíciu, na ktorej sa hľadaný znak alebo reťazec nachádza. Ak sa hľadaná vec v texte nenachádza, metóda vráti číslo `-1`. V tejto metóde môžeme tiež zadať, v akej časti reťazca má hľadať. Ak chceme iba vedieť, či sa tam hľadaný výraz nachádza alebo nie, môžeme použiť aj metódu `contains()`.

```java
String s = "banana";
System.out.println(s.indexOf('n'));     // 2
System.out.println(s.lastIndexOf("na")); // 4
System.out.println(s.contains("ban"));   // true
```

!!! example "Príklad: Nájdene prípony v názve súboru"

    ```java
    public static String getFileExtension(String fileName) {
        // Kontrola, či názov súboru nie je null alebo prázdny
        if (fileName == null || fileName.isEmpty()) {
            return "Žiadna prípona";
        }

        // Nájdenie posledného výskytu bodky
        int dotIndex = fileName.lastIndexOf('.');

        // Ak bodka neexistuje alebo je na konci názvu
        if (dotIndex == -1 || dotIndex == fileName.length() - 1) {
            return "Žiadna prípona";
        }

        // Vracia príponu (časť za poslednou bodkou)
        return fileName.substring(dotIndex + 1);
    }
    ```

Niekedy potrebujeme zistiť, či sa reťazec začína (alebo končí) nejakým textom. Na to v Jave máme metódy `startswith()` a `endswith()`. 

```java
String url = "https://oop.wagjo.com";
System.out.println(url.startsWith("https")); // true
System.out.println(url.endsWith(".com"));    // true
```

## Úprava reťazcov

<div class="md-has-sidebar" markdown>
<main markdown>
Java nám poskytuje aj niekoľko metód na úpravu reťazcov. Keďže reťazce sú v Jave nemenné, všetky tieto metódy vracajú nový upravený reťazec.

Metóda `strip()` vráti reťazec bez ASCII medzier na začiatku a konci reťazca. Ak chcem odstrániť iba medzeri iba na začiatku alebo na konci, použijeme metódy `stripLeading()` a `stripTrailing()`.

</main>
  <aside markdown>
Na odstránenie medzier máme aj metódu `trim()`, ktorá však nepodporuje všetky druhy bielych znakov. Preferujte metódu `strip()`
</div>

Na zmenu všetkých písmen v reťazci na veľké alebo malé máme metódy `toUpperCase()` a `toLowerCase()`. 

<div class="md-has-sidebar" markdown>
<main markdown>
Často používanou metódou pri zmene reťazcov je metóda `replace()`, pomocou ktorej vieme nahradiť znak alebo skupinu znakov niečim iným.

```java
String text = "Ahoj svet";
String replaced = text.replace("svet", "Java");
System.out.println(replaced); // Ahoj Java
```
</main>
  <aside markdown>
Ak chceme použiť regulárne výrazy, vieme použiť metódy `replaceAll()` a `replaceFirst()`.
</div>

## Rozdeľovanie a spájanie reťazcov

<div class="md-has-sidebar" markdown>
<main markdown>
Na rozdelenie reťazca máme v Jave metódu `split()`, ktorá prijíma regulárny výraz. Táto metóda vracia pole reťazcov, a vo výsledných reťazcoch sa text, pomocou ktorého sa oddeľovalo, nebude nachádzať.

```java
String s = "a,b,c";
String[] parts = s.split(",");
for (String part : parts) {
    System.out.println(part);
}
// Výstup: a b c
```

</main>
  <aside markdown>
Ak chceme zachovať vo výsledku aj oddeľovače, použijeme metódu `splitWithDelimiters()`
</div>

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Stringy v Jave. Vysvetli mi jednoducho, [čo sú regulárne výrazy a ako sa pri Stringoch v Jave používajú](https://grok.com/share/c2hhcmQtMg%3D%3D_75b56967-2e03-418c-bb19-0a605a37e8af).

Viacero reťazcov dokopy vieme spojiť pomocou `join()`. Prvý argument tejto metódy je reťazec, ktorý sa použije ako spojovník.

```java
String joined = String.join("-", "a", "b", "c");
System.out.println(joined); // "a-b-c"
```

## Efektívna tvorba reťazcov

Niekedy potrebujeme pri tvorbe reťazca vykonať viacero zmien, alebo reťazec tvoríme postupne. Pretože každá zmena reťazca vytvára reťazec nový, takéto zložitejšie úpravy môžu byť dosť neefektívne a komplikované na písanie. Java nám naštastie ponúka riešenie pomocou triedy `StringBuilder`. Táto trieda je *mutable*, teda jej objekty sú meniteľné a poskytuje metódy na ľahšiu manipuláciu s reťazcami ako pri triede `String`.

!!! abstract "Dokumentácia"

    Pre viac detailov si pozrite oficiálnu dokumentáciu triedy [`java.lang.StringBuilder`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/StringBuilder.html)

S touto triedou sa pracuje nasledovne:

1. Vytvorím si objekt triedy `StringBuilder` pomocou vhodného konštruktora
1. Vykonám úpravy, ako napr. vloženie, nahradenie alebo vymazanie textu
1. Výsledný reťazec získam pomocou metódy `toString()`

Na zmenu textu nám trieda `StringBuilder` poskytuje metódy `append()`, `insert()`, `reverse()` a `delete()`.

!!! example "Príklad použitia triedy `StringBuilder`"

    ```java
    String[] items = {"Jablko", "Hruška", "Banán"};

    StringBuilder html = new StringBuilder("<ul>");
    for (String item : items) {
        html.append("<li>").append(item).append("</li>");
    }
    html.append("</ul>");

    String htmlText = html.toString();
    System.out.println(htmlText);

    // "<ul><li>Jablko</li><li>Hruška</li><li>Banán</li></ul>"    
    ```


## Úlohy na precvičenie

!!! example "Úloha 6.1: Obrátenie reťazca"

    Vypíš reťazec v opačnom poradí

!!! example "Úloha 6.2: Zistenie palindrómu"

    Zisti, či je zadaný reťazec palindróm (čítaný zľava aj sprava je rovnaký).

    𝙱𝚘𝚗𝚞𝚜: Ignoruj medzery a veľkosti písmen

    𝕄𝕖𝕘𝕒 𝕓𝕠𝕟𝕦𝕤: Ignoruj interpunkčné znamienka (., !, ?, atď.)
    
    Ｕｌｔｒａ ｂｏｎｕｓ: Ignoruj diakritiku

!!! example "Úloha 6.3: Počet slov v texte"

    Zadaný text obsahuje viac slov oddelených medzerami. Spočítaj, koľko slov obsahuje.

!!! example "Úloha 6.4: Zmena veľkosti písmen"

    Napíš program, ktorý všetky malé písmená v reťazci zmení na veľké a naopak.

!!! example "Úloha 6.5: Najdlhšie slovo"

    V texte oddelenom medzerami nájdi a vypíš najdlhšie slovo.

!!! example "Úloha 6.6: Odstránenie číslic"

    Zo zadaného reťazca odstráň všetky číslice a vypíš výsledok.

!!! example "Úloha 6.7: Každé slovo s veľkým písmenom"

    Zmeň reťazec tak, aby každé slovo začínalo veľkým písmenom (napr. `"java je fajn"` → `"Java Je Fajn"`).

!!! example "Úloha 6.8: Kontrola anagramu"

    Zisti, či dva reťazce sú anagramy *(obsahujú tie isté písmená, ale v inom poradí)*.

!!! example "Úloha 6.9: Validácia e-mailu"

    Skontroluj, či zadaný reťazec vyzerá ako validná e-mailová adresa (stačí základná kontrola: obsahuje `@` a `.` na správnych miestach).

!!! example "Úloha 6.10: Spojenie viacerých slov"

    Napíšte program, ktorý od používateľa načíta viacero slov (počet zadá používateľ) a pomocou `StringBuilder` ich spojí do jednej vety s medzerami medzi slovami.


## Zhrnutie cvičenia

- [x] V Jave sú reťazce znakov reprezentované ako objekty triedy `java.lang.String`
    * [ ] sú nemenné, po vytvorení sa objekt nedá meniť
    * [ ] Môžu byť použité ako hodnoty v príkaze switch
    * [ ] Reťazec má dĺžku a index, viem pristupovať k jednotlivým znakom

Na tomto cvičení sme si predstavili tieto metódy:

| **Metóda**            | **Popis**                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| `length()`            | Vráti dĺžku reťazca (počet znakov)                                       |
| `charAt()`            | Vráti znak na zadanom indexe v reťazci                                   |
| `equals()`            | Porovná, či majú dva reťazce rovnakú hodnotu                              |
| `equalsIgnoreCase()`  | Porovná dva reťazce bez ohľadu na veľkosť písmen                         |
| `compareTo()`         | Lexikograficky porovná dva reťazce (podľa abecedy)                       |
| `isBlank()`           | Skontroluje, či je reťazec prázdny alebo obsahuje iba biele znaky         |
| `isEmpty()`           | Skontroluje, či je reťazec prázdny                                       |
| `substring()`         | Vráti podreťazec zo zadaného rozsahu indexov                             |
| `indexOf()`           | Vráti index prvého výskytu zadaného znaku alebo podreťazca, alebo -1     |
| `lastIndexOf()`       | Vráti index posledného výskytu zadaného znaku alebo podreťazca, alebo -1 |
| `contains()`          | Skontroluje, či reťazec obsahuje zadaný podreťazec                        |
| `startsWith()`        | Skontroluje, či reťazec začína zadaným podreťazcom                        |
| `endsWith()`          | Skontroluje, či reťazec končí zadaným podreťazcom                         |
| `strip()`             | Odstráni biele znaky (ASCII) zo začiatku a konca reťazca                 |
| `stripLeading()`      | Odstráni biele znaky zo začiatku reťazca                                 |
| `stripTrailing()`     | Odstráni biele znaky z konca reťazca                                     |
| `toUpperCase()`       | Prevedie všetky písmená v reťazci na veľké                               |
| `toLowerCase()`       | Prevedie všetky písmená v reťazci na malé                                |
| `replace()`           | Nahradí všetky výskyty zadaného znaku alebo podreťazca novým             |
| `replaceAll()`        | Nahradí všetky výskyty podreťazca zodpovedajúceho regulárnemu výrazu     |
| `replaceFirst()`      | Nahradí prvý výskyt podreťazca zodpovedajúceho regulárnemu výrazu        |
| `split()`             | Rozdelí reťazec na pole podreťazcov podľa zadaného regulárneho výrazu    |
| `join()`              | Spojí viacero reťazcov do jedného s použitím zadaného oddeľovača         |
| `format()`            | Vytvorí formátovaný reťazec podobný `printf()`                           |
| `repeat()`            | Opakuje reťazec zadaný počet krát                                       |
| `toString()`          | Vráti reťazec z objektu `StringBuilder`                 |
| `append()`            | Pridá text na koniec reťazca v objekte `StringBuilder`                   |
| `insert()`            | Vloží text na zadaný index v objekte `StringBuilder`                     |
| `reverse()`           | Obráti poradie znakov v objekte `StringBuilder`                          |
| `delete()`            | Odstráni časť reťazca v objekte `StringBuilder` zo zadaného rozsahu      |

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    STRING

    Reťazce znakov:
    - Sú objekty triedy java.lang.String, nie sú primitívne
    - Sú nemenné, po vytvorení sa objekt nedá meniť
    - Reťazec má dĺžku a index, viem pristupovať k jednotlivým znakom

    length() - dĺžka
    charAt() - znak na danej pozícii
    equals() - porovnanie hodnôt
    compareTo() - porovnanie "podľa abecedy"
    isBlank() - je reťazec prázdny?
    substring() - podreťazec
    indexOf() - prvý výskyt znaku alebo podreťazca
    startsWith() - či začína určitým reťazcom
    strip() - odstráni biele znaku zo začiatku a z konca
    toUpperCase() - prevedie na veľké písmená
    replace() - nahradí text
    split() - rozdelí do poľa reťazcov
    join() - spojí viacero reťazcov dokopy
    repeat() - opakuje daný reťazec
    StringBuilder - mutable trieda na manipuláciu s reťazcami
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Ako sú v Jave reprezentované reťazce
    - Vlasnosti reťazcov
    - Základné metódy na prácu s reťazcami
    - Čo je a na čo sa používa treida StringBuilder
    