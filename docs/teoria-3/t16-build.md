# Teória 16: Budovanie Java aplikácií

Doteraz sme pri tvorbe Java aplikácií používali výhradne IDE rozhranie IntelliJ IDEA. Toto IDE nám veľa vecí uľahčuje, ale je dobré poznať, aký je proces vytvárania Java aplikácií mimo tohto prostredia.

Proces budovania Java aplikácií zahŕňa tieto kroky:

1. Písanie zdrojového kódu - `.java` súbory

1. Kompilácia do bajtkódu - `.class` súbory

1. Spustenie programu - pomocou Java Virtual Machine


## Písanie zdrojového kódu

Zdrojový kód programu sa píše so textových súborov s príponou `.java`. Každý kus Java kódu musí byť v nejakej triede. Každá verejná trieda musí byť napísaná v samostatnom `.java` súbore s rovnakým názvom, ako je daná trieda.

Zdrojové súbory sú uložené v priečinkoch podľa toho, v akom balíku sa nachádzajú triedy. Všetky zdrojové súbory zvyknú byť v spoločnom adresári, napr. s názvom `src`.

### Zdrojové dáta

Okrem zdrojových súborov s Java kódom máme často aj ďalšie súbory, ktoré sú súčasťou aplikácie. Ide napr. o ikonky, obrázky, textové súbory a iné. Tieto súbory zvyčajne označujeme pojmom **assets** alebo **resources**, a v projekte sú uložené vo svojom vlastnom priečinku.

### Označenie zdrojových adresárov v IntelliJ IDEA

Aby IDE nástroj vedel, v ktorom adresári v našom projekte máme súbory so zdrojovým kódom, je potrebné tento adresár vhodne označkovať. V IntelliJ IDEA to urobíme v lište Project pravým kliknutím na daný adresár a výberom možnosti Mark Directory As - Sources Root

Podobným spôsobom vieme označkovať aj adresár so zdrojovým dátami, pričom vyberieme možnosť Mark Directory As - Resources Root

## Kompilácia

Súbory so zdrojovým kódom sa nedajú spustiť priamo a sú potrebné ďalšie kroky. Najprv je potrebné zdrojové súbory skompilovať do bajtkódu, čo je typ strojového kódu, ktorému rozumie Java Virtual Machine, ktorý spúšťa Java programy.

Každý `.java` súbor je potrebné skompilovať a výsledkom je súbor s príponou `.class`. Každá skompilovaná Java trieda má svoj vlastný `.class` súbor. Tieto skompilované súbory sú uložené v priečinkoch podľa svojich balíkov, podobne ako to bolo aj so zdrojovými súbormi.

Na kompiláciu zdrojového `.java` súboru sa používa nástroj `javac`. Základné použitie je `javac NazovTriedy.java`, kedy sa skompiluje jeden zdrojový súbor a vytvorí sa skompilovaný súbor `NazovTriedy.class`

Väčšinou však máme viacero súborov, ktoré potrebujeme skompilovať a výsledné súbory chceme dať to samostatného adresára. V takomto prípade vieme použiť napr. `javac -d out -sourcepath src src/sk/spse/aplikacia/Main.java`, kedy špecifikujeme priečinok, kde sa majú uložiť skompilované súbory (`-d`), v ktorom priečinku sa nachádzajú zdrojové súbory (`-sourcepath`) a ktorý súbor obsahuje hlavnú triedu programu. Kompilátor následne skompiluje všetky zdrojové súbory potrebné pre spustenie aplikácie.

Takúto manuálnu kompiláciu však nerobíme takmer nikdy sami a namiesto toho používame IDE, ktoré nám kompiluje zdrojové súbory automaticky. V IDE nástroji IntelliJ IDEA vieme skompilované súbory nájsť v priečinku `out/production` alebo `target/classes`, podľa toho, ako máme projekt nakonfugirovaný.

## Spustenie programu

Pre samotné spustenie programu potrebujeme vedieť 2 veci:

- Kde sa nachádzajú všetky potrebné skompilované `.class` súbory
- Ako sa volá hlavná trieda programu, ktorá obsahuje metódu `main`

