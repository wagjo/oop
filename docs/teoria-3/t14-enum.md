# Teória 14: Enumeračné typy

Pri programovaní často potrebujeme dátové typy, ktoré môžu nadobúdať iba niekoľko pevných hodnôt. Takéto nečíselné typy s konečným počtom hodnôt nazývame **enumeračné typy**. Príklady, kedy potrebujeme premenné s pevným počtom hodnôt:

- Stav objednávky: nová, zpracovávaná, expedovaná, doručená
- Dni v týždni: pondelok, utorok, streda, štvrtok, piatok, sobota, nedeľa
- Pohlavie: muž, žena
- Svetové strany: sever, juh, východ, západ
- Veľkosť trička: XS, S, M, L, XL, XXL
- Úroveň obtiažnosti: ľahká, stredná, ťažká
- Typ používateľa: admin, user, anonymous
- Priorita: low, medium, high, critical
- Typ platby: karta, dobierka, osobny odber

Ako takéto hodnoty čo najlepšie v Jave reprezentovať? Máme viacero možností, a často závisí od konkrétnej situácie, ktorá možnosť je tá najlepšia:

## Enumeračné typy pomocou Stringov

Na reprezentáciu enumeračných typov vieme použiť reťazce v týchto prípadoch:

- hodnôt je pomerne veľa (cca viac ako 30)
- hodnoty nevieme zoradiť
- v budúcnosti môžu pribudnúť nové hodnoty
- hodnoty sú rovnocenné, nemáme niektoré hodnoty, ktoré majú špeciálnu logiku alebo dáta

Typický príklad použitia `Stringov` pre enumeračné hodnoty je napríklad krajina alebo platobná mena

=== "Použitie `String`ov pre enumeračné hodnoty"

    ```java
    class Person {
        private String name;
        private String country;

        public Person(String country) {
            this.country = country;
        }

        public String getCountry() {
            return country;
        }
    }

    Person p = new Person("SK");
    System.out.println(p.getCountry());
    ```

Medzi ďalšie situácie, kedy je String lepšie riešenie patria napr. kategórie výrobkov, značky tovarov a kategórie z databáz, nad ktorými nemáme kontrolu.

Medzi nevýhody použitia Stringov ako enumeračných typov patrí:

- pri zadávaní môže nastať preklep
- nemáme žiadnu typovú kontrolu

## Pomocou číselných typov

Druhou možnosťou je použiť celočíselný dátový typ, napr. `int`. Vhodné je to v nasledujúcich situáciách:

- hodnoty vieme zoradiť
- hodnôt je veľké množstvo (viac ako 30)
- hodnoty majú matematický význam
- potrebujeme veľmi efektívne spracovanie (rýchlosť, málo miesta v pamäti)
- hodnoty majú číselný štandard (napr. HTTP statusy, MIDI udalosti)

Typický príklad použitia čísel pre enumeračné hodnoty sú HTTP kódy:

=== "Použitie celých čísel pre enumeračné typy"

    ```java
    class Response {
        int code;
        String body;

        Response(int code, String body) {
            this.code = code;
            this.body = body;
        }

        boolean isSuccess() { return code >= 200 && code < 300; }
        boolean isNotFound() { return code == 404; }
    }

    // Použitie
    Response r = new Response(404, "Stránka nenájdená");
    ```

Medzi nevýhody použitia celých čísel ako enumeračných typov patrí:

- musíme štandardizovať číselnú hodnotu (Pondelok bude 0 alebo 1?, Najvyššia priorita je 1 alebo 10?)
- nemáme žiadnu typovú kontrolu

## Pomocou tried a dedičnosti

Na reprezentáciu enumeračných hodnôt môžeme použiť aj nástroje objektového programovania a vytvoriť si samostatnú hierarchiu tried. Táto možnosť je vhodná v nasledujúcich prípadoch:

- hodnôt nie je veľa
- k hodnotám je potrebné priradiť dodatočnú logiku alebo stav/dáta

