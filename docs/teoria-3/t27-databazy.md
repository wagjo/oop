# Teória 27: Databázy

Databázy patria medzi najdôležitejšie technológie pri vývoji moderných aplikácií. Umožňujú trvalé uchovávanie údajov, ich efektívne vyhľadávanie, úpravu a správu. Príkladmi údajov ukladaných do databázy sú:

- používateľské účty, objednávky v e-shope,
- knižničné katalógy,
- výsledky meraní, údaje zo senzorov.

## Prečo databázy v Jave?

Väčšina reálnych Java aplikácií potrebuje perzistenciu dát. Hlavné dôvody sú:

- Dlhodobé uloženie dát (aj po reštarte aplikácie)
- Súbežný prístup viacerých používateľov/procesov
- ACID vlastnosti *(Atomicity, Consistency, Isolation, Durability)*
- Výkonné dotazovanie a reporting

*ACID* je skratka pre 4 vlastnosti databáz:

- **Atomicita** - Viaceré zmeny v databáza sa vykonajú ako jedna tranzakcia, buď sa vykonajú všetky, alebo ani jedna
- **Konzistencia** - Zmeny v databáze neporušia databázový invariant, napr. nepovolenie duplicitných hodnôt
- **Izolácia** - Pri súčasnom prístupe k databáze od viacerých užívateľov títo nebudú nikdy vidieť nedokončené zmeny od ostatných
- **Durability** - Po úspešnom prevedení zmeny sa táto zmena zachová aj pri reštarte aleo chybe systému

## Relačné databázy

Najčastejšie sa v Jave pracuje s relačnými databázami. Údaje sú organizované do tabuliek pozostávajúcich z riadkov a stĺpcov.

=== "Príklad tabuľky Kniha"

    | id | nazov              | autor         | rok  |
    | -- | ------------------ | ------------- | ---- |
    | 1  | Java Programovanie | Ján Novák     | 2023 |
    | 2  | Databázy           | Peter Horváth | 2022 |

Každý riadok predstavuje jeden záznam a každý stĺpec určitý atribút záznamu. Medzi najznámejšie relačné databázy patria:

- PostgreSQL
- SQLite
- MySQL / MariaDB
- Oracle Database
- Microsoft SQL Server

### SQL Jazyk

Na komunikáciu s relačnými databázami sa používa jazyk SQL *(Structured Query Language)*. Najčastejšie príkazy:

```sql title="Vytvorenie tabuľky"
CREATE TABLE kniha (
    id INTEGER PRIMARY KEY,
    nazov TEXT,
    autor TEXT,
    rok INTEGER
);
```

```sql title="Vloženie údajov"
INSERT INTO kniha(nazov, autor, rok)
VALUES ('Java Programovanie', 'Ján Novák', 2023);
```

```sql title="Výber údajov"
SELECT * FROM kniha;
```

```sql title="Aktualizácia údajov"
UPDATE kniha
SET rok = 2024
WHERE id = 1;
```

```sql title="Odstránenie údajov"
DELETE FROM kniha
WHERE id = 1;
```

## Spôsoby prístupu k databázam

V Jave existujú tri hlavné druhy prístupu k databázam:

1. **JDBC** - nízkoúrovňový, plná kontrola nad SQL
2. **ORM** (Object-Relational Mapping) - automatické mapovanie Java objektov na tabuľky
3. **Vyššie abstrakcie** - Spring Data JPA, jOOQ, Querydsl (nad ORM)

## JDBC

JDBC je štandardné API jazyka Java určené na komunikáciu s databázami.
Predstavuje vrstvu medzi Java aplikáciou a databázovým serverom. Vďaka JDBC môže programátor používať rovnaké rozhranie pre rôzne databázové systémy.

Každá databáza poskytuje vlastný JDBC ovládač (driver), ktorý zabezpečuje komunikáciu medzi aplikáciou a databázou.
JDBC driver je teda knižnica implementujúca JDBC rozhranie pre konkrétnu databázu. O pripojenie ku konkrétnej databáza sa v JDBC stará trieda `DriverManager`.

Pri práci s databázou sa v JDBC najčastejšie používajú tieto triedy:

#### Trieda `Connection`

Objekt triedy `Connection` reprezentuje otvorené spojenie s databázou.

Príklad vytvorenia spojenia so SQLite:

```java
Connection conn =
    DriverManager.getConnection("jdbc:sqlite:knihy.db");
```

Po úspešnom vytvorení spojenia môže aplikácia vykonávať SQL príkazy.

#### Trieda `Statement`

Objekt triedy `Statement` slúži na vykonávanie jednoduchých SQL príkazov.

```java
Statement stmt = conn.createStatement();

ResultSet rs =
    stmt.executeQuery("SELECT * FROM kniha");
```

Používa sa najmä v jednoduchých príkladoch a pri SQL príkazoch bez parametrov.

#### Trieda `PreparedStatement`

`PreparedStatement` je rozšírením triedy `Statement`. Výhody:

