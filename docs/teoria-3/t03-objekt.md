# Teória 3: Identita, Objekt

Dnes si začneme vysvetľovať základné pojmy z objektovo orientovaného programovania. Ako prvé si vysvetlíme, čo je objekt. Okrem toho si povieme niečo o veciach ako identita, príkaz vs výraz a ukážeme si základné operátory v Jave.


## Identita a Hodnota

Číslo 15 je údaj, hodnota. Môže to byť počet hodín, alebo vek človeka. Samotné číslo 15 je však iba hodnota, nič iné v sebe neobnáša. Podobne aj slovo "Fero", ak ho čítame v kalendári, je to mužské meno. Môže ho mať veľa ľudí, alebo dokonca to môže byť názov firmy. Ak iba uvedieme "Fero", je to len meno bez konkrétneho človeka. V angličtine na také názvy máme neurčitý člen *"a"*, napríklad *a pen*, *a chair*.

<div class="md-has-sidebar" markdown>
<main markdown>
Niekedy však máme na mysli konkrétnu vec. Ak si chceme opraviť známku 5 v žiackej knižne, nemyslíme na hocijakú päťku, ale práve na tú našu, ktorú sme pred týždňom dostali. Alebo ak povieme, "Fero mi nevrátil knihu", myslíme na konkrétneho človeka a na konkrétnu knihu. V angličtine pre takéto prípady máme člen *"the"*, napríklad *the book* alebo *the student*.

Identita (anglicky identity) reprezentuje jedinečnosť danej veci, jej výnimočnosť. Identita veci sa v čase nemení, ale hodnota (anglicky value) o danej veci sa meniť môže. Ak si môj spolužiak zmení meno z Fero na Hugo, bude to stále ten istý človek. Ak si prestriekam svoje auto na inú farbu, stále to bude to moje auto.

