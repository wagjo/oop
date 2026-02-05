# Teória 20: JavaFX - základné triedy

Dnes si začneme vysvetľovať základné koncepty knižnice JavaFX. Ako prvé si ukážeme, akým spôsobom JavaFX reprezentuje okná operačného systému. 

Oficiálna dokumentácia knižnice JavaFX je na stránke [https://openjfx.io/javadoc/21/](https://openjfx.io/javadoc/21/)

![Aplikácia Word](../assets/word-okna.png){.on-glb}
/// caption
Aplikácia Word s troma oknami
///

Aplikácia sa štandardne skladá z jedného alebo viacerých okien. 
Okná môžu mať rôzne vlastnosti a štýly. Do vnútra okna si aplikácia vykresľuje rôzne grafické a ovládacie prvky.

Pre všetky tieto koncepty má knižnica JavaFX vlastné triedy. Dnes si predstavíme 3 základné triedy JavaFX:

- `Application` - predstavuje hlavnú triedu aplikácie
- `Stage` - reprezentuje jedno okno aplikácie
- `Scene` - má na starosti vnútro okna - čo sa do neho bude vykresľovať

Každý JavaFX program musí mať práve jeden objekt triedy `Application` ale môže mať viacero objektov `Stage` a `Scene`, podľa toho, koľko okien naša aplikácia potrebuje.


## Application

Trieda [`javafx.application.Application`](https://openjfx.io/javadoc/21/javafx.graphics/javafx/application/Application.html) je abstraktná trieda, ktorá reprezentuje hlavnú triedu aplikácie. Náš JavaFX program musí mať práve jednu triedu, ktorá dedí z `Application` a implementuje abstraktnú metódu `void start(Stage primaryStage)`

Hlavná JavaFX logika má byť umiestnená do metódy `start`, ktorá sa automaticky zavolá po spustení programu. Táto metóda má jeden vstupný argument, a tým je hlavné okno aplikácie. Hlavné okno sa teda vytvorí automaticky a je našou úlohou správne ho nastaviť a zobraziť.

V metóde `start` sa štandardne robia nasledovné veci:

- Nastaví sa titulok hlavného okna
- Nastaví sa štýl hlavného okna
- K hlavnému oknu sa pripojí konkrétna scéna s ovládacími a grafickými komponentami
- Dá sa pokyn na zobrazenie hlavného okna


## Stage

Trieda [`javafx.stage.Stage`](https://openjfx.io/javadoc/21/javafx.graphics/javafx/stage/Stage.html) reprezentuje konkrétne okno aplikácie.

Každá JavaFX aplikácia má práve jedno hlavné okno, ktoré sa vytvorí automaticky. Nie je teda potrebné vytvárať objekt triedy `Stage` ručne.

Všetky zmeny do vlastností okna je potrebné robiť pred jeho samotným zobrazením. Zobrazenie okna sa robí pomocou metódy `stage.show()`

Titulok okna sa nastaví pomocou metódy `stage.setTitle("Titulok")`

Medzi ďalšie užitočné nastavenia patria nastavenie minimálnej a maximálnej veľkosti okna (napr. `setMaxWidth()`) a nastavenie, či chceme aby užívateľ mohol meniť veľkosť okna (`setResizable()`). Stage v sebe taktiež obsahuje informácie o tom, či je okno maximalizované, minimalizované a či je full screen.

Do okna sa nedávajú grafické komponenty priamo, ale oknu sa priradí konkrétna scéna, ktorá obsahuje samotné komponenty. Priradenie scény do okna sa robí pomocou metódy `stage.setScene(scene)`

### Hierarchia okien

Ak má naša aplikácia okien viacero, ostatné okná si už vytvoríme ručne a pridáme ich ako vedľajšie okná do aplikáce.

Každé okno okrem hlavného musí mať svojho vlastníka, teda nejaké nadradené okno, pod ktoré patrí. Týmto sa vytvára stromová hierarchia okien. Prepojenie vedľajšieho okna s hlavným robíme pomocou metódy `stage.initOwner(ownerStage)`.

Ak sa zatvorí okno tak JavaFX automaticky zatvorí aj všetky jeho podradené okná. Ak sa zatvorí hlavné okno, celá JavaFX aplikácia končí.

### Štýl okna

Okno môže mať viacero štýlov. Pre štýly má JavaFX enum `StageStyle`. Ukážeme si dva najzákladnejšie.

- `StageStyle.DECORATED` - klasické okno s titulkom, okrajom a ovládacími prvkami na vrchu (zatvorenie okna, maximalizácia, minimalizácia)
- `StageStyle.UNDECORATED` - okno bez okraja a bez ovládacích prvkov, štandardne používané na tzv. **splash screen**

![Decorated](../assets/decorated-stage.png){.on-glb width=500}
/// caption
Klasické okno
///

![Roblox](../assets/splash-screen.png){.on-glb width=500}
/// caption
Splash screen hry Roblox
///

Nastavenie štýlu okna vykonáme pomocou metódy `stage.initStyle(style)`. Ak chceme klasický DECORATED štýl, nemusíme nič nastavovať, v JavaFX je nastavený defaultne.


### Modalita okna

Pre vedľajšie okná vieme nastaviť tzv. **modalitu okna**. Modalita určije, ako veľmi vedľajšie okno blokuje interakciu s ostatnými oknami aplikácie. Inak povedané: ktoré okná môže používateľ ovládať, kým je dané okno otvorené

Ak je okno modálne, ostatné okná aplikácie sú "zamknuté", kým sa modálne okno nazavrie.

Poznáme tri druhy modality:

- `Modality.NONE` - Žiadna modalita, okno neblokuje nič, používateľ môže klikať na všetky ostatné okná.
- `Modality.WINDOW_MODAL` - Okno blokuje svoje rodičovské okno. Ostatné okná aplikácie sú klikateľné.
- `Modality.APPLICATION_MODAL` - Najsilnejšia modalita, blokované sú všetky okná aplikácie.

Klasická modalita sa využíva pri tzv. Dialog oknách, napr. pri oknách s nastaveniami.

Okná bez modality sa používajú hlavne pre pomocné panely, okná nástrojov a náhľadové okná.

Silná modalita sa používa hlavne pre potvrdenia (Chcete ukončiť aplikáciu?), prihlasovanie, otváranie súboru, a chybové hlášky.

Modalita okna sa nastavuje pomocou metódy `stage.initModality(modality)`. Defaultne je nastavaná na `Modality.NONE`.


## Scene

Do samotného okna nevykresľujeme priamo, ale všetky grafické prvky a ovládacie komponenty vkladáme to tzv. scény. Scéna je objekt triedy [`javafx.scene.Scene`](https://openjfx.io/javadoc/21/javafx.graphics/javafx/scene/Scene.html). Pri vytváraní scény si určujeme aj konkrétne rozmery okna.

Pripojenie scény do okna robíme pomocou metódy `stage.setScene(scene)`. Pomocou tejto metódy môžeme v okne aj prepínať medzi scénami, ak chceme v okne začať zobrazovať niečo iné.

Scene v sebe obsahuje všetko, čo sa má do okna vykresliť. Do scene sa to umiestňuje vo forme stromovej štruktúry, kde každý komponent je jeden uzol v strome. O štruktúre scény a konkrétnych grafických a ovládacích prvkoch si bližšie povieme na budúcej hodine.

## JavaFX aplikácia ako kino Cinemax

Na lepšie pochopenie princípov JavaFX aplikácie si vieme dať tieto 3 hlavné triedy do analógie s multikinom Cinemax.

Objekt JavaFX `Application` reprezentuje našu aplikáciu - kino Cinemax v Novume v Prešove.

Tak ako má Cinemax viacero kinosál, tak aj naša aplikácia môže mať viacero okien. Jedna kinosála - JavaFX okno, je reprezentované objektom triedy `Stage`.

Do okna vie naša JavaFX aplikácia vykresľovať rôzne komponenty a grafické objekty. To, čo sa práve do okna vykresľuje sa v JavaFX nazýva `Scene`. V našej analógii s Cinemax to zodpovedá filmu, ktorý sa v danej kinosále premieta.

Tak ako v kinosále vieme premietať aj iný film, tak aj v `Stage` okne vieme meniť rôzne `Scene` - to, čo sa v okne aktuálne zobrazuje. 


## Launcher

Ak spúšťame JavaFX aplikáciu klasicky cez vstupný bod programu `main`, je vhodné pre tento main vytvoriť samostatnú triedu a nedávať ju do triedy aplikácie. JavaFX má totiž často s tým problém a samostatná trieda je bezpečnejšie riešenie. 

Túto triedu si môžeme nazvať napr. `Launcher` alebo podobne. V samotnej `main` metóde potom spustenie JavaFX aplikácie vykonáme pomocou príkazu `MyApplication.launch(MyApplication.class, args)`, kde `MyApplication` je názov triedy s aplikáciou a args sú vstupné argumenty z `main` metódy.


## Zhrnutie teórie

V repozitári na adrese [https://github.com/wagjo/opg-gui](https://github.com/wagjo/opg-gui) máte ukážku práce s triedami `Scene`, `Stage` a `Application`. Ide o triedy `FxStageExampleApplication` a `FxStageExampleMain`, pomocou ktorej viete aplikáciu spustiť.

- [x] JavaFX aplikácia
    * [ ] Trieda `Application` - predstavuje hlavnú triedu aplikácie
    * [ ] Trieda `Stage` - reprezentuje jedno okno aplikácie
    * [ ] Trieda `Scene` - má na starosti vnútro okna - čo sa do neho bude vykresľovať
    * [ ] Každý JavaFX program musí mať práve jeden objekt triedy Application ale môže mať viacero objektov Stage a Scene, podľa toho, koľko okien naša aplikácia potrebuje.
- [x] javafx.application.Application
    * [ ] Náš JavaFX program musí mať práve jednu triedu, ktorá dedí z `Application` a implementuje abstraktnú metódu void `start(Stage primaryStage)`
    * [ ] Hlavná JavaFX logika má byť umiestnená do metódy `start`, ktorá sa automaticky zavolá po spustení programu.
    * [ ] Metóda `start` má jeden vstupný argument, a tým je automaticky vytvorené hlavné okno aplikácie
    * [ ] V metóde `start` sa nastavuje titulok a štýl okna, pripája sa scéna a nakoniec sa dá pokyn na zobrazenie hlavného okna
- [x] javafx.stage.Stage
    * [ ] Okno aplikácie
    * [ ] Každá JavaFX aplikácia má práve jedno hlavné okno, ktoré sa vytvorí automaticky
    * [ ] Všetky zmeny do vlastností okna je potrebné robiť pred jeho samotným zobrazením.
    * [ ] `stage.show()` - zobrazenie okna
    * [ ] `stage.setTitle("Titulok")` - nastavenie titulku
    * [ ] `stage.setMaxWidth()` a pod. - nastavenie max a min veľkosti okna
    * [ ] `stage.setResizable(true)` - Nastavenie, či užívateľ môže meniť veľkosť okna
- [x] Hierarchia okien
    * [ ] Ak má naša aplikácia okien viacero, ostatné okná si už vytvoríme ručne a pridáme ich ako vedľajšie okná do aplikáce.
    * [ ] Každé okno okrem hlavného musí mať svojho vlastníka, teda nejaké nadradené okno, pod ktoré patrí. 
    * [ ] `stage.initOwner(ownerStage)` - prepojenie vedľajšieho okna z hlavným.
    * [ ] Ak sa zatvorí okno tak JavaFX automaticky zatvorí aj všetky jeho podradené okná. Ak sa zatvorí hlavné okno, celá JavaFX aplikácia končí.
- [x] Štýl okna
    * [ ] `stage.initStyle(style)` - nastaví štýl, defaulne `StageStyle.DECORATED`
    * [ ] `StageStyle.DECORATED` - klasické okno s titulkom, okrajom a ovládacími prvkami na vrchu (zatvorenie okna, maximalizácia, minimalizácia)
    * [ ] `StageStyle.UNDECORATED` - okno bez okraja a bez ovládacích prvkov, štandardne používané na tzv. splash screen
- [x] Modalita okna
    * [ ] Modalita určije, **ako veľmi vedľajšie okno blokuje interakciu s ostatnými oknami** aplikácie.
    * [ ] `stage.initModality(modality)` - nastaví modalitu, defaultne `Modality.NONE`
    * [ ] `Modality.NONE` - Žiadna modalita, okno neblokuje nič, používateľ môže klikať na všetky ostatné okná.
    * [ ] `Modality.WINDOW_MODAL` - Okno blokuje svoje rodičovské okno. Ostatné okná aplikácie sú klikateľné.
    * [ ] `Modality.APPLICATION_MODAL` - Najsilnejšia modalita, blokované sú všetky okná aplikácie.
- [x] javafx.scene.Scene
    * [ ] Do samotného okna nevykresľujeme priamo, ale všetky grafické prvky a ovládacie komponenty vkladáme to tzv. scény. 
    * [ ] Pri vytváraní scény si určujeme aj konkrétne rozmery okna.
    * [ ] `stage.setScene(scene)` pripojenie scény k oknu
- [x] JavaFX aplikácia ako kino Cinemax
    * [ ] Cinemax v Novume - objekt triedy `Application`
    * [ ] Konkrétna kinosála v Cinemaxe - objekt triedy `Stage`
    * [ ] Film, ktorý sa práve premieta v kinosále - objekt triedy `Scene` pripojený k oknu aplikácie
    * [ ] Tak ako má Cinemax viacero kinosál, tak aj naša aplikácia môže mať viacero okien
    * [ ] Tak ako v kinosále vieme premietať aj iný film, tak aj v Stage okne vieme meniť rôzne Scene - to, čo sa v okne aktuálne zobrazuje. 
- [x] Launcher
    * [ ] Vstupný bod programu metódu `main` je dobré mať v samostatnej triede, mimo triedu `Application`
    * [ ] Spustenie JavaFX aplikácie vykonáme pomocou príkazu `MyApplication.launch(MyApplication.class, args)`

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    JavaFX aplikácia - 3 základné triedy:
    - Application - hlavná trieda aplikácie
    - Stage - reprezentuje jedno okno aplikácie
    - Scene - má na starosti vnútro okna - čo sa do neho bude vykresľovať

    Hlavná JavaFX logika sa píše do metódy start v triede dediacej od Application

    Stage - Okno aplikácie
    - Aplikácia má jedno hlavné okno, ktoré sa vytvorí automaticky
    - Všetky zmeny je potrebné robiť pred zobrazením okna.
    - stage.show() - zobrazenie okna
    - stage.setTitle("Titulok") - nastavenie titulku

    Hierarchia okien
    - Každé okno okrem hlavného musí mať svojho vlastníka, teda nadradené okno, pod ktoré patrí. 
    - stage.initOwner(ownerStage) - prepojenie vedľajšieho okna s hlavným.

    Štýl okna
    - stage.initStyle(style) - nastaví štýl
    - StageStyle.DECORATED - klasické okno s titulkom, okrajom a ovládacími prvkami na vrchu
    - StageStyle.UNDECORATED` - okno bez okraja a bez ovládacích prvkov, používané na splash screen

    Modalita okna
    - Určuje ako veľmi vedľajšie okno blokuje interakciu s ostatnými oknami aplikácie.
    - stage.initModality(modality) - nastaví modalitu
    - Modality.NONE - Žiadna modalita, okno neblokuje nič
    - Modality.WINDOW_MODAL - okno blokuje svoje rodičovské okno
    - Modality.APPLICATION_MODAL - najsilnejšia modalita, blokované sú všetky okná aplikácie.

    Scene - čo sa do okna vykreslí
    - Do okna nevykresľujeme priamo, ale ovládacie komponenty vkladáme to scény. 
    - Pri vytváraní scény si určujeme aj konkrétne rozmery okna.
    - stage.setScene(scene) - pripojenie scény k oknu
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Na ďalšej hodine budeme kontrolovať nasledovné veci:

    - Zapísané poznámky z hodiny vo vašom zošite

    Okruhy otázok na test:

    - Triedy Application, Stage a Scene - význam a použitie
    - Základné metódy triedy Stage
    - Aké štýly okna poznáme, na čo sa používajú
    - Čo je modalita a aké druhy poznáme, použitie