Keďže triedy môžu byť v roznych adresároch a nie vždy ich máme všetky v jednom spoločnom adresári, je potrebné dať dokopy zoznam priečinkov, v ktorých má Java hľadať `.class` súbory s našimi triedami. Takýto zoznam ciest sa v Jave volá **classpath**. Do classpath sa uvádza vždy iba koreňový adresár (napr. `src`), podadresáre balíkov sa neuvádzajú.

Classpath je cesta, kde JVM alebo kompilátor hľadá triedy (.class súbory) a knižnice (JAR súbory). Ak máme priečinkov so zdrojovými súbormi viacero, pri písani classpath ich vo Windowse oddelíme bodkočiarkou `;` alebo v Linuxe ich oddeľujeme dvojbodkou `:`. Príklad `src;src-other;external/src`

Spustenie programu realizujeme pomocou príkazu `java`. Základné použitie je `java -cp <classpath> <hlavna_trieda>`. Príklad: `java -cp out sk.spse.aplikacia.Main`


## JAR archív

Ak chceme našu aplikáciu začať používať, nie je dobré mať ju rozlezenú po desiatkach priečinkov a po všelijakých `.class` súboroch. V Jave vieme všetky potrebné súbory zabaliť do jedného archívu a ten bude predstavovať našu výslednú aplikáciu.

Takých archív so skompilovanými súbormi sa v Jave volá *JAR*. JAR archív je súbor s príponou `.jar` a obsahuje všetky skompilované súbory tried a taktiež aj súbory so zdrojovými dátami (obrázky, text, dáta). Do tohto archívu je tiež možné vložiť zdrojové súbory alebo dokumentáciu.

Informáciu o tom, ktorá trieda je hlavná trieda aplikácie je taktiež možné vložiť to JAR archívu. Robí sa to vytvorením súboru `META-INF/MANIFEST.MF` do ktorého uvedieme názov hlavnej triedy.

Na vytvorenie JAR archívu vieme použiť program `jar`. Príklad použitia: `jar cfm app.jar manifest.txt -C out`.

### Spustenie JAR balíkov

Ak máme Java program zabalený vo forme JAR archívu, vieme ho spusiť pomocou príkazu `java`. Namiesto classpathu uvedieme cestu k `.jar` archívu. Príklad `java -jar app.jar`

## Java Knižnice

Niekedy výsledkom nášho projektu nie je spustiteľný program, ale knižnica tried, ktoré ponúkame ostatným programátorom na použitie vo ich projektoch. V takomto prípade je ideálne naše triedy zabaliť do JAR archívu. Tento JAR archív nemusí mať hlavnú triedu, nemusí byť spustiteľný. Dôležité je, že všetko potrebné bude v jednom archíve a ten vieme jednoducho distribuovať.

Ak chceme použiť triedy z JAR archívu v našom programe, je potrebné ich pridať na classpath. To sa robí jednoducho tak, že namiesto cesty k priečinku uvedieme cestu k JAR archívu.

Príklad použitie knižnice tried z archívu `kniznica.jar`: `java -cp src:kniznica.jar sk.spse.aplikacia.Main`

## Budovanie Java aplikácií pomocou IntelliJ IDEA

Použitie nástroja IDE nám výrazne uľahčí všetky procesy popísané vyššie. Ak v nástroji IDEA spustíme projekt, nástroj nám sám skompiluje všetky potrebné súbory a potom spustí hlavnú triedu. 

Pomocou IDEA vieme aj jednoducho vytvoriť JAR archív. Robí sa to v časti *File - Project Structure*, kde potom v sekcii **Artifacts** vieme pridať nový JAR artefakt. Ak to urobíme, IDEA nám pri každom builde vytvorí JAR archív. Build projektu vieme spustiť pomocou Build - Build Project, alebo CTRL-F9. Výsledný JAR archív potom nájdeme v priečincku `out/artifacts`.

## Zhrnutie teórie

