# Teória 17: Usporiadanie prvkov

Pri práci s dátami je často potrebné usporiadať prvky v kolekcii podľa určitých kritérií. Java poskytuje flexibilný a robustný nástroj na implementáciu usporiadania.

Na usporiadanie nám slúžia tieto metódy:

- `java.util.Arrays.sort()` na usporiadanie prvkov v poli
- `java.util.Collections.sort()` na usporiadanie prvkov v kolekciách (napr. List)

Tieto metódy zmenia zadanú kolekciu alebo pole a usporiadajú v nich prvky. Ak nechceme aby sa pôvodná kolekcia menila, je potrebné vytvoriť pred usporiadaním kópiu kolekcie!

## Prirodzené usporiadanie

Spôsob usporiadania si vieme naimplementovať sami. Okrem toho v Jave niektoré dátové typy poskytujú aj tzv. prirodzené usporiadanie (anglicky natural ordering), ktoré predstavuje predvolené (defaultné) poradie prvkov.

Základné dátové typy, ktoré poskytujú prirodzené usporiadanie:

| Typ                         | Prirodzené poradie             |
| --------------------------- | ------------------------------ |
| `Integer`, `Long`, `Double`, ... | číselne vzostupne              |
| `String`                    | lexikograficky (podľa Unicode) |
| `LocalDate`                 | od najstaršieho dátumu         |
| `Enum`                      | podľa poradia deklarácie       |


## Usporiadanie zoznamu čísel

Začneme prirodzeným usporiadaním čísel, ktoré máme v poli.

=== "Usporiadanie prvkov v poli čísel"
    ```java
    // Vytvoríme pole čísel
    Integer[] poleCisel = {4, 5, 2, 1, 3};

    // 4, 5, 2, 1, 3

    // Usporiadanie vzostupne (od najmenšieho po najväčšie)
    Arrays.sort(poleCisel);

    // 1, 2, 3, 4, 5

    // Usporiadanie zostupne (od najväčšieho po najmenšie)
    Arrays.sort(poleCisel, Comparator.reverseOrder());

    // 5, 4, 3, 2, 1
    ```

Polia sú síce jednoduchšie dátové typy, ale majú veľa obmedzení. Preto sa častejšie používajú Java kolekcie, ako napríklad `List`. Usporiadanie čísel v Liste by vyzeralo nasledovne.

=== "Usporiadanie prvkov v zozname čísel"
    ```java
    // Zoznam čísel
    List<Integer> cisla = new ArrayList<>();
    cisla.add(4);
    cisla.add(5);
    cisla.add(2);
    cisla.add(1);
    cisla.add(3);

    // Vypís zoznamu
    for (Integer i : cisla) {
        System.out.println(i);
    }

    // Vytvorenie kópie zoznamu
    List<Integer> cislaVzostupne = new ArrayList<>(cisla); // kopírovací konštruktor

    // Prirodzené zotriedenie vzostupne
    Collections.sort(cislaVzostupne);

    // Výpis
    for (Integer i : cislaVzostupne) {
        System.out.println(i);
    }
    ```

## Comparator

Ak chceme iné ako prirodzené usporiadanie, alebo ak daný dátový typ resp. trieda nemá svoje prirodzené usporiadanie, musíme určiť vlastné pravidlá usporiadania.

V Jave sa tieto pravidlá usporiadania implementujú pomocou rozhrania [`java.util.Comparator`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Comparator.html)

Comparator je malá trieda, ktorá vie porovnať dva objekty a povedať, ktorý má ísť skôr.

Comparator sa používa vtedy, keď:

- triedená trieda nevie sama, ako sa má porovnávať
- chceme viacero rôznych spôsobov triedenia
- nemôžeme alebo nechceme meniť zdrojový kód triedy

=== "Základný tvar rozhrania Comparator"
    ```java
    public interface Comparator<T> {
        int compare(T o1, T o2);
    }
    ```

Základom Comparatora je metóda `compare`, pomocou ktorej bude Java vedieť porovnať 2 objekty. Táto metóda má vrátiť číslo podľa nasledovných pravidiel:

- vráti číslo menšie ako `0` ak o1 je „menší“ ako o2
- vráti `0` ak je rovnaké poradie
- vráti číslo väčšie ako `0` ak o1 je „väčší“ ako o2

Ak by napríklad trieda `Integer` nemala prirodzené usporiadanie, vedeli by sme jej Comparator naimplementovať nasledovne:

