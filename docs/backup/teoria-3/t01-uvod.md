# Teória 1: Úvod a motivácia

Vitajte na predmete Objektovo orientované programovanie. Počas roka sa naučíte navrhovať a vytvárať softvér pomocou princípov OOP. Okrem toho sa zlepšíte v programovaní a softvérovom vývoji, naberiete nové vedomosti a praktické skúsenosti. Predmet je delený na 1 hodinu teórie a 2 hodiny cvičení. 

*[OOP]: Objektovo orientované programovanie
*[DSL]: Domain Specific Language

Tento predmet na škole vyučujeme už viacero rokov. Oblasť softvérového vývoja sa neustále vyvíja. Preto aj my sa budeme snažiť predstaviť vám prístupy, techniky a nástroje, ktoré sú moderné, sú používané v praxi a majú využitie naprieč celou oblasťou vývoja softvéru.

## Nahradia nás AI agenti?

S príchodom asistentov a agentov založených na umelej inteligencii sa od základov mení veľa veci, ktoré boli desaťročia zaužívanou praxou. Začíname to vidieť v roku 2025 už aj na Slovensku. Väčšina veľkých IT korporácií prepúšťa a preškoľuje zamestnancov na používanie umelej inteligencie. Niektoré pracovné pozície sa zlučujú, iné zanikajú. Ktorá organizácia sa tomuto trendu neprispôsobí, bude za pár rokov predbehnutá start-upmi, ktoré budú AI naplno využívať vo svojich flexibilných tímoch.

<div class="md-has-sidebar" markdown>
  <main markdown>
Zmena prichádza aj do vyučovania a vzdelávania. V škole je zbytočné pasívne prijímať informácie, ktoré si aj tak na internete viete nájsť a AI asistent vám ich vie vysvetliť a sprostredkovať. Sústrediť sa preto budeme na rozvíjanie vašich zručností, analytického uvažovania a kritického myslenia. Na tomto predmete sa budete môcť pýtať, dávať pripomienky a konzultovať vaše riešenia a prístupy s vyučujúcimi. 

![Lafferova krivka umelej inteligencie](../assets/ai-laffer.jpeg){ align=right width=400px}

Vo vašej budúcej práci *budete musieť umelú inteligenciu používať dennodenne*, inak ste nezamestnateľný. Vytváranie softvéru cez AI nebude pridanou hodnotou, ale nutnosťou. Dnešní AI agenti za vás vedia napísať zdrojový kód, opraviť chyby, napísať a vykonať testy a vygenerovať dokumentáciu. Používanie AI asistentov a vyhodnotenie toho, čo napíšu, bude vašou zručnosťou.

Bez AI budete pomalý, ale ak AI agentov necháte robiť všetko za vás, budete vytvárať iba odpad, pomyje (AI slop). Firmy budú hľadať konkurenčnú výhodu a tá bude vždy v ľuďoch, ktorí budú vedieť AI vhodne používať a vyhodnotiť to, čo digitálny asistent vyprodukuje. Táto tzv. AI verifikácia sa stane vyhľadávanou zručnosťou a nedá sa plne automatizovať. Budú ju vedieť robiť iba skúsení a zruční softvérovi inžinieri a firmy budú takýchto ľudí vždy potrebovať.
  </main>

  <aside markdown>
Taktiež je stratou vášho aj nášho času, ak učiteľ pomocou AI asistenta navrhne zadanie, študent ho dá umelej inteligencii vypracovať a nakoniec učiteľ použije AI, aby odovzdanú prácu ohodnotil.
  </aside>
</div>

![Kilocode vo VSCode](../assets/kilocode.png){ .on-glb }
/// caption
Použitie AI agenta Kilo Code a Grok 3 mini vo Visual Studio Code na vytvorenie JavaFX projektu
///

Vedieť programovať má teda zmysel aj vo svete umelej inteligencie. Pri bezbrehom používaní digitálnych agentov sa takto generovaný kód rýchlo stane neprehľadným, ťažkopádnym a k riešeniu vášho problému sa nedopracujete.


## Čo je objektovo orientované programovanie

Programovanie sa vyvíjalo od nízkoúrovňových jazykov k postupne zložitejším paradigmám. Objektovo orientované programovanie predstavuje jednu z nich. 

Paradigma určuje spôsoby, akým by sa malo riešenie danej úlohy - *implementácia* - vyjadriť a zapísať v danom programovacom jazyku. Programovacích paradigiem je veľké množstvo a jeden programovací jazyk ich často podporuje viacero. V jazyku Python a aj v jazyku Java sa stretneme s nasledovnými:

