# Cvičenie 20: Práca s ORM

Dnes si precvičíme prácu s ORM

## Hibernate a ORM

Hibernate je najpopulárnejší ORM framework pre Javu. Od verzie 3.5 implementuje štandard JPA (Java Persistence API), dnes nazývaný Jakarta Persistence.

- JPA = štandard (rozhranie)
- Hibernate = jedna z implementácií tohto štandardu (najpoužívanejšia)

V modernom kóde (Hibernate 6/7) používame balíček `jakarta.persistence.*.`

## Základné pojmy 

| Pojem | Význam | Životnosnosť |
|---------|---------|---------------|
|Entita | Trieda mapovaná na tabuľku (@Entity) | Dlhodobá
|Session | Hlavný objekt na prácu s entitami (Hibernate) | Krátkodobá
|EntityManager | JPA ekvivalent Session | Krátkodobá
|SessionFactory | Továreň na Session (vytvára sa raz) | Dlhodobá
|Transaction | Logická jednotka práce s databázou | Krátkodobá

Dôležité anotácie:

- @Entity – trieda je entita
- @Table – názov tabuľky
- @Id – primárny kľúč
- @GeneratedValue – stratégia generovania ID (IDENTITY, SEQUENCE, TABLE, AUTO)
- @Column – mapovanie stĺpca

## Potrebné knižnice:

- SQLite: `org.xerial:sqlite-jdbc:3.53.1.0`
- Hibernate: `org.hibernate.orm:hibernate-community-dialects:7.3.8.Final`

## Príklad

```java title="Kniha.java"
import jakarta.persistence.*;

@Entity
@Table(name = "knihy")
public class Kniha {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String nazov;

    private String autor;

    @Column(name = "rok_vydania")
    private Integer rokVydania;

    public Kniha() {}

    public Kniha(String nazov, String autor, Integer rokVydania) {
        this.nazov = nazov;
        this.autor = autor;
        this.rokVydania = rokVydania;
    }

    // Gettery, settery, toString...
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNazov() { return nazov; }
    public void setNazov(String nazov) { this.nazov = nazov; }
    public String getAutor() { return autor; }
    public void setAutor(String autor) { this.autor = autor; }
    public Integer getRokVydania() { return rokVydania; }
    public void setRokVydania(Integer rokVydania) { this.rokVydania = rokVydania; }

    @Override
    public String toString() {
        return "Kniha{id=" + id + ", nazov='" + nazov + "', autor='" + autor + "', rokVydania=" + rokVydania + '}';
    }
}
```

```java title="Main.java"
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.boot.Metadata;
import org.hibernate.boot.MetadataSources;
import org.hibernate.boot.registry.StandardServiceRegistry;
import org.hibernate.boot.registry.StandardServiceRegistryBuilder;

import java.util.List;

public class Main {

    public static void main(String[] args) {

        StandardServiceRegistry registry = new StandardServiceRegistryBuilder()
                .applySetting("hibernate.dialect", "org.hibernate.community.dialect.SQLiteDialect")
                .applySetting("hibernate.connection.driver_class", "org.sqlite.JDBC")
                .applySetting("hibernate.connection.url", "jdbc:sqlite:knihy.db")
                .applySetting("hibernate.hbm2ddl.auto", "create-drop")
                .applySetting("hibernate.show_sql", "true")
                .applySetting("hibernate.format_sql", "true")
                .build();

        Metadata metadata = new MetadataSources(registry)
                .addAnnotatedClass(Kniha.class)
                .buildMetadata();

        try (SessionFactory sessionFactory = metadata.getSessionFactoryBuilder().build()) {

            // === Ukladanie ===
            try (Session session = sessionFactory.openSession()) {
                Transaction tx = session.beginTransaction();
                session.persist(new Kniha("1984", "George Orwell", 1949));
                session.persist(new Kniha("Duna", "Frank Herbert", 1965));
                tx.commit();
            }

            // === Načítanie ===
            try (Session session = sessionFactory.openSession()) {
                List<Kniha> knihy = session.createQuery("FROM Kniha", Kniha.class).list();
                knihy.forEach(System.out::println);
            }
        } finally {
            StandardServiceRegistryBuilder.destroy(registry);
        }
    }
}
```

!!! example "Úloha 20.1: Hibernate príklad"

    Vytvorte si nový projekt v IDEA a vyskúšajte si vyššie uvedený príklad.

    V prehliadači databáz si pozrite obsah vytvorenej databázy `knihy.db`

!!! example "Úloha 20.2: Nové funkcionality"

    Vytvor triedu KnihaDao alebo pracuj priamo v Main triede a implementuj tieto metódy:

    - ulozKnihu(Kniha kniha)
    - najdiKnihuPodlaId(Long id)
    - aktualizujKnihu(Kniha kniha)
    - vymazKnihu(Long id)
    - vypisVsetkyKnihy()

    Požiadavky:

    - Všetky operácie musia byť v transakcii.
    - Použi try-with-resources pre Session.
    - Otestuj funkčnosť tak, že uložíš aspoň 5 kníh a vypíšeš ich.

    Tip: Použi metódu session.persist(), session.find(), session.merge() a session.remove().

!!! example "Úloha 20.3: Vyhľadávanie pomocou HQL/JPQL"

    Pridaj do projektu nasledujúce funkcie:

    - Nájdi všetky knihy podľa autora.
    - Nájdi všetky dostupné knihy (dostupna = true).
    - Nájdi knihy vydané po určitom roku.
    - Nájdi knihy, ktorých názov obsahuje zadaný text (použi LIKE).

    Príklad dotazu:

    ```java
    JavaList<Kniha> knihy = session.createQuery(
        "FROM Kniha k WHERE k.autor = :autor", Kniha.class)
        .setParameter("autor", "George Orwell")
        .list();
    ```

!!! example "Úloha 20.4: Kategórie kníh"

    Pridaj entitu Kategoria:

    ```java
    @Entity
    @Table(name = "kategorie")
    public class Kategoria {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;

        @Column(unique = true, nullable = false)
        private String nazov;

        // gettery + settery
    }
    ```

    Potom uprav entitu Kniha tak, aby mala vzťah k Kategoria:
    
    ```java
    @ManyToOne
    @JoinColumn(name = "kategoria_id")
    private Kategoria kategoria;
    ```
    
    Úlohy:

    Vytvor niekoľko kategórií (Beletria, Odborná literatúra, Detektívky...).
    Priraď knihám kategórie.
    Napíš dotaz, ktorý vypíše všetky knihy z danej kategórie.

!!! example "Úloha 20.5: Telefónny zoznam pomocou Hibernate"

    Prerobte úlohu z minulej hodiny (telefónny zoznam) tak, aby používala Hibernate