=== "Comparator pre prirodzené porovnanie dvoch čísel"
    ```java
    class IntegerComparator implements Comparator<Integer> {
        @Override
        public int compare(Integer a, Integer b) {
            if (a < b) return -1;
            if (a > b) return 1;
            return 0;
        }
    };
    ```

Pri usporiadaní kolekcie sa potom daný Comparator priloží ako druhý argument metódy:

=== "Použitie Comparatora pri usporiadaní"
    ```
    Collections.sort(cisla, new IntegerComparator());
    ```

## Zotriedenie objektov podľa atribútu

Typické použitie Comparatora je pri usporiadaní objektov našich vlastných tried, ktoré nemajú prirodzené usporiadanie. Ako príklad si uvedieme triedu `Osoba`.

=== "Trieda Osoba"
```java
public class Osoba {

    private final String meno;
    private final String priezvisko;
    private final Pohlavie pohlavie;
    private final LocalDate datumNarodenia;

    public Osoba(String meno, String priezvisko,
                 Pohlavie pohlavie, LocalDate datumNarodenia) {
        this.meno = meno;
        this.priezvisko = priezvisko;
        this.pohlavie = pohlavie;
        this.datumNarodenia = datumNarodenia;
    }

    public String getMeno() {
        return meno;
    }

    public String getPriezvisko() {
        return priezvisko;
    }

    public Pohlavie getPohlavie() {
        return pohlavie;
    }

    public LocalDate getDatumNarodenia() {
        return datumNarodenia;
    }

    public String toString() {
        return meno + " " + priezvisko + ", " + getDatumNarodenia();
    }
}
```

Ak by sme mali zoznam takýchto osôb, zotriedenie podľa dátumu narodenia by sme mohli urobiť pomocou nasledovného komparátora:

=== "Comparator pre porovnanie podľa dátumu narodenia"
    ```java
    class NarodenieComparator implements Comparator<Osoba> {
        @Override
        public int compare(Osoba a, Osoba b) {
            return a.getDatumNarodenia().compareTo(b.getDatumNarodenia());
        }
    }
    ```

Keďže dátum narodenia je typu `LocalDate` a táto trieda má prirodzené usporiadanie, vieme toto prirodzené usporiadanie využiť, a to pomocou volania metódy `compareTo`, ktorá porovnáva 2 objekty podobne ako comparator.

S takýmto vlastným komparátorom potom vieme objekty jednoducho usporiadať. Väčšinou nechceme meniť pôvodný zoznam a tak je dobré vytvoriť kópiu zoznamu a túto kópiu potom zoradiť.

=== "Zoradenie zoznamu osôb podľa dátumu narodenia"
    ```java
    List<Osoba> zoradene = new ArrayList<>(osoby);
    Collections.sort(zoradene, new NarodenieComparator());
    ```

Príklad zoradenia zoznamu osôb podľa dátumu narodenia:

```
Andrej Bartoš, 1974-02-21: Javorová 5, Martin
Peter Horváth, 1978-11-04: Dlhá 7, Trnava
Zuzana Miháliková, 1982-05-06: Líščia 21, Prešov
Ján Novák, 1985-03-12: Hlavná 12, Bratislava
Katarína Šimková, 1988-10-09: Dubová 8, Piešťany
Martina Kováčová, 1992-07-25: Školská 45, Košice
Marek Varga, 1995-09-30: Kvetná 18, Žilina
Tomáš Urban, 1999-12-14: Borovicová 6, Banská Bystrica
Lucia Tóthová, 2000-01-18: Mlynská 3, Nitra
Veronika Králiková, 2001-08-02: Lipová 10, Trenčín
```


!!!tip
    Ak máme komparátor, vieme jednoducho vytvoriť opačný komparátor, teda komparátor, ktorý zoradí prvky v opačnom poradí. Tento komparátor vytvoríme pomocou metódy `reverseOrder()`. Príklad

    ```java
    List<Osoba> zoradene = new ArrayList<>(osoby);
    Collections.sort(zoradene, new NarodenieComparator().reverseOrder());
    ```

## Zotriedenie textu

Podobne vieme zoradiť objekty aj podľa textových atribútov (`String`). Prirodzené usporiadanie Stringov je lexikograficky, ktoré vyhovuje pre väčšinu prípadov. Ak však chceme porovnávať text v slovenčine, lexikografické porovnanie nám pri diakritike bude robiť problém.

Najprv si ukážeme usporiadania lexikograficky:

=== "Usporiadanie osôb podľa priezviska, lexikograficky"
    ```java
    class PriezviskoComparator implements Comparator<Osoba> {
        @Override
        public int compare(Osoba a, Osoba b) {
            return a.getPriezvisko().compareTo(b.getPriezvisko());
        }
    }

    // ...

    List<Osoba> zoradene = new ArrayList<>(osoby);
    Collections.sort(zoradene, new PriezviskoComparator());
    ```