- vyššia bezpečnosť,
- ochrana proti SQL Injection (little [bobby tables](https://xkcd.com/327/)),
- lepší výkon pri opakovanom vykonávaní,
- automatické ošetrenie (escapovanie) špeciálnych znakov.

Príklad:

```java
String sql =
    "SELECT * FROM kniha WHERE id = ?";

PreparedStatement pstmt =
    conn.prepareStatement(sql);

pstmt.setInt(1, 5);

ResultSet rs = pstmt.executeQuery();
```

Vo väčšine reálnych aplikácií sa odporúča používať práve PreparedStatement.

#### Trieda `ResultSet`

Objekt triedy `ResultSet` obsahuje výsledok SQL dotazu.

```java
while (rs.next()) {
    int id = rs.getInt("id");
    String nazov = rs.getString("nazov");

    System.out.println(id + " " + nazov);
}
```

Metóda `next()` posúva kurzor na ďalší riadok výsledku. Najčastejšie používané metódy tejto triedy:

- `getInt()`
- `getLong()`
- `getDouble()`
- `getString()`
- `getBoolean()`
- `getDate()`

## SQLite

SQLite je najrozšírenejšia embedovaná relačná databáza na svete. Embedovaná znamená, že nepotrebujeme
žiaden samostatný server, ale databáza beží priamo v našom programe.

Celá databáza je **jeden súbor** na disku. SQLite je napísaná v jazyku C, je veľmi rýchla a malá. Podporuje väčšinu SQL-92 + niektoré rozšírenia.

=== "Architektúra JDBC + SQLite:"

    ```mermaid
    graph TD
        A[Java aplikácia] -->|JDBC API| B[sqlite-jdbc Driver]
        B --> C[SQLite C engine]
        C --> D[library.db<br/>alebo :memory:]
    ```

Kedy SQLite používať?:

- Desktopové aplikácie
- Mobilné aplikácie (Android používa SQLite natívne)
- IoT / embedded systémy
- Lokálne úložisko v desktopových nástrojoch
- **Testovanie a prototypovanie** (rýchle vytvorenie DB v pamäti)
- Aplikácie s nízkou až strednou záťažou

Kedy SQLite nepoužívať?:

- Vysoká konkurencia zápisov (SQLite zamyká celú databázu pri zápise)
- Veľké dáta a komplexná analytika
- Distribuované systémy s viacerými nodmi

Túto databázu budeme používať aj my v našich ďalších úlohách.

## Zhrnutie cvičenia

- [x] Prečo databázy v Jave?
    * [ ] Dlhodobé uloženie dát (aj po reštarte aplikácie)
    * [ ] Súbežný prístup viacerých používateľov/procesov
    * [ ] ACID vlastnosti (Atomicity, Consistency, Isolation, Durability)
    * [ ] Výkonné dotazovanie a reporting
- [x] ACID
    * [ ] Atomicita - Viaceré zmeny v databáza sa vykonajú ako jedna tranzakcia, buď sa vykonajú všetky, alebo ani jedna
    * [ ] Konzistencia - Zmeny v databáze neporušia databázový invariant, napr. nepovolenie duplicitných hodnôt
    * [ ] Izolácia - Pri súčasnom prístupe k databáze od viacerých užívateľov títo nebudú nikdy vidieť nedokončené zmeny od ostatných
    * [ ] Durability - Po úspešnom prevedení zmeny sa táto zmena zachová aj pri reštarte aleo chybe systému
- [x] Relačné databázy
    * [ ] Údaje sú organizované do tabuliek pozostávajúcich z riadkov a stĺpcov.
    * [ ] Každý riadok predstavuje jeden záznam a každý stĺpec určitý atribút záznamu. 
    * [ ] Medzi najznámejšie relačné databázy patria: PostgreSQL, SQLite, MySQL / MariaDB, Oracle Database a Microsoft SQL Server
- [x] Spôsoby prístupu k databázam
    * [ ] JDBC - nízkoúrovňový, plná kontrola nad SQL
    * [ ] ORM (Object-Relational Mapping) - automatické mapovanie Java objektov na tabuľky
    * [ ] Vyššie abstrakcie - Spring Data JPA, jOOQ, Querydsl (nad ORM)
- [x] JDBC
    * [ ]  Predstavuje vrstvu medzi Java aplikáciou a databázovým serverom, vďaka ktorej môže programátor používať rovnaké rozhranie pre rôzne databázové systémy.
    * [ ] JDBC driver je teda knižnica implementujúca JDBC rozhranie pre konkrétnu databázu. O pripojenie ku konkrétnej databáza sa v JDBC stará trieda DriverManager.
    * [ ] Objekt triedy Connection reprezentuje otvorené spojenie s databázou.
    * [ ] Objekt triedy Statement alebo PreparedStatement slúži na vykonávanie SQL príkazov.
    * [ ] Objekt triedy ResultSet obsahuje výsledok SQL dotazu.
- [x] SQLite
    * [ ] Populárna embedovaná relačná databáza. Nepotrebujeme žiaden samostatný server, ale databáza beží priamo v našom programe.
    * [ ] Celá databáza je jeden súbor na disku. 
    * [ ] Oblasti použitia: Desktopové aplikácie, Mobilné aplikácie, IoT / embedded systémy
    * [ ] Nie je vhodná v týchto prípadoch: Vysoká konkurencia zápisov, veľké dáta a komplexná analytika alebo Distribuované systémy s viacerými nodmi
