# Teória 18: Records
 
Na dnešnej hodine si predstavíme Java records a vysvetlíme si aj metódy nájdenia a opravy chýb v našom kóde.

## Records

Records sú pomerne nová časť jazyka Java. Slúžia na uľahčenie písania tried, ktorých hlavným účelom je reprezentovať nejaké informácie.

Record je špeciálny typ triedy určený na nesenie dát (data carrier). Nahrádza triedy, ktoré majú privátne nemenné atribúty, gettery a slúžia na modelovanie nejakých dát.

Records odstraňuje prebytočný kód (anglicky boilerplate), prináša jednoduchosť a zvyšuje spoľahlivosť kódu.

=== "Základná syntax Records"

    ```java
    public record Osoba(String meno, String priezvisko,
                        Pohlavie pohlavie, LocalDate datumNarodenia) {}
    ```

Tento jediný riadok automaticky generuje:

- private final atribúty
- parametrický konštruktor
- gettre (bez get prefixu)
- metódy `equals()`, `hashCode()` a `toString()`

Kompilátor z recordu automaticky vygeneruje triedu, ktorá má približne takýto kód:

=== "Čo java vygeneruje z recordu Osoba"

    ```java
    public class Osoba extends java.lang.Record {

        private final String meno;
        private final String priezvisko;
        private final Pohlavie pohlavie;
        private final LocalDate datumNarodenia;

        public SimpleOsoba(String meno, String priezvisko,
                           Pohlavie pohlavie, LocalDate datumNarodenia) {
            this.meno = meno;
            this.priezvisko = priezvisko;
            this.pohlavie = pohlavie;
            this.datumNarodenia = datumNarodenia;
        }

        public String meno() {
            return meno;
        }

        public String priezvisko() {
            return priezvisko;
        }

        public Pohlavie pohlavie() {
            return pohlavie;
        }

        public LocalDate datumNarodenia() {
            return datumNarodenia;
        }

        @Override
        public boolean equals(Osoba other) {
            return this.meno.equals(other.meno) 
                && this.priezvisko.equals(other.priezvisko) 
                && this.pohlavie.equals(other.pohlavie) 
                && this.datumNarodenia.equals(other.datumNarodenia);
        }

        @Override
        public int hashCode() {
            return java.util.Objects.hash(meno, priezvisko, pohlavie, datumNarodenia);
        }

        @Override
        public String toString() {
            return "...výpis hodnôt";
        }


    }
    ```

### Vlastnosti recordov

Gettre v recordoch nezačínajú slovkom `get`

Recordy sú nemenné:

- všetky polia sú `final`
- neexistujú settre
- stav objektu sa po vytvorení nemení

Recordy sa nededia:

- record je definovaný ako `final`
- record dedí iba z triedy `java.lang.Record`
- record ale môže implementovať akékoľvek rozhrania

Do recordov môžete pridávať vlastné metódy a statické atribúty. Podporujú tiež vlastný konštruktor v tzv. kompaktom formáte, kedy nemusíme písať argumenty konštruktora:

```java
public record Interval(int od, int do) {
    // kompaktný konštruktor
    public Interval {
        if (od > do) throw new IllegalArgumentException("od nesmie byť väčší ako do");
    }

    public int dlzka() {
        return do - od + 1;
    }
}
```


### Podpora serializácie

Keďže recordy sú nemenné objekty, podpora serializácie (uloženie hodnôt objektu mimo program) je pomerne jednoduchá. Recordy taktiež dobre spolupracujú s knižnicou Jackson, ktorá umožňuje načítať hodnoty z JSON alebo CSV súborov priamo do recordov. Jednoduché mapovanie si nevyžaduje žiadne zmeny v kóde, avšak môžete použiť Jackson anotácie, ak je potrebné namapovať hodnoty na iné atribúty.