Usporiadané osoby budú vyzerať nejako takto:
```
Podla priezviska:
Andrej Bartoš, 1974-02-21: Javorová 5, Martin
Peter Horváth, 1978-11-04: Dlhá 7, Trnava
Martina Kováčová, 1992-07-25: Školská 45, Košice
Veronika Králiková, 2001-08-02: Lipová 10, Trenčín
Zuzana Miháliková, 1982-05-06: Líščia 21, Prešov
Ján Novák, 1985-03-12: Hlavná 12, Bratislava
Lucia Tóthová, 2000-01-18: Mlynská 3, Nitra
Tomáš Urban, 1999-12-14: Borovicová 6, Banská Bystrica
Marek Varga, 1995-09-30: Kvetná 18, Žilina
Katarína Šimková, 1988-10-09: Dubová 8, Piešťany
```

Ako vidno z výstupu, posledná osoba je na zlom mieste. Je to kvôli diakritike, kde písmeno `Š` je lexikograficky podľa Unicode až za klasickými písmenami. Ak vieme, že porovnávame slovenský text, vieme v Jave toto usporiadanie prispôsobiť pomocou triedy [`Collator`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/text/Collator.html), ktorá v sebe obsahuje informácie potrebné pre špecifické jazyky ako je slovenský.

```java
class PriezviskoComparator implements Comparator<Osoba> {
    Collator collator;

    {
        // Collator nakonfigurujeme v inicializačnom bloku triedy
        collator = Collator.getInstance(new Locale("sk", "SK"));
        collator.setStrength(Collator.SECONDARY);
    }

    @Override
    public int compare(Osoba a, Osoba b) {
        return collator.compare(a.getPriezvisko(), b.getPriezvisko());
    }
}
```

Výsledné porovnanie cez Collator už bude správne:

```
Podla priezviska:
Andrej Bartoš, 1974-02-21: Javorová 5, Martin
Peter Horváth, 1978-11-04: Dlhá 7, Trnava
Martina Kováčová, 1992-07-25: Školská 45, Košice
Veronika Králiková, 2001-08-02: Lipová 10, Trenčín
Zuzana Miháliková, 1982-05-06: Líščia 21, Prešov
Ján Novák, 1985-03-12: Hlavná 12, Bratislava
Katarína Šimková, 1988-10-09: Dubová 8, Piešťany
Lucia Tóthová, 2000-01-18: Mlynská 3, Nitra
Tomáš Urban, 1999-12-14: Borovicová 6, Banská Bystrica
Marek Varga, 1995-09-30: Kvetná 18, Žilina
```

## Zotriedenie objektov podľa viacerých atribútov

Comparator môže objekty porovnať akýmkoľvek spôsobom, nemusí použiť iba jeden atribút objektu. Často napríklad je potrebné porovnať podľa viacerých atribútov, nap. najpr podľa priezviska a potom podľa mena. V nasledujúcom príklade vytvoríme Comparator, ktoré usporiada najpr podľa pohlavia a potom podľa dátumu narodenia. 

=== "Usporiadanie podľa viacerých atribútov"
    ```java
    class MultiComparator implements Comparator<Osoba> {
        @Override
        public int compare(Osoba a, Osoba b) {
            int r = a.getPohlavie().compareTo(b.getPohlavie());

            // Ak prvé porovnanie objekty usporiadalo, 
            // tak už nemusíme porovnávať podľa druhého atribútu
            if (r != 0)
                return r;

            return b.getDatumNarodenia().compareTo(a.getDatumNarodenia());
        }
    }
    ```

Samotné usporiadanie zoznamu bude vyzerať nasledovne

```java
List<Osoba> zoradene = new ArrayList<>(osoby);
Collections.sort(zoradene, new MultiComparator());
```

Výsledné usporiadanie osôb:

```
Podla pohlavia a potom podla datumu narodenia:
Tomáš Urban, 1999-12-14: Borovicová 6, Banská Bystrica
Marek Varga, 1995-09-30: Kvetná 18, Žilina
Ján Novák, 1985-03-12: Hlavná 12, Bratislava
Peter Horváth, 1978-11-04: Dlhá 7, Trnava
Andrej Bartoš, 1974-02-21: Javorová 5, Martin
Veronika Králiková, 2001-08-02: Lipová 10, Trenčín
Lucia Tóthová, 2000-01-18: Mlynská 3, Nitra
Martina Kováčová, 1992-07-25: Školská 45, Košice
Katarína Šimková, 1988-10-09: Dubová 8, Piešťany
Zuzana Miháliková, 1982-05-06: Líščia 21, Prešov
```