Ako príklad môžme uviesť spôsob platby alebo doručenia tovaru, kedy niektoré typy doručenia potrebujú uchovávať dodatočné informácie:

=== "Použitie tried a dedičnosti pre enumeračné typy"

    ```java
    public class Objednavka {
        private Tovar[] tovar;
        private double celkovaSuma;
        private Adresa adresa;
        private Dorucenie sposobDorucenia;
        private Platba sposobPlatby;
    }

    public abstract class Platba {
        public abstract boolean mozeOdoslat();
    }

    public class Dobierka extends Platba {
        public boolean mozeOdoslat() {
            return true;
        }
    }

    public class PlatbaKartou extends Platba {
        private boolean jeZaplatene;

        public boolean mozeOdoslat() {
            return jeZaplatene;
        }

        public void otvorPlatobnuBranu() {
            // ...
        }
    }

    public class PlatbaPrevodom extends Platba {
        private String variabilnySymbol;

        public boolean mozeOdoslat() {
            // ... skontroluj ucet
        }

        public String getVariabilnySymbol () {
            return variabilnySymbol;
        }

    }
    ```

Nevýhody tohto prístupu:

- Existuje viacero inštancií jednej hodnoty
- Príliš veľa kódu, veľká zložitosť


## Pomocou triedy Enum

V Jave máme možnosť reprezentovať enumeračné typy pomocou špeciálnej triedy `enum`. Tá nám umožňuje zadefinovať fixnú množinu hodnôt.

Enum (enumeration) je špeciálny typ triedy v Jave, ktorý reprezentuje pevný, nemenný a obmedzený zoznam hodnôt.

=== "Enum v Jave"

    ```java
    public enum Pohlavie { MUZ, ZENA }

    public enum svetovaStrana { SEVER, VYCHOD, JUH, ZAPAD }
    ```

Enum je trieda so statickými konštantami, preto názvy hodnôt píšeme veľkými písmenami. Enum nemá verejné konštruktory a je zakázané vytvárať vlastné objekty Enumu. Jediné objekty tejto triedy sú teda konštanty uvedené v triede.

=== "Použitie Enumu"

    ```java
    public class Osoba {
        private String meno;
        private String priezvisko;
        private Pohlavie pohlavie;

        public Osoba(String meno, String priezvisko, Pohlavie pohlavie) {
            this.meno = meno;
            this.priezvisko = priezvisko;
            this.pohlavie = pohlavie;
        }
    }

    Osoba o = new Osoba("Ján", "Novák", Pohlavie.MUZ);

    // použitie v podmienke
    if(o.getPohlavie() == Pohlavie.MUZ) {
        // ...
    }
    ```

Vlastnosti Enumu:

- Každá hodnota (napr. SEVER) je `public static final` inštancia enumu
- Enum je implicitne final. Nemôžeš z neho dediť
- Všetky enumy dedia od `java.lang.Enum`

Enum je vhodný pre nasledovné situácie:

- Existuje pevná množina hodnôt
- Počet hodnôt nie je veľký
- Hodnoty nepotrebujú špeciálne dáta alebo správanie
- Potrebujeme silnú typovú kontrolu

Nevýhody enumu:

- Zoznam hodnôt je pevne daný a nedá sa rozšíriť
- Je pomalý ak máme veľké množstvo hodnôt (viac ako 1000)


### Enum v podmienke switch

Veľkou výhodou enumov je, že ich vieme použiť v podmienkach `switch`. Kompilátor sa navyše postará, že v podmienke `switch` musíte mať ošetrené všetky možné hodnoty daného enumu.

=== "Použitie enumu v podmienke switch"

    ```java
    public enum OrderStatus {
        NEW, PAID, SHIPPED, DELIVERED
    }

    public static void sendNotification(OrderStatus status) {
        switch (status) {
            case NEW:
                System.out.println("Nová objednávka");
                break;
            case PAID:
                sendEmail("Platba prijatá");
                break;
            case SHIPPED:
                sendSms("Zásielka odoslaná");
                break;
            case DELIVERED:
                sendEmail("Tovar doručený");
                break;
            default:
                // nie je potrebné – kompilátor ti povie, ak zabudneš nový stav
                System.out.println("Nič sa neposiela");
        }
    }
    ```