!!! info "Filozofické okienko"

    Identita versus vlastnosť je častým predmetom filozofických debát. Veľmi známy je grécky paradox [Theseovej lode](https://en.wikipedia.org/wiki/Ship_of_Theseus). Predstav si, že máš novú loď, s ktorou sa plavíš po mori. Jej drevené časti však začínajú postupne hniť a si nútený vymieňať ich. Ak na lodi postupne vymeníš všetky jej časti (dosky, klince, plachty) za nové, je to stále tá istá loď?

Identita je veľmi dôležitá vec. Ak máme v triede dvoch "Ferov", identita je to, čo nám pomôže rozlíšiť medzi nimi. Podobne je to aj v programovaní. Niekedy pracujeme s hodnotou, a niekedy potrebujeme porovnať identitu daných vecí.

Všetky **primitívne dátove typy v Jave majú hodnoty bez identity**. Ak vytvoríme premennú typu int, hodnoty tejto premennej nemajú identitu a nevieme sa opýtať, ktorá päťka to je.

```java title="Porovnanie primitívnych hodnôt"
  int a = 5;
  int b = 5;
  System.out.println(a == b); // true, porovnala sa hodnota premenných
```

  </main>

  <aside markdown>
!!! quote "Terry Pratchett: [Pátý Elefant](https://www.martinus.sk/28961-paty-elefant/kniha)"
    „Tohle, mylorde, je dědičná sekera naší rodiny. Dědí se u nás z pokolení na
    pokolení přes devět set let. Je pochopitelné, že během toho času několikrát
    potřebovala novou hlavici. A mezitím také novou rukojeť, drobné úpravy
    tvaru a obnovu některých ornamentů…, ale není to snad stále devět set let
    stará sekera naší rodiny? A protože se měnila během času jen pomalu, je to
    pořád ještě skvělá sekera, abyste věděl. Velmi skvělá. Chcete mi snad říci,
    že i tohle je padělek?“  
</aside>
</div>

## Objekt

Ak chceme mať v Jave niečo, čo so sebou nesie aj identitu, musíme použiť objekt.

Objekt v Jave je vec, ktorá

- **má svoju identitu** - je jedinečná entita
- **nesie stav** - má svoje konkrétne hodnoty - atribúty
- **poskytuje správanie** - poskytuje metódy, ktoré môžeme nad objektom volať
- **je inštanciou triedy** - je konkrétnym výtvorom z abstraktného návrhu triedy

<div class="md-has-sidebar" markdown>
<main markdown>

Všetky **neprimitívne dátove typy vytvárajú objekty**. Ak vytvoríme dva rôzne objekty rovnakého typu, s rovnakou hodnotou, budú mať rozdielnu identitu.

```java title="Porovnanie objektov"
String s1 = new String("Fero");
String s2 = new String("Fero");
System.out.println(s1 == s2); // false, porovnali sa identity
System.out.println(s1.equals(s2)); // true, porovnali sa hodnoty
```

Objekty vytvárame pomocou operátora `new`. Pri takomto vytváraní sa zavolá špeciálna metóda, konštruktor, ktorá nám inicializuje stav objektu, teda všetky jeho atribúty.

Identitu objektov porovnávame pomocou operátora `==`. (Pri primitívnych typoch sa porovná ich hodnota).

Hodnotu objektov porovnávame pomocou metódy `equals()`. Ak chceme, aby toto porovnanie fungovalo aj pre naše vlastné triedy, musíme si napísať vlastnú metódu `equals()`


  </main>

  <aside markdown>
Viac o triedach a objektoch si povieme na cvičení, kde si vyskúšame vytvoriť svoje vlastné triedy. 
</aside>
</div>

## Operátory

Java má viac ako 40 operátorov. Sú to špeciálne symboly, ktoré vykonávajú určitú operáciu nad jednou alebo viacerými hodnotami.

<div class="md-has-sidebar" markdown>
<main markdown>

Medzi základné operátory patria:

- aritmetické operátory: `+`, `-`, `/`, `*`, `%`, `++`, `--`
- logické operátory: `||`, `&&`
- operátory priradenia: `=`, `+=`, `-=`, `*=`, `/=`
- relačné operátory: `>`, `<`, `>=`, `<=`
- operátory rovnosti: `==,` `!`
- operátor vytvorenia objektu: `new`


  </main>

  <aside markdown>
Niektoré operátory sa dajú použiť viacerými spôsobmi. Napríklad operátor inkrementácie čísel `++` sa dá použiť v dvoch formách: prefixovej `++a` alebo postfixovej `a++`. Je rozdiel v správaní, pri postfixovej sa vráti pôvodná hodnota, pri prefixovej tá zvýšená.</aside>
</div>


!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Javu. Vytvor mi [podrobnú tabuľku všetkých operátorov, s ich názvom, kategóriou a s príkladom použitia](https://grok.com/share/c2hhcmQtMg%3D%3D_a8bdc384-5adc-482b-b030-66c210563cb6).


## Príkazy a výrazy


<div class="md-has-sidebar" markdown>
<main markdown>

Java rozlišuje medzi príkazmi (statements) a výrazmi (expressions).

Výraz je kombinácia hodnôt, premenných, operátorov a volaní metód, ktorá po vyhodnotení vráti hodnotu. Výrazy môžu byť súčasťou príkazov.

Príklad výrazov: `5`, `a + 3`, `s.length()`

Príkaz je samostatná jednotka vykonania, ktorá niečo robí. Príkazy nevracajú hodnotu.

Príklad príkazov: podmienky a cykly (`if`, `while`, `for`), `return`, `break`, `System.out.println()`

Volanie metódy môže byť aj ako výraz aj ako príkaz. Ako výraz však môže byť iba taká metóda, ktorá vracia hodnotu. (teda nesmie mať `void` návratový typ)

  </main>

  <aside markdown>
Niektoré príkazy majú špeciálnu výrazovú formu. Napríklad príkaz `if` má ternárny operátor, pre použitie podmienky ako výrazu. Alebo napríklad deklarácia metód má svoju verziu vo forme výrazu, tzv. lambda výraz, napríklad `x -> x * x`. Takéto lambda výrazy sa používajú ak sa v Jave programuje vo funkcionálnom štýle.
</aside>
</div>

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Javu. [Prečo v Jave rozlišujeme medzi príkazmi a výrazmi, na čo je to dobré?](https://grok.com/share/c2hhcmQtMg%3D%3D_9ccfb967-8ab2-47aa-886c-fafdbfc785db).


## Zhrnutie teórie

- [x] Identita (anglicky identity) reprezentuje jedinečnosť danej veci, jej výnimočnosť.  
    * [ ] Identita veci sa v čase nemení, ale hodnota (anglicky value) danej veci sa meniť môže
    * [ ] Všetky primitívne dátove typy v Jave majú hodnoty bez identity
    * [ ] Hodnoty primitívnych typov porovnávame pomocou operátora ==
- [x] Objekt v Jave je základný stavebný blok, ktorý
    * [ ] má svoju identitu - je jedinečná entita
    * [ ] nesie stav - má svoje konkrétne hodnoty - atribúty
    * [ ] poskytuje správanie - poskytuje metódy, ktoré môžeme nad objektom volať
    * [ ] je inštanciou triedy (class) - je konkrétnym výtvorom z abstraktného návrhu triedy
- [x] Všetky neprimitívne dátove typy vytvárajú objekty.
    * [ ] Objekty vytvárame pomocou operátora new
    * [ ] Identitu objektov porovnávame pomocou operátora ==
    * [ ] Hodnoty objektov porovnávame pomocou metódy equals()    
- [x] Java má viac ako 40 operátorov.
    * [ ] aritmetické operátory: `+`, `-`, `/`, `*`, `%`, `++`, `--`
    * [ ] logické operátory: `||`, `&&`
    * [ ] operátory priradenia: `=`, `+=`, `-=`, `*=`, `/=`
    * [ ] relačné operátory: `>`, `<`, `>=`, `<=`
    * [ ] operátory rovnosti: `==,` `!`
    * [ ] operátor vytvorenia objektu: `new`
- [x] Príkazy (statements) a výrazy (expressions)
    * [ ] Výraz je kombinácia hodnôt, premenných, operátorov a volaní metód, ktorá po vyhodnotení vráti hodnotu. 
    * [ ] Výrazy môžu byť súčasťou príkazov.
    * [ ] Príkaz je samostatná jednotka vykonania, ktorá niečo robí. Príkazy nevracajú hodnotu.
    

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Identita reprezentuje jedinečnosť danej veci. 
    Hodnota/vlastnosť veci sa meniť môže, identita ostáva.

    Objekt
    - má svoju identitu - jedinečnosť
    - nesie stav - atribúty
    - poskytuje správanie - metódy
    - je inštanciou triedy (class) - má typ

    Nové objekty vytvárame pomocou operátora new
    - == porovnáva objekty podľa identity, primitívne typy podľa hodnoty
    - equals() porovnáva objekty podľa hodnoty

    Príkazy (statements) a výrazy (expressions)
    - Výraz po vyhodnotení vráti hodnotu
    - Výrazy môžu byť súčasťou príkazov
    - Príkaz je samostatná jednotka vykonania, ktorá niečo robí
    - Príkazy nevracajú hodnotu

    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Ústne skúšanie alebo krátka 5-minútovka:

    - Rozdiel medzi identitou a hodnotou
    - Ako vieme porovnať primitívne hodnoty?
    - Ako vieme porovnať neprimitívne objekty?
    - 4 Vlastnosti objektu
    - Rozdiel medzi príkazom a výrazom