## Zhrnutie teórie

Všetky príklady uvedené na tejto hodine viete nájsť a vyskúšať v repozitári na adrese [https://github.com/wagjo/opg-jackson](https://github.com/wagjo/opg-jackson)

- [x] Usporiadanie prvkov
    * [ ] `java.util.Arrays.sort()` na usporiadanie prvkov v poli
    * [ ] `java.util.Collections.sort()` na usporiadanie prvkov v kolekciách (napr. List)
    * [ ] Usporiadanie mení kolekciu alebo pole! Ak to nechceme, je potrebné vytvoriť pred usporiadaním kópiu kolekcie!
- [x] Prirodzené usporiadanie
    * [ ] Anglicky Natural ordering, predstavuje predvolené (defaultné) poradie prvkov
    * [ ] `Integer`, `Long`, `Double`, ... - číselne vzostupne
    * [ ] `String` - lexikograficky (podľa Unicode)
    * [ ] `LocalDate` - od najstaršieho dátumu
    * [ ] `Enum` - podľa poradia deklarácie
- [x] Comparator
    * [ ] Vlastné pravidlá usporiadania implementujú pomocou rozhrania java.util.Comparator
    * [ ] Comparator je malá trieda, ktorá vie porovnať dva objekty a povedať, ktorý má ísť skôr.
    * [ ] Základom Comparatora je metóda compare, pomocou ktorej bude Java vedieť porovnať 2 objekty
- [x] Comparator sa používa vtedy, keď:
    * [ ] triedená trieda nevie sama, ako sa má porovnávať
    * [ ] chceme viacero rôznych spôsobov triedenia
    * [ ] nemôžeme alebo nechceme meniť zdrojový kód triedy
- [x] Metóda compare má vrátiť číslo podľa nasledovných pravidiel:
    * [ ] vráti číslo menšie ako 0 ak o1 je „menší“ ako o2
    * [ ] vráti 0 - rovnaké poradie
    * [ ] vráti číslo väčšie ako 0 ak o1 je „väčší“ ako o2
- [x] Collator
    * [ ] Pri porovnávaní textu obsahujúceho diakritiku je vhodné použiť Collator, ktorý porovná z ohľadom na špecifiká daného jazyka


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Usporiadanie prvkov
     
    java.util.Arrays.sort() na usporiadanie prvkov v poli
    java.util.Collections.sort() na usporiadanie prvkov v kolekciách (napr. List)
    
    Usporiadanie mení kolekciu alebo pole! Ak to nechceme, je potrebné vytvoriť pred usporiadaním kópiu kolekcie!

    Prirodzené usporiadanie, má ho väčšina štandardných dátových typov
    Anglicky Natural ordering, predstavuje predvolené (defaultné) poradie prvkov

    Comparator

    public interface Comparator<T> {
        int compare(T o1, T o2);
    }

    Vlastné pravidlá usporiadania implementujú pomocou rozhrania java.util.Comparator
    Comparator je malá trieda, ktorá vie porovnať dva objekty a povedať, ktorý má ísť skôr.
    Základom Comparatora je metóda compare, pomocou ktorej bude Java vedieť porovnať 2 objekty

    Comparator sa používa vtedy, keď:
    - triedená trieda nevie sama, ako sa má porovnávať
    - chceme viacero rôznych spôsobov triedenia
    - nemôžeme alebo nechceme meniť zdrojový kód triedy

    Metóda compare má vrátiť číslo podľa nasledovných pravidiel:
    - vráti číslo menšie ako 0 ak o1 je „menší“ ako o2
    - vráti 0 - rovnaké poradie
    - vráti číslo väčšie ako 0 ak o1 je „väčší“ ako o2

    class IntegerComparator implements Comparator<Integer> {
        @Override
        public int compare(Integer a, Integer b) {
            if (a < b) return -1;
            if (a > b) return 1;
            return 0;
        }
    };

    // kópia, ak nechceme meniť pôvodný zoznam
    List<Osoba> zoradene = new ArrayList<>(cisla);
    // zostupne
    Collections.sort(zoradene, new IntegerComparator());
    // vzostupne
    Collections.sort(zoradene, new IntegerComparator().reverseOrder());

    Collator je trieda ktorá porovná text z ohľadom na špecifiká daného jazyka
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Čo je prirodzené usporiadanie
    - Na čo slúži rozhranie Comparator
    - Na čo slúži Collator