=== "Príklad použitia recordov pre načítanie JSON hodnôt"

    ```java
    @JsonPropertyOrder({"meno", "priezvisko", "titul", "pohlavie", "datumNarodenia"})
    public record RecordOsoba (@JsonProperty("meno") String meno,
                               @JsonProperty("priezvisko") String priezvisko,
                               @JsonProperty("titul") String titul,
                               @JsonProperty("pohlavie") Pohlavie pohlavie,
                               @JsonProperty("datumNarodenia") LocalDate datumNarodenia,
                               @JsonUnwrapped @JsonProperty("adresa") RecordAdresa adresa) {
       
        public String toString() {
            return meno + " " + priezvisko + ", " + datumNarodenia() + ": " + adresa;
        }
    }
    ```

## Oprava chýb

Pri písaní zdrojového kódu sa každému stane, že do kódu prinesie nejakú chybu. Ukážeme si typické prípady chýb a ako ich riešiť. 

### Chyby pri kompilácii

Veľké množstvo chýb nám dokáže odhaliť kompilátor a teda prídeme na nich ešte pred tým, ako sa program spustí.

Najčastejším zdrojom týchto chýb je kopírovanie kódu z webových stránok alebo z asistentov umelej inteligencie. 

Veľkým pomocníkom pri nájdení a oprave týchto chýb je IDE. Nástroj IntelliJ IDEA, ktorý používame v tomto predmete, nám identifikuje väčšinu kompilačných chýb a často ponúkne aj opravu.

Ak nevieme nájsť zdroj problému, odporúčame vykonať automatické formátovanie kódu (Code-Reformat Code) pomocou `Ctrl-Alt-L`. Ak máme niekde v kóde zlé vnorenie alebo nám chýba nejaká zátvorka, toto formátovanie nám pomôže nájsť chybu.

Typické kompilačné chyby:

- Cannot find symbol - Chýba import triedy alebo sme v názve urobili preklep
- class, interface, enum, or record expected - Máme niekde zátvorku navyše
- reached end of file while parsing - Niekde nám zátvorka chýba
- illegal start of expression - Chýbajúca zátvorka

### Chyby pri behu aplikácie

Java kompilátor nedokáže odhaliť všetky chyby. Niektoré sa ukážu až pri behu programu. V tom šťastnejšom prípade nám program pri chybe vyhodí výnimku a mi môžme začať skúmať, čo je príčinou. Štandardne program vypíše do konzoly tzv. stacktrace. Stacktrace je výpis volaní metód, pomocou ktorého vieme nájsť zdroj chyby.

V tom horšom prípade program nevyhodí výnimku, ale aj tak nefunguje správne, nereaguje podľa očakávaní, alebo nevyprodukuje požadovaný výstup. Tu sa už odporúča použiť debugovacie nástroje IDE, alebo pridať do kódu stručný výpis pomocou `System.out.println` na tie miesta, kde tušíme chybu. Pomocou takýchto výpisov si vieme odsledovať, či program na danom mieste naozaj robí to, čo od neho čakáme.

Medzi najčastejšie výnimky počas behu probramu patria NullPointerException a ClassCastException.

#### NullPointerException

Výnimka `NullPointerException` je spôsobená `null` hodnotou na mieste, kde sa očakáva objekt. Predísť takému typu chyby sa dá dôslednou kontrolou vstupov do metód. Často je zdrojom tejto výnimky vstup od užívateľa alebo zo súboru, ktorý má neočakávanú hodnotu. Príklad stacktrace pri takejto chybe:

