# Teória 10: Zapuzdrenie

Dnes začíname preberať hlavné koncepty objektovo orientovaného programovania. Začneme pri zapuzdrení.

## Zapuzdrenie

Zapuzdrenie je **zabalenie dát (atribútov) a metód do jednej komponenty** - objektu.

Zapuzdrenie (anglicky *encapsulation*) je jeden zo základných princípov objektovo orientovaného programovania. V Jave sa zapuzdrenie používa na skrytie interných detailov objektu a ochranu jeho dát pred neoprávneným prístupom alebo modifikáciou.

Zapuzdrenie umožňuje:

- Zvýšiť bezpečnosť
- Zlepšiť modularitu 
- Jednoduchšiu udržiavateľnosť kódu

Hlavné ciele zapuzdrenia:

- Ochrana dát: Zamedzenie priameho prístupu k interným premenným triedy.
- Abstrakcia:  Vonkajší kód nemusí poznať vnútornú štruktúru triedy, iba jej verejné rozhranie (verejné metódy).
- Flexibilita: Zmena internej implementácie bez ovplyvnenia vonkajšieho kódu.

=== "Príklad zapuzdrenia atribútov"

    ```java
    public class Osoba {
        private String meno; // súkromný atribút

        // Getter
        public String getMeno() {
            return meno;
        }

        // Setter
        public void setMeno(String meno) {
            if(meno != null && !meno.isEmpty()) // validácia vstupu
                this.meno = meno;
        }
    }
    ```

    Použitie:

    ```java
    public class Main {
        public static void main(String[] args) {
            Osoba osoba = new Osoba();
            osoba.setMeno("Jano");
            System.out.println("Meno: " + osoba.getMeno());
        }
    }
    ```

### Techniky zapuzdrenia

Zapuzdrenie v Jave sa dosahuje kombináciou:

- Prístupových modifikátorov (private, protected, public, prípadne defaultný prístup).
- Getterov a settrov (metódy na čítanie a zápis dát).
- Skrytím implementačných detailov (interné dáta triedy sú prístupné iba cez definované rozhranie - verejné metódy)

Pokročilejšie zapuzdrenie a väčšiu kontrolu nad stavom objektov vieme dosiahnuť tiež použitím privátnych konštruktorov a továrenských metód.

## Invarianty

Zapuzdrenie nám umožňuje vykonávať validáciu vstupných hodnôt a udržiavať **invarianty** objektu.

Invariant (alebo triedny invariant) je podmienka alebo vlastnosť, ktorá musí byť pravdivá pre objekt počas jeho celého života.

Triedny invariant:

- platí po každom skončení konštruktora
- platí pred aj po každom verejnom volaní metódy
- môže byť dočasne porušený vo vnútri metódy, ale musí byť obnovený pred jej ukončením

Medzi príklady invariantov patrí napr. že meno osoby musí byť neprázdne alebo dĺžka strany obdlžníka musí byť kladná.

V nasledujúcom príklade si zadefinujme invariant ako podmienku, že bankový účet musí mať nenulový zostatok. Všetky verejné metódy musia zaručiť, že sa invariant neporuší.

=== "Zapuzdrenie udržiava invarianty objektu"

    ```java
    public class BankovyUcet {
        private double dostatok;
        private String cisloUctu;

        public BankovyUcet(String cisloUctu, double pociatocnyZustatok) {
            this.cisloUctu = cisloUctu;
            if (pociatocnyZostatok >= 0) {
                this.zostatok = pociatocnyZostatok;
            } else {
                throw new IllegalArgumentException("Počiatočný zostatok nemôže byť záporný!");
            }
        }

        public void vloz(double suma) {
            if (suma > 0) {
                zostatok += suma;
            } else {
                throw new IllegalArgumentException("Suma musí byť kladná!");
            }
        }

        public void vyber(double suma) {
            if (suma > 0 && suma <= zostatok) {
                zostatok -= suma;
            } else {
                throw new IllegalArgumentException("Neplatná suma alebo málo prostriedkov!");
            }
        }

        public double getZustatok() {
            return zostatok;
        }

        public String getCisloUctu() {
            return cisloUctu;
        }
    }
    ```

Zapuzdrenie chráni invariant pred porušením zvonka. Objekt tak zostáva v konzistentnom stave. Vďaka zapuzdreniu ho nikto nemôže porušiť priamym prístupom k atribútom. 

## Nemennosť

Zapuzdrenie umožňuje vytvárať nemenné (immutable) triedy, kde po vytvorení objektu nemožno meniť jeho stav.

Nemenný objekt je objekt, ktorého stav sa po vytvorení už nikdy nezmení.

- všetky jeho polia sú nastavené len raz (v konštruktore),
- a potom už žiadna metóda nemôže tieto hodnoty zmeni

Takýto objekt je bezpečný na zdieľanie a vždy ostáva v konzistentnom stave, teda jeho invarianty sa nikdy neporušia.

Nemenný objekt môžeme dosiahnuť nasledovne:

- Atribúty nemenného objektu budú konštanty (použitím kľúčového slova `final`)
- Trieda nebude poskytovať žiadne setter metódy, alebo metódy, ktoré menia stav objektu

Nemenná trieda je veľmi užitočná vec a uľahčuje návrh architektúry programu. Jednoduchšie sa testuje a používa. V Jave poznáme veľa nemenných tried, napr. `java.lang.String`, `java.time.LocalDate` atď.

=== "Príklad nemennej triedy"

    ```java
    public final class Auto {
        private final String znacka;
        private final int rok;

        public Auto(String znacka, int rok) {
            this.znacka = znacka;
            this.rok = rok;
        }

        public String getZnacka() {
            return znacka;
        }

        public int getRok() {
            return rok;
        }
    }
    ```