- [x] Budovanie Java aplikácií
    * [ ] Písanie zdrojového kódu - `.java` súbory
    * [ ] Kompilácia do bajtkódu - `.class` súbory
    * [ ] Spustenie programu - pomocou Java Virtual Machine
- [x] Písanie zdrojového kódu
    * [ ] Každá verejná trieda musí byť napísaná v samostatnom `.java` súbore s rovnakým názvom, ako je daná trieda.
    * [ ] Zdrojové súbory sú uložené v priečinkoch podľa toho, v akom balíku sa nachádzajú triedy. 
    * [ ] Všetky zdrojové súbory zvyknú byť v spoločnom adresári, napr. s názvom `src`.
    * [ ] Okrem zdrojových súborov s Java kódom máme často aj ďalšie súbory, ktoré sú súčasťou aplikácie.
    * [ ] Tieto zdrojové dáta zvyčajne označujeme pojmom *assets* alebo *resources*, a v projekte sú uložené vo svojom vlastnom priečinku.
    * [ ] V IntelliJ IDEA označíme zdrojové adresáre v lište *Project* pravým kliknutím na daný adresár a výberom možnosti *Mark Directory As*
- [x] Kompilácia
    * [ ] Zdrojové súbory sa kompilujú do bajtkódu (anglicky bytecode), čo je typ strojového kódu, ktorému rozumie Java Virtual Machine, ktorý spúšťa Java programy.
    * [ ] Každá skompilovaná Java trieda má svoj vlastný `.class` súbor
    * [ ] Tieto skompilované súbory sú uložené v priečinkoch podľa svojich balíkov, podobne ako to bolo aj so zdrojovými súbormi
    * [ ] Na kompiláciu zdrojového .java súboru sa používa nástroj `javac`
    * [ ] `javac NazovTriedy.java` - kompilácia jedného súboru
    * [ ] `javac -d out -sourcepath src src/sk/spse/aplikacia/Main.java` - kompilácia projektu
    * [ ] Takúto manuálnu kompiláciu však nerobíme takmer nikdy sami a namiesto toho používame IDE
    * [ ] V IDE nástroji IntelliJ IDEA vieme skompilované súbory nájsť v priečinku `out/production` alebo `target/classes`, podľa konfigurácie
- [x] Spustenie programu
    * [ ] Potrebujeme vedieť kde sa nachádzajú všetky potrebné skompilované .class súbory a ako sa volá hlavná trieda programu, ktorá obsahuje metódu main
    * [ ] Zoznam ciest k zdrojovým súborom sa v Jave volá **classpath**. 
    * [ ] Classpath je cesta, kde JVM alebo kompilátor hľadá triedy (.class súbory) a knižnice (JAR súbory).
    * [ ] Do classpath sa uvádza vždy iba koreňový adresár (napr. `src`), podadresáre balíkov sa neuvádzajú.
    * [ ] Ak máme priečinkov so zdrojovými súbormi viacero, pri písani classpath ich vo Windowse oddelíme bodkočiarkou `;` alebo v Linuxe ich oddeľujeme dvojbodkou `:`
    * [ ] Príklad classpath: `src;src-other;external/src`
    * [ ] Spustenie programu realizujeme pomocou príkazu `java`
    * [ ] Základné použitie je `java -cp <classpath> <hlavna_trieda>`
    * [ ] Príklad: `java -cp out sk.spse.aplikacia.Main`