### Dodatočné informácie

Enum môže obsahovať dodatočné atribúty a metódy. To sa používa napríklad, keď pre jednotlivé hodnoty sú k dispozícii aj ďalšie informácie.

=== "Enum s atribútmi a metódami"

    ```java
    enum Planet {
        MERCURY(3.303e+23, 2.4397e6),
        EARTH(5.976e+24, 6.37814e6);

        private final double mass;
        private final double radius;

        Planet(double mass, double radius) {
            this.mass = mass;
            this.radius = radius;
        }

        double getMass() {
            return mass;
        }

        double surfaceGravity() {
            final double G = 6.67300E-11;
            return G * mass / (radius * radius);
        }
    }

    double g = Planet.EARTH.surfaceGravity();
    ```

### Enum z reťazca

Objekty triedy enum vieme vytvárať aj z reťazcov, a to za pomoci metódy `valueOf()`. To je užitočné pri načítavaní hodnôt so súboru alebo databázy. 

Podobne ak chceme pre danú hodnotu zistiť jej reprezentáciu vo fome reťazca, enum nám ponúka metódu `name()`

=== "Príklad vytvorenia enumu z reťazcov"

    ```java
    SvetovaStrana strana = SvetovaStrana.valueOf("SEVER");

    System.out.println(strana.name()); // SEVER
    ```

### Enum v Java API

Java API obsahuje niekoľko enum tried, ktoré sa často používajú. Zopár príkladov:

- [java.time.Month](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/time/Month.html) - mesiace v roku - JANUARY, MARCH
- [java.nio.file.StandardOpenOption](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/nio/file/StandardOpenOption.html) - režim otvárania súboru - READ, WRITE
- [java.util.concurrent.TimeUnit](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/TimeUnit.html) - Jednotka času - DAYS, SECONDS
- [java.math.RoundingMode](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/RoundingMode.html) - spôsob zaokrúhľovania - FLOOR, CEILING


## Ktorý spôsob si vybrať?

| Kritérium / Vlastnosť                    | **Enum**             | **String**          | **Int / Integer**            | **Triedy + dedičnosť**      |
| ---------------------------------------- | -------------------- | ------------------- | ---------------------------- | --------------------------- |
| **Typ hodnôt**                           | Uzavretá sada        | Ľubovoľný text      | Číselné kódy                 | Hierarchia objektov         |
| **Možnosť pridávať hodnoty za behu**     | ❌ Nie                | ✔️ Áno              | ✔️ Áno                       | ✔️ Áno                      |
| **Bezpečnosť typu (type safety)**        | ✔️ Výborná           | ❌ Slabá             | ❌ Slabá                      | ✔️ Výborná                  |
| **Riziko preklepov**                     | ❌ Žiadne             | ⚠️ Veľké riziko     | ❌ Žiadne                     | ❌ Žiadne                    |
| **Výkonnosť / pamäť**                    | ⚠️ Stredná (objekty) | ⚠️ Slabšia          | ✔️ Najlepšia                 | ⚠️ slabšia                  |
| **Možnosť uloženia do DB bez mapovania** | ⚠️ Áno (string)      | ✔️ Áno              | ✔️ Áno                       | ❌ Nie (ID potrebné)         |
| **Stabilita hodnôt**                     | ✔️ Konštantná        | ⚠️ Závisí od dát    | ✔️ Stabilná                  | ✔️ Stabilná                 |
| **Komplexná logika pre každú hodnotu**   | ⚠️ Obmedzená         | ❌ Nie               | ❌ Nie                        | ✔️ Najlepšie                |
| **Možnosť dedičnosti / polymorfizmu**    | ❌ Nie                | ❌ Nie               | ❌ Nie                        | ✔️ Áno                      |
| **Vhodné pre plug-in systém**            | ❌ Nie                | ✔️ Áno              | ✔️ Áno                       | ✔️ Najlepšie                |
| **Vhodné pre bitové príznaky (flags)**   | ❌ Nie                | ❌ Nie               | ✔️ Najlepšie                 | ⚠️ Možné, ale zbytočné      |
| **Jednoduchosť použitia**                | ✔️ Jednoduché        | ✔️ Veľmi jednoduché | ✔️ Jednoduché                | ⚠️ Zložitejšie              |
| **Backwards compatibility pri zmene**    | ⚠️ Rizikové          | ✔️ Bezpečné         | ✔️ Bezpečné                  | ✔️ Bezpečné                 |
| **Kedy použiť**                          | Fixné hodnoty        | Dynamické hodnoty   | Kódy, protokoly, performance | Komplexné typy so správaním |


