# Cvičenie 19: Práca s databázou

Java poskytuje niekoľko rôznych spôsobov, ako pristupovať k databázam.
Tým najzákladnejším je JDBC a s ním dnes budeme aj pracovať.

## JDBC

Databáz je veľké množstvo a každá má svoj vlastný spôsob pripojenia. 
Java proces pripojenia a komunikácie s databázou zjednotila do jedného 
API s názvom JDBC (Java Database Connectivity). 

Každá komunikácia s databázou v Jave sa realizuje cez `JDBC` driver. 
Priame použitie JDBC API je však niekedy príliš nízkoúrovňové a komplikované,
 preto Java aplikácie často používajú aj ďalšie nadstavby.

Vedieť pracovať s JDBC je však stále veľmi potrebné. JDBC je poskytovaná v rámci
JDK, takže na jej použitie nie je potrebné inštalovať žiadnu knižnicu.

Hlavné triedy a rozhrania JDBC sú v balíku [`java.sql`](https://docs.oracle.com/en/java/javase/24/docs/api/java.sql/java/sql/package-summary.html)

=== "Hlavné triedy a rozhrania:"

    | Trieda / Rozhranie       | Účel                                      |
    |--------------------------|-------------------------------------------|
    | `DriverManager`          | Získanie `Connection`                     |
    | `Connection`             | Spojenie s databázou + transakcie         |
    | `Statement`              | Jednoduché SQL (riziko SQL injection)     |
    | `PreparedStatement`      | Bezpečné parametrizované dotazy (`?`)     |
    | `ResultSet`              | Výsledok `SELECT` (iterácia ako cursor)   |
    | `SQLException`           | Výnimka pri chybe databázy                |

Pomocou týchto tried vieme nadviazať spojenie s databázou a vykonať základné SQL príkazy.

=== "Základný flow, s try-with-resources:"

    Príklad vloženia údajov do existujúcej tabuľky:

    ```java
    String url = "jdbc:sqlite:library.db";

    try (Connection conn = DriverManager.getConnection(url);
        PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO books (title, author, published_year) VALUES (?, ?, ?)")) {

        ps.setString(1, "Effective Java");
        ps.setString(2, "Joshua Bloch");
        ps.setInt(3, 2018);
        ps.executeUpdate();

    } catch (SQLException e) {
        e.printStackTrace();
    }
    ```

* Príkazy, ktoré menia databázu (CREATE, INSERT, UPDATE, DELETE) sa realizujú pomocou `executeUpdate`
* Príkazy, ktoré načítavajú dáta, sa realizujú pomocou `executeQuery`

=== "Príklad načítania údajov z tabuľky"

    ```java
    public List<Book> findAllBooks() {
        List<Book> books = new ArrayList<>();

        String sql =
            "SELECT title, author, published_year FROM books";

        try (
            Connection conn = DriverManager.getConnection(URL);
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery(sql)
        ) {
            while (rs.next()) {
                books.add(new Book(
                    rs.getString("title"),
                    rs.getString("author"),
                    rs.getInt("published_year")
                ));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return books;
    }
    ```

## Dôležité pravidlá

1. **Vždy** používajte `try-with-resources` (od Java 7)
1. **Vždy** používajte `PreparedStatement` pri vstupe od používateľa
1. Obyčajný `Statement` používajte len na statické DDL (`CREATE TABLE`)
1. Mapovanie `ResultSet` na objekty sa robí ručne, alebo sa použije ORM)

## SQLite

Pre dnešné úlohy budeme používať databázu SQLite. Ide o populárnu a kvalitnú databázu, 
ktorá funguje priamo z vašej aplikácie a nie je potrebná žiadna inštalácia a konfigurácia.


Jediné, čo je potrebné vo vašom projekte urobiť, je použiť knižnicu `org.xerial:sqlite-jdbc:3.53.1.0`.

Celá SQLite databáza sa ukladá do jedného súboru na disku.

Na základe umiestnenia súboru si potom vytvoríte JDBC URL. Príklady:

- Súbor: `jdbc:sqlite:library.db`
- In-memory only (ideálne pre testy): `jdbc:sqlite::memory:`
- Read-only: `jdbc:sqlite:file:library.db?mode=ro`

### IDEA plugin na prehliadanie databázy

Obsah databázy si viete prezrieť aj priamo z nástroja IDEA po nainštalovaní pluginu **Database Tools and SQL**.


## Úlohy

!!! example "Úloha 19.1: Projekt telefónny zoznam"

    Vytvorte nový projekt a použite knižnicu SQLite.

    Vytvorte triedu pre reprezentáciu osoby:

    - Meno
    - Priezvisko
    - Telefónne číslo

    Navrhnite štruktúru tabuľky pre osoby v telefónnom zozname. Pridajte aj jedinečný identifikátor a dátumy pridania a zmeny.

    Vytvorte metódy na vytvorenie tabuľky a na pridanie nového kontaktu.

    Vytvorte buď ovládanie pomocou konzoly alebo pomocou JavaFX.

!!! example "Úloha 19.2: Naplnenie zoznamu"

    Vytvorte v tabuľke aspoň 50 záznamov.

    Nainštalujte si Database Tools and SQL plugin a otvorte si v ňom SQLite databázu. Zobrazte v tomto nástroji štruktúru tabuľky a jej dáta.

!!! example "Úloha 19.3: Výpis kontaktov"

    Pridajte do vašej aplikácie funkcionality na vypísanie kontaktov.

!!! example "Úloha 19.4: Volanie a história hovorov"

    Pridajte do vašej aplikácie funkcionalitu na "volanie" a na výpis posledných hovorov.

    Na zoznam hovorov si navrhnite a vytvorte novú tabuľku.

## Zhrnutie cvičenia

- [x] JDBC
    * [ ] JDBC je poskytovaná v rámci JDK, takže na jej použitie nie je potrebné inštalovať žiadnu knižnicu.
    * [ ] `DriverManager` - Získanie `Connection`                     
    * [ ] `Connection` - Spojenie s databázou + transakcie         
    * [ ] `Statement` - Jednoduché SQL (riziko SQL injection)     
    * [ ] `PreparedStatement` - Bezpečné parametrizované dotazy (`?`)     
    * [ ] `ResultSet` - Výsledok `SELECT` (iterácia ako cursor)  
    * [ ] `SQLException` - Výnimka pri chybe databázy               
- [x] Príkazy
    * [ ] Príkazy, ktoré menia databázu (CREATE, INSERT, UPDATE, DELETE) sa realizujú pomocou executeUpdate
    * [ ] Príkazy, ktoré načítavajú dáta, sa realizujú pomocou executeQuery
- [x] Dôležité pravidlá
    * [ ] **Vždy** používajte `try-with-resources` (od Java 7)
    * [ ] **Vždy** používajte `PreparedStatement` pri vstupe od používateľa
    * [ ] Obyčajný `Statement` používajte len na statické DDL (`CREATE TABLE`)
    * [ ] Mapovanie `ResultSet` na objekty sa robí ručne, alebo sa použije ORM)
- [x] SQLite
    * [ ] Rýchla embedovaná databáza, ktorá ukladá celú databázu do jedného súboru
    * [ ] Knižnica `org.xerial:sqlite-jdbc:3.53.1.0`.
    * [ ] JDBC pre SQLite v súbore: `jdbc:sqlite:library.db`
    * [ ] In-memory only JDBC (ideálne pre testy): `jdbc:sqlite::memory:`
    * [ ] Read-only: `jdbc:sqlite:file:library.db?mode=ro`
- [x] IDEA plugin na prehliadanie databázy - **Database Tools and SQL**.