- [x] JAR archív
    * [ ] V Jave vieme všetky potrebné súbory zabaliť do jedného archívu a ten bude predstavovať našu výslednú aplikáciu.
    * [ ] JAR archív je súbor s príponou .jar a obsahuje všetky skompilované súbory tried a taktiež aj súbory so zdrojovými dátami (obrázky, text, dáta). Do tohto archívu je tiež možné vložiť zdrojové súbory alebo dokumentáciu
    * [ ] Informáciu o tom, ktorá trieda je hlavná trieda aplikácie je taktiež možné vložiť to JAR archívu. Robí sa to vytvorením súboru `META-INF/MANIFEST.MF` do ktorého uvedieme názov hlavnej triedy.
    * [ ] Na vytvorenie JAR archívu vieme použiť program `jar`. Príklad použitia: `jar cfm app.jar manifest.txt -C out`
    * [ ] Ak máme Java program zabalený vo forme JAR archívu, vieme ho spusiť pomocou príkazu `java`. Namiesto classpathu uvedieme cestu k `.jar` archívu.
    * [ ] Príklad: `java -jar app.jar`
    * [ ] Ak chceme použiť triedy z JAR archívu v našom programe, je potrebné ich pridať na classpath.
    * [ ] Príklad: `java -cp src:kniznica.jar sk.spse.aplikacia.Main` - používame JAR archív ako knižnicu - zdroj tried, nespúšťame ho
    * [ ] Pomocou IDEA vieme vytvoriť JAR archív v časti File - Project Structure, kde potom v sekcii Artifacts vieme pridať nový JAR artefakt.
    * [ ] Po zbuildovaní projektu nájdeme výsledný JAR archív v priečincku `out/artifacts`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Budovanie Java aplikácií
    
    1. Písanie zdrojového kódu - .java súbory
    2. Kompilácia do bajtkódu - .class súbory - nástroj javac
    3. Spustenie programu - pomocou Java Virtual Machine - nástroj java

    Písanie zdrojového kódu
    
    Každá verejná trieda je v samostatnom .java súbore
    Zdrojové súbory sú v priečinkoch podľa ich balíkov
    Ostatné potrebné súboru - zdrojové dáta zvyčajne označujeme pojmom assets alebo resources, 
    majú vlastný priečinok
    V IntelliJ označíme zdrojové adresáre pomocou pravého tlačíťka a Mark Directory As

    Kompilácia
    
    Zdrojové súbory sa kompilujú do bajtkódu, ktorému rozumie Java Virtual Machine
    Každá skompilovaná trieda má svoj vlastný .class súbor v priečinkoch podľa balíkov
    javac NazovTriedy.java - kompilácia jedného súboru
    javac -d out -sourcepath src src/sk/spse/aplikacia/Main.java - kompilácia projektu
    Na kompiláciu väčšinou používame IDE, skompilované súbory sú v out/production alebo 
    target/classes, podľa konfigurácie

    Spustenie programu

    Potrebujeme vedieť kde sú skompilované .class súbory a ako sa volá hlavná trieda programu
    Zoznam ciest k zdrojovým súborom sa v Jave volá classpath. 
    Do classpath sa uvádza vždy iba koreňový adresár (napr. src), podadresáre balíkov sa neuvádzajú.
    Viacero ciest oddelíme bodkočiarkou ; alebo v Linuxe ich oddeľujeme dvojbodkou :
    Príklad classpath: src;src-other;external/src
    Spustenie programu realizujeme pomocou príkazu java
    Základné použitie: java -cp <classpath> <hlavna_trieda>
    Príklad: java -cp out sk.spse.aplikacia.Main

    JAR archív

    JAR archív je súbor s príponou .jar a obsahuje všetky skompilované súbory tried a taktiež
    aj súbory so zdrojovými dátami
    Názov hlavnej triedy dáme do súboru manifest.txt (MANIFEST.MF)
    Príklad vytvorenia pomocou nástroja jar: jar cfm app.jar manifest.txt -C out
    Spustenie programu s JAR archívu: java -jar app.jar
    Triedy z JAR archívu vieme pridať na classpath a použiť v inom programe
    Príklad: `java -cp src:kniznica.jar sk.spse.aplikacia.Main` 
    Vtedy používame JAR archív ako knižnicu - zdroj tried, nespúšťame ho
    
    V IDEA vytvoríme JAR v časti File - Project Structure, Artifacts, kde prodáme JAR artefakt
    Po zbuildovaní projektu nájdeme JAR archív v priečincku out/artifacts
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - názvy nástrojov na kompiláciu, spúšťanie programov a vytváranie JAR archívu
    - Rozdiel medzi .java a .class súborom
    - Čo je classpath
    - Na čo slúži JAR archív