## Zhrnutie teórie

- [x] Enumeračné typy - dátové typy s konečným počtom hodnôt
    * [ ] Stav objednávky: nová, zpracovávaná, expedovaná, doručená
    * [ ] Dni v týždni: pondelok, utorok, streda, štvrtok, piatok, sobota, nedeľa
    * [ ] Pohlavie: muž, žena
    * [ ] Svetové strany: sever, juh, východ, západ
    * [ ] Veľkosť trička: XS, S, M, L, XL, XXL
- [x] Enumeračné typy pomocou Stringov - napr. pre kódy štátov, kategórie výrobkov
    * [ ] Ak je hodnôt pomerne veľa (cca viac ako 30)
    * [ ] Ak hodnoty nevieme zoradiť
    * [ ] Ak v budúcnosti môžu pribudnúť nové hodnoty
    * [ ] Ak sú hodnoty rovnocenné, teda nemáme niektoré hodnoty, ktoré majú špeciálnu logiku alebo dáta
    * [ ] Nevýhody: Pri zadávaní môže nastať preklep, nemáme žiadnu typovú kontrolu
- [x] Pomocou číselných typov - napr. HTTP statusy, chybové kódy
    * [ ] Ak hodnoty vieme zoradiť
    * [ ] Ak je hodnôt veľké množstvo (viac ako 30)
    * [ ] Ak hodnoty majú matematický význam
    * [ ] Ak potrebujeme veľmi efektívne spracovanie (rýchlosť, málo miesta v pamäti)
    * [ ] Ak majú hodnoty číselný štandard (napr. HTTP statusy, MIDI udalosti)
    * [ ] Nevýhody: musíme štandardizovať číselnú hodnotu (Pondelok bude 0 alebo 1?, Najvyššia priorita je 1 alebo 10?)
    * [ ] Nevýhody: nemáme žiadnu typovú kontrolu
- [x] Pomocou tried a dedičnosti - napr. spôsob platby alebo doručenia
    * [ ] Ak hodnôt nie je veľa
    * [ ] Ak je k hodnotám potrebné priradiť dodatočnú logiku alebo stav/dáta
    * [ ] Nevýhody: Existuje viacero inštancií jednej hodnoty; Príliš veľa kódu, veľká zložitosť
- [x] Pomocou triedy Enum - napr. svetová strana, pohlavie
    * [ ] `enum` (enumeration) je špeciálny typ triedy v Jave, ktorý reprezentuje pevný, nemenný a obmedzený zoznam hodnôt.
    * [ ] `public enum Pohlavie { MUZ, ZENA }`
    * [ ] Enum je trieda so statickými konštantami, preto názvy hodnôt píšeme veľkými písmenami. 
    * [ ] Enum nemá verejné konštruktory a je zakázané vytvárať vlastné objekty Enumu. Jediné objekty tejto triedy sú teda konštanty uvedené v triede.
    * [ ] Každá hodnota (napr. `SEVER`) je public `static final inštancia` enumu
    * [ ] Enum je implicitne final. Nemôžeme z neho dediť
    * [ ] Všetky enumy dedia od `java.lang.Enum`
    * [ ] enum vieme použiť v podmienkach switch.
    * [ ] Kompilátor kontroluje, aby v podmienke switch boli všetky enum hodnoty
    * [ ] Enum môže obsahovať dodatočné atribúty a metódy
    * [ ] Objekty triedy `enum` vieme vytvárať z reťazcov pomocou `valueOf()`
    * [ ] Pomocou `name()` zistíme rextovú reprezentáciu enumu
