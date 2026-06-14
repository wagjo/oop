# Teória 28: ORM

Keď programujeme aplikáciu, ktorá potrebuje ukladať dáta, najčastejšie používame relačné databázy. V Jave však pracujeme s objektmi – vytvárame inštancie tried, voláme metódy, využívame dedičnosť a zapuzdrenie. Tieto dva svety – svet objektov a svet tabuliek – sú odlišné. Práve tu nastupuje **ORM** (Object-Relational Mapping).

!!! warning

    Dnešné podklady aj prezentácia sú celé vygenerované pomocou Umelej Inteligencie.

    ![](../assets/end.jpeg)

## ORM

ORM je technika, ktorá umožňuje programátorovi pracovať s databázou tak, akoby išlo o bežné objekty v Jave. Namiesto toho, aby sme ručne prevádzali objekty na SQL príkazy a výsledky dotazov späť na objekty, túto prácu preberie ORM knižnica. V praxi to znamená, že môžeme písať kód ako:

```java
Kniha k = new Kniha("Duna", "Frank Herbert", 1965);
session.persist(k);
```

a ORM sa postará o vygenerovanie správneho `INSERT` príkazu a jeho vykonanie.

Tento prístup rieši jeden z dlhodobých problémov v softvérovom vývoji, ktorý sa nazýva **Object-Relational Impedance Mismatch** – nesúlad medzi objektovým a relačným modelom. Objekty v Jave môžu mať zložité vzťahy, dedičnosť alebo kolekcie, zatiaľ čo relačné databázy pracujú so stĺpcami, riadkami a cudzími kľúčmi. ORM sa snaží tento nesúlad prekonať čo najpohodlnejším spôsobom.

### Prečo nestačí obyčajné JDBC?

JDBC je základný spôsob, ako sa v Jave pripojiť k databáze. Je to nízkoúrovňové API a dáva programátorovi veľkú kontrolu. Pri malých projektoch alebo pri veľmi jednoduchých operáciách je JDBC úplne postačujúce. Pri väčších aplikáciách sa však rýchlo ukážu jeho limity.

Pri použití JDBC musíme pre každú operáciu napísať pomerne veľké množstvo kódu. Musíme vytvoriť `PreparedStatement`, nastaviť parametre, vykonať dotaz, prejsť cez `ResultSet` a ručne naplniť objekty. Tento kód sa veľmi opakuje pri každej entite. Navyše, ak sa zmení štruktúra tabuľky v databáze, musíme manuálne upraviť mapovanie na viacerých miestach v kóde.

Ďalším problémom je, že pri JDBC programátor často premýšľa v pojmoch databázy (tabuľky, stĺpce, joiny), namiesto toho, aby premýšľal v pojmoch domény problému (Kniha, Čitateľ, Výpožička). ORM tento rozdiel výrazne zmenšuje.

## Ako ORM funguje v praxi

ORM knižnica (v našom prípade Hibernate) plní tri hlavné úlohy.

1. **Mapovanie**. Pomocou anotácií v kóde definujeme, ktorá trieda v Jave zodpovedá ktorej tabuľke v databáze a ktoré pole zodpovedá ktorému stĺpcu. Táto definícia sa robí raz a potom ju ORM využíva automaticky.

1. **CRUD operácie**. Namiesto písania `INSERT`, `SELECT`, `UPDATE` a `DELETE` voláme metódy ako `persist()`, `find()`, `merge()` alebo `remove()`. ORM sa postará o vygenerovanie správneho SQL a jeho vykonanie.

1. **Podpora dotazov**. Aj keď môžeme písať klasické SQL, ORM nám ponúka vyšší dotazovací jazyk (HQL alebo JPQL), v ktorom pracujeme s názvami tried a ich atribútov namiesto názvov tabuliek a stĺpcov. To robí dotazy čitateľnejšie a menej závislé od konkrétnej databázy.

## Hibernate a štandard JPA

V Jave existuje štandard na prácu s ORM, ktorý sa nazýva **Jakarta Persistence** (predtým Java Persistence API). Tento štandard definuje, ako by malo ORM fungovať, aké anotácie by sa mali používať a aké rozhrania by malo poskytovať.