## Defenzívne kopírovanie

Defenzívne kopírovanie je technika, ktorá rieši problém s únikom referencií z rozhrania triedy, napr. z jej getter metód.

Ak getter vracia referenciu na mutable objekt (napr. pole), vonkajší kód ho môže zmeniť.

=== "Únik referencie"

    ```java
    public class Trieda {
        private String[] ziaci = new String[30];

        // Konštruktor
        public Trieda(String... ziaci) {
            // Invariant triedy
            for (var ziak: ziaci)
                Objects.requireNonNull(ziak, "Ziak je null!");
            this.ziaci = ziaci;
        }

        // Zlý getter - vracia priamu referenciu
        public String[] getZiaci() {
            return ziaci;
        }
    }
    ```

V tomto prípade getter metóda vrátila referenciu na vnútorný objekt, ktorý je meniteľný a vonkajší kód tak vie zmeniť jeho hodnoty a porušiť invarianty triedy.

=== "Zmena vnútorného atribútu objektu"

    ```java
    Trieda trieda = new Trieda("Fero", "Jano", "Beatka");
    String ziaci = trieda.getZiaci();
    ziaci[0] = null; // invariant porušený
    ```

Riešením je defenzívne kopírovanie, kde **vraciame vždy kópiu vnútornej hodnoty** objektu

=== "Defenzívne kopírovanie v getter metóde"

    ```java
    public String[] getZiaci() {
        return ziaci.clone();
    }
    ```

Problém s únikom referencií sa dá riešiť aj použitím nemenných objektov. Ak aj v nejakej metóde vrátime vnútorný objekt, ak je nemenný, vonkajší kód nemôže nijako zmeniť jeho hodnoty a porušiť tak invarianty našej triedy.


## Zhrnutie teórie

- [x] Zapuzdrenie
    * [ ] Zabalenie dát (atribútov) a metód do jednej komponenty - objektu
    * [ ] Používa sa na skrytie interných detailov objektu a ochranu jeho dát pred neoprávneným prístupom alebo modifikáciou.
- [x] Zapuzdrenie umožňuje
    * [ ] Zvýšiť bezpečnosť
    * [ ] Zlepšiť modularitu
    * [ ] Jednoduchšiu udržiavateľnosť kódu
- [x] Hlavné ciele zapuzdrenia:
    * [ ] Ochrana dát: Zamedzenie priameho prístupu k interným premenným triedy.
    * [ ] Abstrakcia: Vonkajší kód nemusí poznať vnútornú štruktúru triedy, iba jej verejné rozhranie (verejné metódy).
    * [ ] Flexibilita: Zmena internej implementácie bez ovplyvnenia vonkajšieho kódu.
- [x] Techniky zapuzdrenia
    * [ ] Prístupové modifikátory (private, protected, public, prípadne defaultný prístup).
    * [ ] Getter a setter metódy (metódy na čítanie a zápis dát).
    * [ ] Skrytie implementačných detailov (interné dáta triedy sú prístupné iba cez definované rozhranie - verejné metódy)
    * [ ] Privátne konštruktory a továrenske metódy
- [x] Invariant - podmienka alebo vlastnosť, ktorá musí byť pravdivá pre objekt počas jeho celého života.
    * [ ] Platí po každom skončení konštruktora
    * [ ] Platí pred aj po každom verejnom volaní metódy
    * [ ] Môže byť dočasne porušený vo vnútri metódy, ale musí byť obnovený pred jej ukončením
    * [ ] Zapuzdrenie chráni invariant pred porušením zvonka. Objekt tak zostáva v konzistentnom stave.
- [x] Nemennosť - immutability - objekt, ktorého stav sa po vytvorení už nikdy nezmení
    * [ ] všetky jeho polia sú nastavené len raz (v konštruktore),
    * [ ] a potom už žiadna metóda nemôže tieto hodnoty zmeni
    * [ ] V nemennej triede sa používajú sa konštanty alebo sa neposkytnú setter metódy alebo iné metódy meniace stav
- [x] Defenzívne kopírovanie
    * [ ] Getter metóda môže spôsobiť únik referencie na meniteľný vnútorný objekt
    * [ ] Riešením je defenzívne kopírovanie, kde vraciame vždy kópiu vnútornej hodnoty objektu


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    ZAPUZDRENIE

    Zabalenie dát (atribútov) a metód do jednej komponenty - objektu

    - Zvýšuje bezpečnosť
    - Zlepšuje modularitu
    - Jednoduchšie sa udržiava kód

    Hlavné ciele zapuzdrenia sú ochrana dát, abstrakcia a flexibilita

    Techniky zapuzdrenia
    - Modifikátory prístupu
    - Getter a setter metódy
    - Skrytie implementačných detailov do súkromných metód

    Invariant - podmienka alebo vlastnosť, ktorá musí byť pravdivá pre objekt počas jeho celého života.
    - Platí po každom skončení konštruktora
    - Platí pred aj po každom verejnom volaní metódy
    - Môže byť dočasne porušený vo vnútri metódy, ale musí byť obnovený pred jej ukončením

    Nemenný - immutable - objekt, ktorého stav sa po vytvorení už nikdy nezmení
    V nemennej triede sa používajú konštanty alebo sa neposkytnú setter metódy

    Defenzívne kopírovanie
    - Getter metóda môže spôsobiť únik vnútorného objektu
    - Riešením je vrátiť vždy kópiu vnútornej hodnoty objektu
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Zapuzdrenie - čo to je
    - Techniky zapuzdrenia
    - Invariant
    - Nemenný objekt
    - Defenzívne kopírovanie