```
Exception in thread "main" com.fasterxml.jackson.databind.JsonMappingException: Cannot invoke "String.toUpperCase()" because "this.priezvisko" is null (through reference chain: java.util.ArrayList[0]->sk.spse.jackson.Osoba["priezvisko"])
	at com.fasterxml.jackson.databind.JsonMappingException.wrapWithPath(JsonMappingException.java:400)
	at com.fasterxml.jackson.databind.JsonMappingException.wrapWithPath(JsonMappingException.java:359)
	at com.fasterxml.jackson.databind.ser.std.StdSerializer.wrapAndThrow(StdSerializer.java:324)
	at com.fasterxml.jackson.databind.ser.std.BeanSerializerBase.serializeFields(BeanSerializerBase.java:765)
	at com.fasterxml.jackson.databind.ser.BeanSerializer.serialize(BeanSerializer.java:183)
	at com.fasterxml.jackson.databind.ser.impl.IndexedListSerializer.serializeContents(IndexedListSerializer.java:119)
	at com.fasterxml.jackson.databind.ser.impl.IndexedListSerializer.serialize(IndexedListSerializer.java:79)
	at com.fasterxml.jackson.databind.ser.impl.IndexedListSerializer.serialize(IndexedListSerializer.java:18)
	at com.fasterxml.jackson.databind.ser.DefaultSerializerProvider._serialize(DefaultSerializerProvider.java:503)
	at com.fasterxml.jackson.databind.ser.DefaultSerializerProvider.serializeValue(DefaultSerializerProvider.java:342)
	at com.fasterxml.jackson.databind.ObjectMapper._writeValueAndClose(ObjectMapper.java:4926)
	at com.fasterxml.jackson.databind.ObjectMapper.writeValue(ObjectMapper.java:4088)
	at sk.spse.jackson.Json.main(Json.java:42)
Caused by: java.lang.NullPointerException: Cannot invoke "String.toUpperCase()" because "this.priezvisko" is null
	at sk.spse.jackson.Osoba.getPriezvisko(Osoba.java:50)
	at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at com.fasterxml.jackson.databind.ser.BeanPropertyWriter.serializeAsField(BeanPropertyWriter.java:688)
	at com.fasterxml.jackson.databind.ser.std.BeanSerializerBase.serializeFields(BeanSerializerBase.java:760)
	... 9 more
```

Pri hľadaní príčiny sa treba zamerať na:

- riadky, ktoré sú z našich tried (Osoba.java)
- prvé riadky začínajúce s `at`
- skutočná príčína je často v sekcii `Caused by`, ktorá obsahuje výpis vnorenej výnimky

#### ClassCastException

Ďalšou častou výnimkou je `ClassCastExeption`, ktorý vznikne pri downcastingu, teda pri pretypovaní s rodičovskej triedy na triedu potomka, pričom v skutočnosti objekt nie je daným potomkom. Príklad takejto výnimky:

```
Exception in thread "main" java.lang.ClassCastException: class sk.spse.jackson.Osoba cannot be cast to class sk.spse.jackson.Adresa (sk.spse.jackson.Osoba and sk.spse.jackson.Adresa are in unnamed module of loader 'app')
	at sk.spse.jackson.Json.main(Json.java:34)
```

### Ostatné typy výnimiek

Pri iných typoch výnimiek často pomôže dokumentácia. Po tom, čo nájdeme riadok v kóde, ktorý skutočne vyhodil výnimku, pozrieme sa na metódu a nájdeme si dokumentáciu k danej triede a metóde. Často v dokumentácii sa spomínajú výnimky, ktoré daná metóda môže vyhodiť a aj príčina. To nám môže veľmi pomôcť pri oprave danej chyby.

## Zhrnutie teórie