**Hibernate** je najrozšírenejšia a najpoužívanejšia implementácia tohto štandardu. To znamená, že keď píšeme kód podľa pravidiel JPA, môžeme v budúcnosti pomerne jednoducho vymeniť Hibernate za inú implementáciu, aj keď sa to v praxi stáva len zriedka.

Od verzie Hibernate 6 a vyššie sa používa balíček `jakarta.persistence.*`. Staršie kódy často používajú `javax.persistence.*`, ktorý je už zastaraný.

### Základné stavebné prvky

Pri práci s Hibernate (alebo akýmkoľvek ORM) sa stretneme s niekoľkými kľúčovými pojmami.

- **Entita** je bežná trieda v Jave, ktorá je pomocou anotácií označená ako niečo, čo sa má ukladať do databázy. Entita reprezentuje jeden riadok v tabuľke. Dôležité je, že entita musí mať prázdny konštruktor a identifikátor (primárny kľúč).

- **Session** (alebo v JPA `EntityManager`) je objekt, cez ktorý komunikujeme s databázou. Reprezentuje akési „pracovné prostredie“ alebo kontext, v ktorom sa nachádzajú naše entity. Session má pomerne krátku životnosť – zvyčajne ju vytvoríme na začiatku nejakej logickej operácie a na konci ju uzavrieme.

- **SessionFactory** je továreň na Session. Vytvára sa zvyčajne raz na začiatku aplikácie a je pomerne „ťažkým“ objektom. Jej úlohou je držať konfiguráciu a vedieť vytvárať nové Session podľa potreby.

- **Transakcia** zabezpečuje, že všetky zmeny v databáze sa vykonajú ako jeden celok. Buď sa všetky zmeny úspešne uložia (`commit`), alebo sa pri akejkoľvek chybe všetko vráti do pôvodného stavu (`rollback`). Bez transakcie Hibernate väčšinou odmietne zapisovať dáta do databázy.

### Mapovanie entít pomocou anotácií

Najbežnejší spôsob, ako v Jave definovať, ako sa má trieda mapovať do databázy, je pomocou anotácií. Základná entita môže vyzerať napríklad takto:

```java
@Entity
@Table(name = "knihy")
public class Kniha {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "nazov", nullable = false)
    private String nazov;

    private String autor;

    @Column(name = "rok_vydania")
    private Integer rokVydania;

    public Kniha() { }

    public Kniha(String nazov, String autor, Integer rokVydania) {
        this.nazov = nazov;
        this.autor = autor;
        this.rokVydania = rokVydania;
    }

    // gettery a settery
}
```

Anotácia `@Entity` hovorí Hibernate, že táto trieda má byť mapovaná do databázy. `@Table` umožňuje nastaviť názov tabuľky. `@Id` označuje primárny kľúč a `@GeneratedValue` s stratégiou `IDENTITY` hovorí, že hodnotu ID bude generovať samotná databáza (vhodné pre SQLite aj MySQL).

### Práca so Session a transakciami

Typický spôsob práce s Hibernate vyzerá nasledovne:

```java
try (Session session = sessionFactory.openSession()) {
    Transaction tx = session.beginTransaction();

    Kniha k = new Kniha("1984", "George Orwell", 1949);
    session.persist(k);

    tx.commit();
}
```

Použitie `try-with-resources` je odporúčané, pretože Session by mala byť vždy správne uzavretá. Transakcia je dôležitá najmä pri zapisovaní dát. Ak by sme `persist()` zavolali mimo transakcie, Hibernate by vyhodil výnimku.

Po zavolaní `persist()` sa objekt stane tzv. **managed** objektom. Od tohto momentu Hibernate sleduje zmeny v tomto objekte a pri commite transakcie automaticky vygeneruje potrebné `UPDATE` príkazy (tzv. dirty checking).

### Základné operácie s dátami

