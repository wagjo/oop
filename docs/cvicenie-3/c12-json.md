# Cvičenie 12: Práca s JSON a CSV

Na tomto cvičení pokračujete v práci na polročnom projekte. Okrem toho si ukážeme, ako sa v Jave pracuje s JSON a CSV súbormi

## Jackson

Na prácu s JSON a CSV súbormi je v Jave najpopulárnejšia knižnica Jackson. Hlavná stránka tejto knižnice je [https://github.com/FasterXML/jackson](https://github.com/FasterXML/jackson)

Táto knižnica má viacero častí, my budeme používať týchto 5:

- **jackson-core**: Hlavný modul poskytujúci základnú funkcionalitu (JsonParser a JsonGenerator) pre rýchle čítanie a zápis JSONu bez závislosti na mapovaní objektov.

- **jackson-annotations**: Obsahuje anotácie (napr. @JsonProperty, @JsonIgnore, @JsonInclude), ktoré slúžia na konfiguráciu serializácie a deserializácie Java objektov do/z JSONu.

- **jackson-databind**: Najpoužívanejší vysokourovňový modul (ObjectMapper), ktorý umožňuje jednoduchú konverziu medzi Java objektmi (POJO) a JSONom pomocou reflexie a anotácií (závisí na core a annotations).

- **jackson-dataformat-csv**: Modul pre prácu s formátom CSV. Ak pracujeme iba s JSON, tento modul nie je potrebný

- **jackson-datatype-jsr310**: Podpora pre Java 8 Time API (java.time – LocalDateTime, Instant atď.) pri serializácii/deserializácii.

### Pridanie knižníc do projektu

V IntelliJ IDEA projekte si tieto knižnice vieme pridať nasledovným spôsobom:

1. Otvoríme File -> Project Structure a v ňom časť Libraries

1. Klikneme na ikonku + a vyberieme možnosť "From Maven"

1. Pridáme knižnicu `com.fasterxml.jackson.core:jackson-databind:2.20.1`. 

1. Potom pridáme knižnicu `com.fasterxml.jackson.dataformat:jackson-dataformat-csv:2.20.1`

1. Nakoniec pridáme knižnicu `com.fasterxml.jackson.datatype:jackson-datatype-jsr310:2.20.1`

V zozoname sa nám automaticky objavia aj knižnice jackson-core a jackson-annotations. To je preto, lebo knižnica jackson-databind má v sebe uvedenú závislosť na týchto knižniciach a teda IntelliJ IDEA stiahne a importuje aj tie ostatné.

## Príklad - Osoba

Vyskúšame si načítať JSON, ktorý obsahuje informácie o osobách. Každá osoba má nasledovné informácie:

- Meno
- Priezvisko
- Titul (voliteľný)
- Pohlavie (muž/žena)
- Dátum narodenia (formát ISO 8601, YYY-MM-DD)

=== "Príklad JSON súboru s osobami"

```json title="osoby1.json"
[
  {
    "meno": "Ján",
    "priezvisko": "Novák",
    "titul": "Ing.",
    "pohlavie": "MUZ",
    "datumNarodenia": "1985-03-12"
  },
  {
    "meno": "Martina",
    "priezvisko": "Kováčová",
    "titul": null,
    "pohlavie": "ZENA",
    "datumNarodenia": "1992-07-25"
  }
]
```

## Dátový model v Jave

Jackson nám ponúka viacero spôsobov, ako načítať a zapisovať dáta do JSON súboru. Ukážeme si jednoduchý postup pomocou anotácií. Na začiatku ne potrebné namapovať atribúty z JSON súboru na atribúty triedy. To urobíme pomocou anotácií v konštruktore (inak by sme museli mať setter metódy)

=== "Dátový model s Jackson anotáciami"

```java
package sk.spse.jackson;

import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonPropertyOrder;

import java.time.LocalDate;

public enum Pohlavie {
    MUZ,
    ZENA
}

@JsonPropertyOrder({"meno", "priezvisko", "titul", "pohlavie","datumNarodenia"})
public class SimpleOsoba {

    private final String meno;
    private final String priezvisko;
    private final String titul;
    private final Pohlavie pohlavie;
    private final LocalDate datumNarodenia;

    @JsonCreator  // Jackson tento konštruktor použije pri vytváraní objektu
    public SimpleOsoba(
            // mapovanie JSON atribútov na atribúty triedy
            @JsonProperty("meno") String meno,
            @JsonProperty("priezvisko") String priezvisko,
            @JsonProperty("titul") String titul,
            // Jackson automaticky skovertuje na enum alebo LocalDate
            @JsonProperty("pohlavie") Pohlavie pohlavie,
            @JsonProperty("datumNarodenia") LocalDate datumNarodenia
    ) {
        this.meno = meno;
        this.priezvisko = priezvisko;
        this.titul = titul;
        this.pohlavie = pohlavie;
        this.datumNarodenia = datumNarodenia;
    }

    public String getMeno() {
        return meno;
    }

    public String getPriezvisko() {
        return priezvisko;
    }

    public String getTitul() {
        return titul;
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

Využili sme 3 typy anotácií:

- **JsonProperty** - mapovanie atribútu
- **JsonCreator** - označí konštruktor, ktorý sa má použiť pri práci s JSON súborom
- **JsonPropertyOrder** - definuje poradie atribútov pri zapisovaní do JSON súboru

## Práca s JSON formátom

Pre načítanie dát zo súboru je potrebné vytvoriť Jackson mapper objekt a vhodne ho nakonfigurovať. Nasledujúci príklad ukazuje načítanie dát.

```java
// Hlavný objekt pre Jackson
ObjectMapper mapper = new ObjectMapper();
// Pridanie podpory pre java.time (LocalDate)
mapper.registerModule(new JavaTimeModule());

File file = new File("assets/osoby1.json");

// Parsovanie sa vykoná tu
List<SimpleOsoba> osoby = mapper.readValue(
        file,
        // Tu povieme Jacksonu akú štruktúru má json,
        // teda zoznam osôb - List v ktorom sú objekty SimpleOsoba
        new TypeReference<List<SimpleOsoba>>() {}
);
```

Výsledné dáta sú uložené v premennej osoby.

Zapísanie dát do JSON súboru sa robí podobne. Konfigurácia je mierne odlišná. Príklad:

```java
// Hlavný objekt pre Jackson
ObjectMapper mapper = new ObjectMapper();
// Pridanie podpory pre java.time (LocalDate)
mapper.registerModule(new JavaTimeModule());

// pekné odsadenie (pretty print)
mapper.enable(SerializationFeature.INDENT_OUTPUT);
// Chceme zapísať dátum v ISO formáte
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
// Zapísanie dát do súboru
File output = new File("out/osoby1.json");
mapper.writeValue(output,osoby);
```

## Práca s CSV formátom

Pomocou Jackson knižnice vieme pracovať aj s CSV formátom. 

```csv title="osoby1.csv"
meno,priezvisko,titul,pohlavie,datumNarodenia
Ján,Novák,Ing.,MUZ,1985-03-12
Martina,Kováčová,,ZENA,1992-07-25
Peter,Horváth,Mgr.,MUZ,1978-11-04
Lucia,Tóthová,,ZENA,2000-01-18
Marek,Varga,,MUZ,1995-09-30
```

Dátový model a anotácie ostávajú nezmenené, iný je len kód na čítanie a zápis. Príklad čítania zo spboru:

```java
// Hlavný objekt pre Jackson
CsvMapper mapper = new CsvMapper();
// Pridanie podpory pre java.time (LocalDate)
mapper.registerModule(new JavaTimeModule());

File file = new File("assets/osoby1.csv");

// Definovanie schémy podľa hlavičky CSV
// Jackson použije názvy stĺpcov z hlavičky CSV na mapovanie
CsvSchema schema = CsvSchema.emptySchema().withHeader();

List<SimpleOsoba> osoby = null;

// Načítanie z CSV súboru
try (MappingIterator<SimpleOsoba> it = mapper.readerFor(SimpleOsoba.class)
        .with(schema)
        .readValues(file)) {

    osoby = it.readAll();

}
```

Zapísanie do CSV súboru:

```java
// Hlavný objekt pre Jackson
CsvMapper mapper = new CsvMapper();
// Pridanie podpory pre java.time (LocalDate)
mapper.registerModule(new JavaTimeModule());

// Chceme zapísať dátum v ISO formáte
mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

// Definovanie CSV schémy pre zápis
CsvSchema schemaOut = mapper
        .schemaFor(SimpleOsoba.class)
        .withHeader(); // pridať riadok s hlavičkou CSV

// Zápis CSV súboru
File output = new File("out/osoby1.csv");
mapper.writer(schemaOut).writeValue(output, osoby);
```

## Projekt s príkladmi práce s JSON a CSV

Všetky príklady uvedené na tomto cvičení viete nájsť v projekte opg-jackson na adrese [https://github.com/wagjo/opg-jackson](https://github.com/wagjo/opg-jackson). V tomto projekte sú tiež uvedené príklady pre mierne zložitejšie spracovanie dát, kedy máme dátový model zložený z viacerých tried.