Všetky príklady uvedené na tejto hodine viete nájsť a vyskúšať v repozitári na adrese [https://github.com/wagjo/opg-jackson](https://github.com/wagjo/opg-jackson)

- [x] Records
    * [ ] Uľahčenie písania tried, ktorých hlavným účelom je reprezentovať nejaké informácie
    * [ ] Record je špeciálny typ triedy určený na nesenie dát (data carrier). Nahrádza triedy, ktoré majú privátne nemenné atribúty, gettery a slúžia na modelovanie nejakých dát.
    * [ ] `public record Osoba(String meno, String priezvisko, Pohlavie pohlavie, LocalDate datumNarodenia) {}`
    * [ ] Records odstraňuje prebytočný kód (anglicky boilerplate), prináša jednoduchosť a zvyšujú spoľahlivosť kódu.
- [x] Records automaticky generuje
    * [ ] private final atribúty
    * [ ] parametrický konštruktor
    * [ ] gettre (bez get prefixu)
    * [ ] metódy `equals()`, `hashCode()` a `toString()`
- [x] Recordy sú nemenné:
    * [ ] všetky polia sú final
    * [ ] neexistujú settre
    * [ ] stav objektu sa po vytvorení nemení
- [x] Recordy sa nededia:
    * [ ] record je definovaný ako final
    * [ ] record dedí iba z triedy java.lang.Record
    * [ ] record ale môže implementovať akékoľvek rozhrania
- [x] Chyby pri kompilácii
    * [ ] Najčastejším zdrojom týchto chýb je kopírovanie kódu z webových stránok alebo z asistentov umelej inteligencie. 
    * [ ] Automatické formátovanie kódu (Code-Reformat Code) pomocou Ctrl-Alt-L nám pomôže nájsť chybu.
    * [ ] Cannot find symbol - Chýba import triedy alebo sme v názve urobili preklep
    * [ ] class, interface, enum, or record expected - Máme niekde zátvorku navyše
    * [ ] reached end of file while parsing - Niekde nám zátvorka chýba
    * [ ] illegal start of expression - Chýbajúca zátvorka
- [x] Chyby pri behu aplikácie
    * [ ] Pri vyhodení výnimky program vypíše stacktrace
    * [ ] Stacktrace je výpis volaní metód, pomocou ktorého vieme nájsť zdroj chyby.
    * [ ] Niekedy nevyhodí výnimku, ale aj tak nefunguje správne, nereaguje podľa očakávaní, alebo nevyprodukuje požadovaný výstup
    * [ ] Vtedy sa odporúča použiť debugovacie nástroje IDE, alebo pridať do kódu stručný výpis pomocou System.out.println na tie miesta, kde tušíme chybu
    * [ ] Medzi najčastejšie výnimky počas behu probramu patria NullPointerException a ClassCastException.
    * [ ] Často v dokumentácii sa spomínajú výnimky, ktoré daná metóda môže vyhodiť a aj príčina. 
- [x] Hľadanie príčine v stacktrace
    * [ ] riadky, ktoré sú z našich tried (`Osoba.java`)
    * [ ] prvé riadky začínajúce s `at`
    * [ ] skutočná príčína je často v sekcii `Caused by`, ktorá obsahuje výpis vnorenej výnimky

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    RECORDS

    Record je špeciálny typ triedy určený na nesenie dát (data carrier)

    public record Osoba(String meno, String priezvisko, Pohlavie pohlavie, LocalDate datumNarodenia) {}
    
    Records odstraňuje prebytočný kód (anglicky boilerplate), prináša jednoduchosť a zvyšujú spoľahlivosť kódu.    

    Records automaticky generuje
    - private final atribúty
    - parametrický konštruktor
    - gettre (bez get prefixu)
    - metódy equals(), hashCode() a toString()

    Recordy sú nemenné:
    - všetky polia sú final
    - neexistujú settre
    - stav objektu sa po vytvorení nemení

    Recordy sa nededia:
    - record je definovaný ako final
    - record dedí iba z triedy java.lang.Record
    - record ale môže implementovať akékoľvek rozhrania

    Chyby pri kompilácii
    - Najčastejším zdrojom týchto chýb je kopírovanie kódu z webových stránok alebo z umelej inteligencie. 
    - (Code-Reformat Code) pomocou Ctrl-Alt-L nám pomôže nájsť chybu.

    Chyby pri behu aplikácie
    - Pri vyhodení výnimky program vypíše stacktrace
    - Stacktrace je výpis volaní metód, pomocou ktorého vieme nájsť zdroj chyby.
    - Medzi najčastejšie výnimky počas behu probramu patria NullPointerException a ClassCastException.
    - Často v dokumentácii sa spomínajú výnimky, ktoré daná metóda môže vyhodiť a aj príčina. 
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Čo je record, na čo slúži
    - Vlastnosti record
    - Obmedzenia record
    - Chyby pri kompilácii, ako ich riešiť
    - Chyby pri behu programu a ako ich riešiť