ORM výrazne zjednodušuje základné operácie s dátami. Na uloženie nového záznamu stačí zavolať `persist()`. Na načítanie podľa primárneho kľúča použijeme `find()`. Na aktualizáciu stačí zmeniť hodnoty v objekte a nechať transakciu sa commitnúť. Na vymazanie slúži metóda `remove()`.

Tieto operácie sú vysoko abstraktné. Programátor nemusí vedieť, aký presný SQL príkaz sa vygeneruje – ORM sa o to postará samo.

```java
// Create
session.persist(kniha);           // odporúčané v Hibernate 6+

// Read
Kniha k = session.find(Kniha.class, 5L);

// Update
k.setNazov("Nový názov");
session.merge(k);                 // alebo len commit

// Delete
session.remove(k);
```

### Dotazy v HQL a JPQL

Aj keď ORM generuje veľa SQL automaticky, často potrebujeme písať vlastné dotazy. Na tento účel slúži **JPQL** (Jakarta Persistence Query Language) alebo jeho Hibernate verzia **HQL**.

Namiesto písania klasického SQL pracujeme s názvami tried a ich polí:

```java
List<Kniha> knihy = session.createQuery(
    "SELECT k FROM Kniha k WHERE k.autor = :autor", Kniha.class)
    .setParameter("autor", "George Orwell")
    .getResultList();
```

Tento prístup má niekoľko výhod. Dotaz je čitateľnejší, je menej závislý od konkrétnej databázy a Hibernate ho vie preložiť do natívneho SQL podľa používaného dialektu.

## Kedy použiť ORM a kedy radšej JDBC?


Nie je správne tvrdiť, že ORM je vždy lepšie ako JDBC. Oba prístupy majú svoje miesto.

JDBC dáva väčšiu kontrolu a je vhodné v situáciách, kde potrebujeme maximálny výkon alebo pracujeme s veľmi zložitými a optimalizovanými dotazmi. Je tiež dobré na začiatok učenia, pretože núti študenta pochopiť, čo sa v databáze skutočne deje.

ORM naopak výrazne zvyšuje produktivitu pri vývoji väčších aplikácií. Väčšina dnešných projektov v Jave používa práve ORM, pretože ušetrený čas a lepšia udržateľnosť kódu prevažujú nad drobnými nevýhodami.

V praxi sa často stretávame aj s kombináciou oboch prístupov – ORM sa používa na väčšinu operácií a JDBC sa využíva len na špecifické, výkonnostne kritické miesta.

V praxi dnes väčšina tímov používa **ORM** a JDBC ponecháva na špecifické prípady.

| Situácia                              | Odporúčanie     | Dôvod |
|---------------------------------------|------------------|-------|
| Veľmi jednoduché operácie, maximálny výkon | JDBC            | Menej overheadu |
| Väčší projekt s mnohými entitami      | ORM             | Výrazne menej kódu |
| Potreba veľmi zložitých optimalizovaných dotazov | JDBC + ORM     | Kombinácia |
| Začiatočník, ktorý sa učí základy     | Najprv JDBC     | Lepšie pochopenie princípov |
| Reálny projekt v roku 2026            | ORM (Hibernate / Spring Data JPA) | Produktivita |

## Zhrnutie

ORM predstavuje vyššiu úroveň abstrakcie pri práci s databázami v Jave. Namiesto toho, aby sme ručne prevádzali objekty na SQL a späť, necháme túto prácu na knižnicu, ktorá sa o všetko postará. Najpoužívanejším ORM v Jave je Hibernate, ktorý implementuje štandard Jakarta Persistence.

Základnými prvkami, s ktorými pracujeme, sú entity (mapované triedy), Session (alebo EntityManager) a transakcie. Pomocou anotácií definujeme mapovanie medzi triedami a tabuľkami a následne môžeme jednoducho ukladať, načítavať a upravovať dáta pomocou objektov.

Aj keď ORM nie je všeliek a v niektorých prípadoch je vhodnejšie použiť priame JDBC, v drvivej väčšine reálnych projektov dnes ORM výrazne zjednodušuje vývoj a údržbu aplikácií.