- [x] Kedy použiť `enum`
    * [ ] Ak existuje pevná množina hodnôt
    * [ ] Ak počet hodnôt nie je veľký
    * [ ] Ak hodnoty nepotrebujú špeciálne dáta alebo správanie
    * [ ] Ak potrebujeme silnú typovú kontrolu
- [x] Nevýhody enumu
    * [ ] Zoznam hodnôt je pevne daný a nedá sa rozšíriť
    * [ ] Je pomalý ak máme veľké množstvo hodnôt (viac ako 1000)



!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    ENUMERAČNÉ TYPY

    Dátové typy s konečným počtom hodnôt

    - Stav objednávky: nová, zpracovávaná, expedovaná, doručená
    - Svetové strany: sever, juh, východ, západ
    - Veľkosť trička: XS, S, M, L, XL, XXL

    1. Enumeračné typy pomocou Stringov
    
    Ak je hodnôt pomerne veľa (cca viac ako 30)
    Ak hodnoty nevieme zoradiť
    Ak v budúcnosti môžu pribudnúť nové hodnoty
    Ak sú hodnoty rovnocenné, teda nemáme niektoré hodnoty, ktoré majú špeciálnu logiku alebo dáta
    Nevýhody: Pri zadávaní môže nastať preklep, nemáme žiadnu typovú kontrolu

    2. Pomocou číselných typov

    Ak hodnoty vieme zoradiť
    Ak je hodnôt veľké množstvo
    Ak hodnoty majú matematický význam
    Ak potrebujeme veľmi efektívne spracovanie (rýchlosť, málo miesta v pamäti)
    Ak majú hodnoty číselný štandard (napr. HTTP statusy, MIDI udalosti)
    Nevýhody: musíme štandardizovať číselnú hodnotu, nemáme žiadnu typovú kontrolu

    3. Pomocou tried a dedičnosti

    Ak hodnôt nie je veľa
    Ak je k hodnotám potrebné priradiť dodatočnú logiku alebo stav/dáta
    Nevýhody: Existuje viacero inštancií jednej hodnoty; Príliš veľa kódu, veľká zložitosť

    4. enum
    
    enum (enumeration) trieda - pre pevný, nemenný a obmedzený zoznam hodnôt
    public enum Pohlavie { MUZ, ZENA }
    Hodnoty sú statické konštanty, preto názvy sú veľkými písmenami.
    Zakázané vytvárať vlastné objekty enumu
    Všetky enumy dedia od java.lang.Enum
    enum vieme použiť v podmienkach switch, kompilátor kontroluje, aby v podmienke boli všetky enum hodnoty
    enum môže obsahovať dodatočné atribúty a metódy
    valueOf() - vytváranie enumu z reťazca
    name() - názov enumu vo forme reťazca

    Kedy použiť enum:

    - Ak existuje pevná množina hodnôt
    - Ak počet hodnôt nie je veľký
    - Ak hodnoty nepotrebujú špeciálne dáta alebo správanie
    - Ak potrebujeme silnú typovú kontrolu

    Nevýhody enumu
    - Zoznam hodnôt je pevne daný a nedá sa rozšíriť
    - Je pomalý ak máme veľké množstvo hodnôt (viac ako 1000)
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Čo sú enumeračné typy
    - Kedy je vhodné použiť reťacec - aj príklady
    - Kedy je vhodné použiť čísla - aj príklady
    - Kedy je vhodné použiť triedy a dedičnosť - aj príklady
    - Trieda enum - vlastnosti
    - Kedy je vhodné použiť enum
    - Výhody a nevýhody
    - Vytvorenie enumu z reťazca, získanie textovej reprezentácie hodnoty enumu