* *Štruktúrované programovanie* - používanie sekvencie príkazov, podmienok, cyklov, a blokov kódu
* *Procedurálne programovanie* - rozdelenie kódu na podprogramy či funkcie, ktoré sa vedia navzájom volať 
* *Objektovo orientované programovanie* - používanie objektov, zapuzdrenia a polymorfizmu
* *Funkcionálne programovanie* - funkcie bez názvu, schopnosť funkciu posielať ako argument, alebo návratovú hodnotu
* *Súbežné programovanie* - rozdelenie programu do častí, ktoré sú vykonávane súbežne, teda úlohy sú vykonávané súčasne naraz

<div class="md-has-sidebar" markdown>
  <main markdown>
Na predchádzajúcich predmetoch ste sa naučili technikám štruktúrovaného a procedurálneho programovania. Tie budete využívať aj naďalej a k nim na tomto predmete pridáme programovanie objektovo orientované.

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, a učím sa objektové programovanie. Uveď [krátku históriu rôznych typov programovacích jazykov](https://chatgpt.com/share/68b363bf-4888-8011-bf48-d9c73d8e0115).

Objektovo orientované programovanie vzniklo z potreby zvládať komplexnosť. OOP sa snaží modelovať programy prirodzenejšie, podľa sveta okolo nás. Umožňuje nám organizovať kód intuitívnym spôsobom.

Objektovo orientované programovanie nám prináša nové možnosti abstrakcie. Pomocou nich vieme zložité systémy zjednodušiť tak, že sa zameriame na podstatné vlastnosti a nepotrebné detaily skryjeme. 

Cieľom zavedenia objektovo orientovaného programovania je tiež zlepšenie organizácie kódu a znovupoužiteľnosť. Raz napísaný kód objektu môžeme použiť znovu na iných miestach alebo aj neskôr v iných programoch. Ak pracujete v tíme, pomocou objektov si budete vedieť problém lepšie rozdeliť medzi seba.
  </main>

  <aside markdown>
Objektové programovanie sa stalo populárnym v 90tych rokov minulého storočia a dlhú dobu bolo hlavným spôsobom tvorby softvéru. Jeho používanie pretrvalo dodnes a má svoje reálne uplatnenie. Niektoré jeho aspekty sa však po rokoch ukázali ako príliš zložité alebo nevhodné a v dnešnom modernom programovaní sa prestávajú používať. 

V prípadoch, kde je použitie OOP príliš rozvláčne alebo pridáva zbytočnú zložitosť sa v súčasnosti využívajú iné paradigmy ako napríklad programovanie funkcionálne, deklaratívne alebo sa použijú malé doménovo špecifické jazyky (DSL)
  </aside>
</div>


## Programovací jazyk Java

<div class="md-has-sidebar" markdown>
  <main markdown>
  ![Java logo](../assets/java.svg){ align=left width=100px }

  Java patrí dlhodobo medzi najpopulárnejšie a najväčšie programovacie jazyky a softvérové vývojové platformy. Za svoju dlhú históriu prešiel mnohými zmenami a vylepšeniami a aj dnes dokáže obstáť v konkurencii iných moderných jazykov. Objektovo orientované zameranie patrí medzi jeho základné vlastnosti, ale programátorom umožňuje používať aj iné paradigmy.

  Medzi najväčšie konkurenčné výhody platformy Java patrí:

  * Špičkový virtuálny stroj Java Virtual Machine (JVM), ktorý umožňuje spúšťať Java programy nezávisle od operačného systému alebo hardvéru 
  * Mohutná sada štandardizovaných knižníc Java API, ktoré môžu vývojári použiť na bežné programovacie úlohy bez toho, aby museli písať kód od začiatku
  * Obrovský ekosystém knižníc, frameworkov a nástrojov, rokmi otestovaných v praxi
</main>
  <aside markdown>
Prieskum najnovších trendov a technológií v oblasti softvérového vývoja nájdete v [The 2025 Developer Survey](https://survey.stackoverflow.co/2025/) firmy Stack Overflow.
  </aside>
</div>

Pre ekosystém a platformu Java sú typické nasledovné vlastnosti:

* *Platformová nezávislosť* - Kód sa prekladá do tzv. bajtkódu (bytecode) a beží na JVM. Mottom Javy je Napíš raz, spusti všade (Write Once, Run Anywhere, WORA).
* *Statické typovanie* - typy premenných sú známe a kontrolované už pri kompilácii
* *Dlhodobá stabilita a kompatibilita, vysoká bezpečnosť* - Staré Java programy často bežia aj na nových verziách JVM bez úprav
* *Podpora paralelizmu, multithreadingu a distribuovaných systémov* - Zabudovaná podpora pre viacvláknové aplikácie a súbežné spracovanie dát.
* *Všestrannosť použitia, škálovateľnosť a vysoký výkon* - Java je vhodná pre webové, mobilné a aj veľké podnikové aplikácie

Java sa dnes využíva hlavne vo veľkých enterprise systémoch, webových back-endoch, cloudových aplikáciách a finančných systémoch. Je taktiež aj dominantnou platformou na vývoj mobilných Android aplikácií. Tam je populárnym jazyk Kotlin, ktorý beží nad platformou Java a ponúka jednoduchšiu syntax.

Hlavné nevýhody Javy sú nižší výkon v porovnaní s low-level jazykmi a vyššia spotreba pamäte. Ďalej je to "verbózna", teda príliš ukecaná syntax, kedy často aj jednoduchá vec je zapísaná na desiatkách riadkov. Z dôvodu použitia JVM majú programy v Jave pomalší štart aplikácií (to sa však dnes dá riešiť špeciálnym kompilátorom). Statické typovanie a mohutnosť jazyka, knižníc a nástrojov sú často pre začínajúcich programátorov zložité na pochopenie.

## Zhrnutie teórie

- [x] Asistenti a agenti umelej inteligencie majú v programovaní svoje miesto
    * [ ] Výsledky, ktoré nám AI dá, je potrebné verifikovať
    * [ ] Slepá dôvera v AI vedie k produkovaniu AI odpadu
- [x] Objektovo orientované programovanie ako paradigma
    * [ ] Je spôsob, ako zvládať komplexnosť programov
    * [ ] Modeluje svet podobne ako ho vnímame my
    * [ ] Zavádza používanie objektov, zapuzdrenia a polymorfizmu
    * [ ] Implementácia - praktické uskutočnenie alebo naprogramovanie určitého návrhu alebo algoritmu
    * [ ] Abstrakcia - proces, pri ktorom sa zameriavaš len na dôležité vlastnosti a skrývaš nepodstatné detaily.
- [x] Programovací jazyk Java
    * [ ] Možnosť používať objektovo orientované a aj funkcionálne programovanie
    * [ ] Používa statické typovanie, kde typy premenných sú známe a kontrolované už pri kompilácii
    * [ ] Dlhodobo stabilný a kompatibilný, s vysokou bezpečnosťou
    * [ ] Virtuálny stroj Java Virtual Machine (JVM) umožňuje spúšťať programy nezávisle od operačného systému alebo hardvéru
    * [ ] Ponúka mohutnú sada knižníc, nástrojov a štandardizované Java API
    * [ ] Dominuje v podnikových aplikáciách a v prostredí Android

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Učebnica na stránke oop.wagjo.com


    DIGITÁLNY ASISTENTI

    AI Asistent - reaktívny, odpovedá na otázky, jednoduché úlohy
    AI Agent - proaktívny, autonómny, zložité viackrokové úlohy

    AI Prompting - vytváranie inštrukcií pre AI
    AI Verifikácia - kontrola presnosti, spoľahlovosti a kvality výstupu
                   - vždy riadená človekom

    AI Slop - odpad, vznikne ak AI slepo dôverujem


    OBJEKTOVO ORIENTOVANÉ PROGRAMOVANIE

    Paradigmy v programovaní:
    - Štruktúrované: sekvencie príkazov, podmienky, cykly
    - Procedurálne: podprogramy, ktoré sa vedia navzájom volať
    - Objektovo orientované: objekty, zapuzdrenie, polymorfizmus
    - Funkcionálne: anonymné funkcie, high-order funkcie

    OOP:
    - modeluje podľa nášho vnímania sveta
    - zvláda väčšiu komplexnosť
    - umožňuje znovupoužitie
    - intuitívna organizácia kódu


    JAZYK JAVA

    Vlastnosti:
    - objektovo orientovaný
    - univerzálny
    - platformovo nezávislý 
    - staticko typovaný: typy premenných sú kontrolované pri kompilácii 

    Použitie:
    - veľké podnikové systémy
    - webový back-end
    - Android aplikácie (jazyk Kotlin)
    - cloudové a distrubuované systémy

    Platformová nezávislosť:
    - kód sa kompiluje do bytecode a beží nad JVM
    - motto Javy: Write Once, Run Anywhere (WORA)
    - virtuálny stroj JVM (Java Virtual Machine)
    - Java API: vždy dostupná sada knižníc na bežné použitie
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Ústne skúšanie alebo krátka 5-minútovka:

    - Aká je úloha programátora vo svete, kde sa používajú AI asistenti a agenti?
    - Aké rôzne programovacie paradigmy poznáme?
    - Z akej potreby vzniklo objektové programovanie?
    - Čo nové nám OOP prináša?
    - Čo znamená: Napíš raz, spusti všade (Write Once, Run Anywhere, WORA)?
    - Aké sú typické vlastnosti jazyka Java?
    - Bonus: Aké sú nevýhody jazyka Java?
