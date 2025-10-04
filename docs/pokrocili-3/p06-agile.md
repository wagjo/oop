# Pokročílí 6: Agilný vývoj, verziovacie systémy, správa vývoja


## Agilný vývoj

<div class="md-has-sidebar" markdown>
<main markdown>

Ak softvér vyvíja tím ľudí, dohodnú sa na spôsobe spolupráce aby si navzájom rozumeli a nevznikal chaos. Metódy a princípy pri tvorbe softvéru sa s rokmi menili. V súčasnosti je zaužívaný tzv. agilný prístup k vývoju softvéru.

Agilný vývoj softvéru vznikol ako reakcia programátorov na skostnatelé metodiky v korporáciách. S touto filozofiou prišli samotní programátori a zo začiatku boli agilné princípy oslobodzujúce, prinášali viac kreativity a flexibility. Ako na agilné princípy postupne prechádzali veľké firmy a korporácie, stal sa hlavným prístupom vývoja softvéru. Urobil sa z toho však veľký biznis a agilné metodiky sa začali príliš formalizovať, začali byť povrchnými rituálmi a prestali plniť svoju pôvodnú funkciu. Takýmto iba naoko agilným metodikám sa hovorí Agilné divadlo, anglicky *Agile theater*. 

 </main>

  <aside markdown>
  Pred agilným prístupom bol v oblasti softvérového vývoja zaužívaný tzv. *waterfall model*, ktorý mal lineárny proces zberu požiadaviek, návrhu, implementácii, testovania a nasadenia. Kým sa však dostal do bodu nasadenia, jeho oneskorenie bolo často také veľké, že bol softvér nepoužiteľný. Požiadavky sa totiž v čase menia a konkurencieschopná firma musí vedieť reagovať na zmeny.  
</aside>
</div>


!!! tip "Učím sa s pomocou umelej inteligencie"

    [Kritický pohľad na históriu agilných metodík vývoja softvéru a aký je súčasný stav](https://grok.com/share/c2hhcmQtMg%3D%3D_b57203e2-b196-4e8c-b868-abef7380a961)


<div class="md-has-sidebar" markdown>
<main markdown>


Aj napriek súčasenému častému zneužívaniu agilných metodík v korporáciách a veľkých firmách majú tieto prístupy stále veľký prínos, samozrejme iba ak sa používajú správne. Základné princípy agilného vývoja sú vyjadrené v [Manifeste agilného vývoja softvéru](https://agilemanifesto.org/iso/sk/manifesto.html):

- **Ľudia a komunikácia** sú viac než procesy a nástroje
- **Funkčný softvér** je viac než vyčerpávajúca dokumentácia
- **Spolupráca so zákazníkom** je viac než dojednávanie zmluvy
- **Radšej reagovať na zmenu** než sa držať plánu.

*Aj keď časť napravo je dôležitá, viac si ceníme ľavú časť.*

 </main>

  <aside markdown>
  Príkladom úspešného používania agilných prístupov vo veľkých korporáciách sú Tesla a SpaceX, firmy Elona Muska a firma Valve, ktorú vlastní Gabe Newell. Prečítajte si ich [príručku pre nových zamestnancov](https://www.valvesoftware.com/en/publications)</aside>
</div>


<div class="md-has-sidebar" markdown>
<main markdown>

V centre agilného vývoja je teda flexibilita kvôli lepšiemu reagovaniu na zmeny na trhu a krátke iterácie kvôli lepšiemu pochopeniu potrebám zákazníkov. Tento prístup má však aj niekoľko podstatných nedostatkov, ktoré sa musíme snažiť pri svojej práci čo najviac eliminovať.

- **Nedostatok dokumentácie a plánovania**, dôsledkom častých zmien a rýchlej iterácii. Vedie k nedostatku štruktúry projektu a veľmi ťažkému prijímaniu nových ľudí (onboarding)
- Scope creep - **neustále pridávanie nových funkcionalít**. Zákazníci často sami nevedia, čo potrebuj. Bez jasnej vízie sa projekt rýchlo nafúkne
- **Prílišná byrokracia**, ceremónia a rituály - manažment zneužíva agilné postupy na tvorbu výkonnostných metrík a deadline-ov 

Väčšina z nedostatkov agilného prístupu sa dá eliminovať, ak má projekt **jasnú víziu**, **lídra** a ľudia v tíme majú **možnosť vlastniť** (anglicky own) nejakú časť projektu, teda mať možnosť rozhodovať ale aj **byť zodpovedný** sa nejakú konkrétnu časť.

 </main>

  <aside markdown>
Pri vyzretejších a stabilnejších projektoch sa v súčasnosti používajú hybridné prástupy, kombinujúce agilnú metodiku s waterfall modelom. Pre nové startupové a výskumné projekty sú však správne agilné prístupy stále tou najlepšou alternatívou.
</aside>
</div>

Programovací jazyk Python je svojou dynamickou povahou veľmi vhodný na použitie agilných prístupov. Rýchlo sa v ňom tvoria a overujú nové nápady, má výbornú podporu pre automatizované testovanie a nástroje pre priebežné nasadzovanie softvéru.

V oblasti softvérového vývoja sa osvedčilo niekoľko nástrojov a techník, ktoré pomáhajú a uľahčujú používanie agilných prístupov.

Prvou technikou je **Kanban**, v ktorom si úlohy rozdelíme do roznych stĺpcov, ktoré reprezentujú stav, v akom sa úloha nachádza. Tradičné stavy sú *"Backlog"*, *"TODO"*, *"In development"*, *"QA"* a *"Done"*. Ako sa na úlohe postupne pracuje, tak sa úloha presúva medzi stĺpcami. Populárnymi nástrojmi pre Kanban sú nástroje GitHub Projects, Trello alebo Jira.

![Kanban](../assets/github-zenhub.png){.on-glb}
/// caption
Ukážka nástroja používajúceho Kanban tabuľku
///



<div class="md-has-sidebar" markdown>
<main markdown>

Asi najpopulárnejšou sadou techník pri agilnom programovaní je **SCRUM**, ktorý zavádza určité konkrétne roly a postupy v rámci tímu:

- **Sprint** - mesačné alebo dvojtýždňové iterácie návrhu, vývoja a vyhodnotenia
- **Daily Standup** - pravidelná krátka tímová porada
- **Product Owner** - človek zodpovedný za dodržanie vízie projektu
- **Scrum Master** - asistent, ktorý pomáha, aby v tíme fungovali agilné prístupy 

Najpopulárnejším nástrojom pre podporu techník SCRUM je Jira, ktorá je však často príliš komplikovaná a práca v Jire zaberá veľa času a energie. Pre menšie tímy sú často vhodnejšie agilné nástroje platformy GitHub.

V rámci nasadenia agilných prístupov v softvérovom vývoji často používajú techniky tzv. extrémneho programovania. Ide hlavne o párové programovanie, a Test Driven Development, ktorý dáva dôraz na podrobné testovanie. Prínos týchto techník je však diskutabilný, nakoľko sú pomerne náročné na čas a energiu a tak ich použitie treba dobre zvážiť.

 </main>

  <aside markdown>
Oproti súčastným metodikám agilného vývoja pomocou techník SCRUM sa hlavne v malých tímoch používa tzv. Lean software development. Tento prístup sa snaží o elimináciu zbytočnej byrokracie a porád a zamerieva sa na praktickosť, rýchle prototypovanie a kontinuálne zlepšovanie.
</aside>
</div>

## Verziovacie systémy

## Správa vývoja


1. Agilný vývoj (Agile)

Agile je filozofia riadenia vývoja softvéru. Python projekty sa veľmi často riadia agilne, lebo sa dobre hodí na rýchlu iteráciu a prototypovanie.

Základné princípy:

vývoj po krátkych iteráciách (tzv. sprinty),

spätná väzba od zákazníka alebo užívateľa priebežne,

schopnosť flexibilne reagovať na zmenu požiadaviek,

dôraz na fungujúci kód viac než na dokumentáciu.

Metodiky v rámci Agile:

Scrum → rozdelenie práce na sprinty, role (Product Owner, Scrum Master, vývojársky tím),

Kanban → vizualizácia práce cez tabuľku („To do → In progress → Done“),

Extreme Programming (XP) → dôraz na testovanie, párové programovanie, TDD.

V Pythone sa Agile prejavuje napríklad tým, že:

sa rýchlo tvoria prototypy (Python má veľmi krátky čas od nápadu k funkčnému kódu),

používa sa automatizované testovanie (pytest, unittest),

CI/CD pipeline (GitHub Actions, GitLab CI, Jenkins) umožňuje priebežné nasadzovanie.


Verzovacie systémy

Najbežnejší je dnes Git. Python projekty sú takmer vždy verzované v Gite.

Prečo je to dôležité:

sledovanie histórie zmien,

spolupráca viacerých programátorov,

možnosť vracať sa k starým verziám,

experimentovanie v branches.

Platformy:

GitHub, GitLab, Bitbucket → hosting repozitárov + integrácie pre CI/CD.

Typické workflow v Pythone:

main/master branch pre stabilný kód,

feature branches pre nové funkcie,

pull/merge requesty pre code review,

tagy pre verzie (v1.0.0).

Nástroje:

git (príkazová riadok),

GUI (Sourcetree, GitKraken),

v IDE (PyCharm, VS Code).

3. Správa vývoja projektov

Správa projektu znamená, že nielen kód, ale aj organizácia vývoja je riadená.

Projektové nástroje:

Jira, Trello, Asana, GitHub Projects, GitLab Boards – na správu úloh a sprintov,

Slack, MS Teams, Discord – komunikácia v tíme,

Confluence, Notion – dokumentácia.

V Pythone konkrétne:

správa balíkov cez pip, pyproject.toml, poetry alebo pipenv,

používanie virtuálneho prostredia (venv) aby bol projekt oddelený,

dodržiavanie štandardov kvality (PEP8, pylint, black),

testovanie + CI/CD pipeline (každý commit spustí testy).

Release management:

verzovanie cez Semantic Versioning (major.minor.patch),

generovanie release notes (napr. git-changelog, release-drafter),

automatizované nasadzovanie (napr. Docker + Kubernetes).

🔑 Zhrnutie:

Agile → spôsob práce (iteratívne, flexibilne).

Git (verzovací systém) → nástroj na riadenie zdrojového kódu.

Project management → nadstavba, kde sa sleduje kto, čo a kedy robí, a ako sa verzia dostane k zákazníkovi.


## Správa verzií

co to je, na co to je dobre

## Instalacia git

## registracia na githube



`pip` je najpopulárnejší nástroj na spravovanie knižníc v Pythone. Tento nástroj sa nainštaluje automaticky spolu s Pythonom, preto ho má každý používateľ Pythonu k dispozícii.

Knižnica spravovaná pomocou `pip` sa anglicky volá *distribution package*, teda distribučný balík, ale ide o iný druh "balíka", ako sme si vysvetľovali minule keď sme preberali `import` balíkov. Preto `pip` "distribučné balíky" budeme ďalej niekedy nazývať jednoducho *knižnice*.

Distribučné balíky v `pip` majú svoje jedinečné meno a obsahujú ľubovoľný počet balíkov a modulov, ktoré po nainštalovaní budeme mať k dispozícii. Okrem mena má každá knižnica aj svoju verziu. Väčšina `pip` knižníc používa sémantické verzionovanie.

Nástroj `pip` sa používa z príkazového riadku. Ak chceme zistiť, aké `pip` distribučné balíky máme vo svojom počítači nainštalované, použijeme na to príkaz `pip list`. Program nám do konzoly vypíše všetky nainštalované knižnice a aj ktorú verziu máme nainštalovanú.

<div class="md-has-sidebar" markdown>
<main markdown>

Inštaláciu knižnice vykonáme pomocou príkazu `pip install <nazov>`. Existuje napríklad populárna knižnica na vytvorenie webového servera s názvom Flask. Do svojho počítača ju vieme nainštalovať pomocou príkazu `pip install flask`, ktorý spustíme z príkazového riadku.

 </main>
  <aside markdown>
Knižnicu odinštalujeme príkazom `pip uninstall <nazov>`.
  </aside>
</div>

!!! info "Inštalácia konkrétnej verzie knižnice"

    Niekedy je potrebné mať nainštalovanú nie najnovšiu, ale nejakú konkrétnu verziu knižnice. Verziu vieme špecifikovať pri inštalácii a to pomocou `pip install <nazov>==<verzia>`, teda napr.`pip install flask==3.0.1`

<div class="md-has-sidebar" markdown>
<main markdown>

Ak na svojom počítači už máme knižnicu nainštalovanú, ale medzitým vyšla nová verzia, je potrebné knižnicu aktualizovať. To vieme urobiť pomocou príkazu `pip install --upgrade <nazov>`.

**`pip` knižnice sa v Pythone inštalujú globálne**, teda nainštalujem ju raz a môžem ju používať vo všetkých svojich Python programoch. Toto je niekedy zdroj nepríjemných problémov, keď máme 2 python programy, ktoré potrebujú tú istú knižnicu, ale rozdielne verzie. Ešte raz, knižnice sa v Pythone inštalujú do celého systému, ich inštalácia nie je obmedzená iba pre daný Python projekt. Nemôžeme mať naraz nainštalované 2 verzie tej istej knižnice.

Na vyriešenie tohto problému sa v Pythone používa virtuálne prostredie, anglicky virtual environment.

 </main>
  <aside markdown>

!!! info "Moderné nástroje na správu knižníc"

    Modernou alternatívou k `pip` je nástroj [`uv`](https://docs.astral.sh/uv/), ktorý nahrádza `pip` a aj ďalšie iné Python nástroje. Oproti `pip` je 10-100x rýchlejší a ponpka viac funkcionalít
</aside>
</div>

## Virtuálne prostredie `.venv`

Virtuálne prostredie v Pythone poskytuje izolovaný priestor pre programátorov, v ktorom majú vlastnú sadu knižníc a vlastnú verziu jazyka Python. Knižnice nainštalované pomocou `pip` v rámci jedného virtuálneho prostredia neovplyvňujú iné virtuálne prostredia a nie sú viditeľné ani zo systému.

Virtuálne prostredie má na disku svoj vlastný adresár, do ktorého ukladá svoje nastavenia a nainštalované distribučné balíky. Väčšinou je tento adresár umiestnený v adresári daného Python projektu a jeho názov je `.venv`.

!!! info "Správa virtuálneho prostredie cez príkazový riadok"

    Ak používame príkazový riadok, tak s virtuálnym prostredím vieme pracovať nasledovne.

    - Virtuálne prostredie pred prvým použitím vytvoríme pomocou príkazu `python -m venv .venv`, ktorý vytvorí virtuálne prostredie do adresára `.venv`
    - Aktivujeme virtuálne prostredie pomocou príkazu `.venv\scripts\activate` 
    - Vo vnútri virtuálneho prostredia mám zmenený príkazový riadok, na ktorého začiatku mi bude ukazovať `(.venv)`
    - Po skončení práce s virtuálnym prostredím ho ukončím pomocou príkazu `deactivate`

Vývojové prostredie PyCharm vie spravovať a používať virtuálne prostredia a štandardne vám ho aj vytvorí pre každý váš projekt. Ak ho chcete znovu vytvoriť, stačí zmazať adresár `.venv` vo vašom projekte a potom kliknutím na menu vpravo dole si viete vytvoriť nové virtuálne prostredie.

![Vytvorenie virtuálneho prostredia v IDE PyCharm](../assets/pycharm-venv1.png){.on-glb}
/// caption
Vytvorenie virtuálneho prostredia pomocou možnosti `Add new interpreter`
///

![Vytvorenie virtuálneho prostredia v IDE PyCharm](../assets/pycharm-venv2.png){.on-glb}
/// caption
Vytvorenie nového virtuálneho prostredia z existujúceho Pythonu
///

To, či náš projekt máme spustený vo virtuálnom prostredí si vieme overiť tak, že si v Pythone otvoríme terminál, pomocou ikonky vľavo dole, alebo pomocou kláves ++"Alt"+"F12"++. Príkazový riadok v tomto termináli musí začínať značkou virtuálneho prostredia `(.venv)`

![Vytvorenie virtuálneho prostredia v IDE PyCharm](../assets/pycharm-venv3.png){.on-glb}
/// caption
Virtuálne prostredie je aktívne, príkazový riadok začína textom `(.venv)`
///

Ak v tomto termináli teraz nainštalujeme nejakú knižnicu pomocou príkazu `pip install`, tak bude viditeľná iba v rámci tohto virtuálneho prostredia. Tak isto príkaz `pip list` bude ukazovať iba knižnice nainštalované do tohto virtuálneho prostredia.

## Verejný repozitár balíkov `PyPI.org`

Všetky knižnice, ktoré si vieme stiahnuť a používať pomocou nástroja `pip` sú zverejnené vo verejnom repozitári Python Package Index (PyPI) na adrese [https://pypi.org](https://pypi.org). Pomocou tejto stránky vieme knižnice vyhľadávať a prečítať si ich dokumentáciu. Napríklad vyššie spomínaná knižnica Flask má svoju stránku na [https://pypi.org/project/Flask/](https://pypi.org/project/Flask/)

Do repozitára PyPI si vieme zadarmo uložiť aj svoje vlastné knižnice alebo celé Python programy. Musia byť licencované Open Source licenciou a nesmú byť príliš veľké. Jediné čo treba urobiť je zaregistrovať sa na ich stránke a vygenerovať si API token, pomocou ktorého budete vedieť nahrať váš distribučný balíček do repozitára.

## Správa závislostí pomocou `requirements.txt`

Táto sekcia popisuje starší (ale stále často používaný) spôsob správy projektových závislostí. Moderný spôsob si uvedieme v ďalšej sekcii.

<div class="md-has-sidebar" markdown>
<main markdown>

Keď používame nejakú knižnicu v našom Python programe, musíme ju mať nainštalovanú pomocou `pip` nástroja. Ak vytvárame projekt, ktorý má používať niekto iný, tak ten človek si po stiahnutí nášho projektu musí všetky tieto knižnice nainštalovať. Aby sme mu nemuseli hovoriť, ktorú knižnicu a v akej verzii si musí nainštalovať, tak v Pythone existuje dohoda, že názvy všetkých knižníc, ktoré náš projekt potrebuje, sa dajú do súbora `requirements.txt`. Tento súbor sa umiestni do nášho projektu a ktokoľvek bude chcieť náš projekt používať, bude mať prehľad o požiadavkách nášho projektu na cudzie knižnice.

Súbor `requirements.txt` je obyčajný textový subor, do ktorého na samostatné riadky uvedieme názvy knižníc, ktoré náš projekt vyžaduje.

 </main>
  <aside markdown>
Vygenerovať súbor so závislosťami si vieme aj automaticky pomocou príkazu **`pip freeze > requirements.txt`**. Tento príkaz nám vloží do súboru závislosti na všetky aktuálne nainštalované knižnice a uvedie aj ich konkrétnu verziu. Súbor následne môžeme ďalej manuálne upraviť a spresniť naše závislosti.
  </aside>
  </div>

=== "Ukážka súboru `requirements.txt`"

    ```
    blinker==1.9.0
    click>=8.3.0
    Flask~=3.1.2
    itsdangerous<=2.2.0
    MarkupSafe
    Jinja2>3.1.6
    ```

Ak má projekt takýto súbor, potom všetky jeho **závislosti vieme nainštalovať jedným príkazom `pip install -r requirements.txt`**

V súbore `requirements.txt` vidíme, že verzie vieme písať rôznymi spôsobmi. Ak verziu distribučného balíka neuvedieme, nainštaluje sa najnovšia verzia. Pomocou operátora `==` nainštalujeme konkrétnu verziu, a pomocou operátorov `>=`, `<=`, `>` a `<` vieme uviesť, aké verzie vyžadujeme. Operátor `~=` bude akceptovať iba kompatibilné verzie podľa sémentického verzionovania, napr. `~=2.2` bude akceptovať verzie `2.2.1`, `2.2.9`, `2.3`, ale nie `2.1` alebo `3.0`.

!!! abstract "Dokumentácia"

    Kompletný popis rôznych možností deklarovania závislostí a ich verzií môžeme nájsť v [dokumentácii špecifikácie verzií distribučných balíkov Pythonu](https://packaging.python.org/en/latest/specifications/version-specifiers/#id5)


## Konfigurácia projektu pomocou `pyproject.toml`

Súbor `requirements.txt` nám umožňuje uviesť závislosti, nič viac. Na detailnejšiu konfiguráciu Python projektov slúži súbor `pyproject.toml`. Poskytuje modernejšiu alternatívu k suboru `requirements.txt` a viem do neho dať všetky nastavenia môjho projektu.

V súbore `pyproject.toml` musím uviesť minimálne názov môjho projektu a verziu. Všetky ostatné nastavenia sú voliteľné. Závislosti na knižniciach uvádzam do atribútu `dependencies`.

=== "Príklad súboru `pyproject.toml`"

    ```toml
    [project]
    name = "moj-projekt"
    version = "0.0.1"
    dependencies = [
        "flask",
        "ascii-announcers==0.0.2"
    ]
    ```

**Ak uvediem závislosti (dependencies) v súbore `pyproject.toml`, súbor `requirements.txt` nepotrebujem**

Súbor `pyproject.toml` umiestňujeme do adresára s projektom. Všetky zdrojové moduly a balíky odporúčame dať to adresára `src`, podľa konvencie [src layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)

V PyCharm vieme potom adresár `src` označiť ako zdrojový tak, že na neho klikneme pravým tlačítkom a vyberieme `Mark Directory as -> Sources root`.

Takto vytvorený projekt potom vieme lokálne nainštalovať príkazom `pip install -e .`, ktorý spustím vo virtuálnom prostredí, napríklad pomocou terminálu v PyCharme. Tým sa nainštalujú všetky závislosti zo súboru `pyproject.toml` a naviac budeme mať lokálne k dispozícii všetky balíky a moduly z adresára `src`, takže ich budeme vedieť spúšťať pomocou príkazu `python -m balik.modul`


## Úlohy na precvičenie

!!! example "Úloha 5.1: Základný projekt"

    1. Vytvorte v PyCharm nový projekt s názvom "cowsay", tak aby mal virtuálne prostredie
    1. Vytvorte v ňom súbor `pyproject.toml` s nasledovným obsahom
    ```toml
    [project]
    name = "cowsay"
    version = "0.0.1"
    dependencies = [
        "colored",
        "ascii-announcers==0.0.2"
    ]
    ```
    1. Vytvorte nasledovné adresáre a súbory
    ```
    src
     └── cowsay
           └── __init__.py
           └── __main__.py
    ```
    1. Adresár `src` v PyCharme označte ako "Sources root"
    1. V súbore `__main__.py` napíšte jednoduchý Hello World
    ```
    print("Hello World!")
    ```
    1. V terminály lokálne nainštalujte tento projekt pomocou `pip install -e .`
    1. Projekt spustite pomocou `python -m cowsay`

!!! example "Úloha 5.2: Farebný výpis argumentov"

    Upravte projekt z úlohy 5.1 nasledovne.

    - prečítajte vstupné argumenty programu pomocou premennej `sys.argv` a vypíšte prvý argument na obrazovku. Príklad spustenia
    ```python
    python -m cowsay "Hello there!"
    ```
    - použite knižnicu `colored` na zmenu farby textu. Príklad
    ```python
    from colored import Style, fore

    color="red"
    print(fore(color) + "Hello World!" + Style.reset)
    ```

!!! example "Úloha 5.3: Zvieratko"

    Upravte projekt z úlohy 5.2 nasledovne

    - použite knižnicu `ascii-announcers` na zobrazenie ASCII zvieratka. Príklad
    ```python
    from ascii_announcers import announcers

    print(announcers.get("cow"))
    ```
    - Vypíšte farebný text z predchádzajúceho zadania a pod ním vykreslite zvieratko
    - Vyskúšajte iné zvieratká, ktoré sú uložené v premennej `announcers`

!!! example "Úloha 5.4: Orámovaný text"

    Upravte projekt z úlohy 5.3 nasledovne
    
    - Použite knižnicu `textwrap` na zalomenie textu do viacerých riadkov
    - Orámujte text, ktorý vypisujete
    - Vyskúšajte použiť farby

    Príklad výstupu:
    ```
     ________________________________ 
    / Hello there! I hope you have a \
    \      nice day today sir!       /
     -------------------------------- 
    ```

!!! example "Úloha 5.5: Cowsay"

    Upravte projekt z úlohy 5.4 nasledovne

    - Zo zvieratka nakreslite čiaru ku rámčeku, aby bolo jasné, že to hovorí zvieratko. Príklad:
    ```
     ________________________________ 
    / Hello there! I hope you have a \
    \      nice day today sir!       /
     -------------------------------- 
       \  ^__^
        \ (oo)\_______
          (__)\       )\/\
              ||----w |
              ||     ||
    ```
    - Urobte to tak, aby to fungovalo s akýmkoľvek zvieratkom



## Zhrnutie cvičenia

- [x] `pip` je nástroj na spravovanie distribučných balíkov
    * [ ] `pip` knižnice sa v Pythone inštalujú globálne
    * [ ] Nemôžeme mať naraz nainštalované 2 verzie tej istej knižnice
    * [ ] `pip list` - zoznam nainštalovaných knižníc
    * [ ] `pip install <nazov>` - nainštalovanie najnovšej verzie knižnice
    * [ ] `pip install <nazov>==<verzia>` - nainštalovanie konkrétnej verzie knižnice
    * [ ] `pip install --upgrage <nazov>` - aktualizácia knižnice
    * [ ] `pip uninstall <nazov>` - odinštalovanie knižnice
- [x] Virtuálne prostredie `.venv`
    * [ ] Izolovaný priestor pre programátorov, ktorý má vlastnú sadu knižníc a vlastnú verziu jazyka Python
    * [ ] Knižnice z jedného virtuálneho prostredia neovplyvňujú iné virtuálne prostredia a nie sú viditeľné ani zo systému.
    * [ ] Virtuálne prostredie potrebuje svoj vlastný adresár, väčšinou je to `.venv`
- [x] Verejný repozitár balíkov PyPI.org
    * Python Package Index (PyPI) na adrese https://pypi.org
    * `pip` balíky sa sťahujú práve z tohto repozitára
    * Do repozitára PyPI si vieme zadarmo uložiť aj svoje vlastné knižnice alebo celé Python programy
- [x] Správa závislostí pomocou requirements.txt
    * Starý spôsob
    * Textový súbor `requirements.txt` na samostatných riadkoch obsahuje názvy knižníc, ktoré náš projekt vyžaduje.
    * Ak má projekt takýto súbor, potom všetky jeho závislosti vieme nainštalovať jedným príkazom `pip install -r requirements.txt`
    * Okrem názvu vieme uviesť, akú verziu potrebujeme. Vieme na to použiť operátory `==`, `~=`, `>`, `>=`, `<`, `<=` a iné
- [x] Konfigurácia projektu pomocou pyproject.toml
    * Moderný spôsob
    * V súbore `pyproject.toml` musím uviesť minimálne názov môjho projektu a verziu.
    * Závislosti na knižniciach uvádzam do atribútu `dependencies`
    * Ak uvediem závislosti (dependencies) v súbore `pyproject.toml`, súbor `requirements.txt` nepotrebujem
    * Súbor pyproject.toml umiestňujeme do adresára s projektom. Všetky zdrojové moduly a balíky odporúčame dať to adresára `src`
    * Projekt viem lokálne nainštalovať príkazom `pip install -e .`
    * Projekt budem vedieť spúšťať pomocou príkazu `python -m balik.modul`



!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    PIP

    pip je nástroj na spravovanie distribučných balíkov
    pip knižnice sa v Pythone inštalujú globálne
    Nemôžeme mať naraz nainštalované 2 verzie tej istej knižnice

    pip list
    pip install
    pip install --upgrade

    VIRTUÁLNE PROSTREDIE

    Izolovaný priestor, ktorý má vlastnú sadu knižníc
    Knižnice z jedného virtuálneho prostredia neovplyvňujú iné virtuálne prostredia
    a nie sú viditeľné ani zo systému.
    Virtuálne prostredie potrebuje svoj vlastný adresár, väčšinou je to .venv

    PYPI.ORG

    Python Package Index (PyPI) na adrese https://pypi.org
    pip balíky sa sťahujú práve z tohto repozitára
    Viem v ňom zverejniť zadarmo aj svoje balíky

    requirements.txt

    Starý spôsob písania závislostí, do textového súboru
    Okrem názvu knižnice vieme uviesť verziu.
    Vieme na to použiť operátory ==, ~=, >, >=, <, <= a iné
    Ak mám súbor requirements.txt, závislosti nainštalujem pomocou pip install -r requirements.txt

    pyproject.toml

    Moderný spôsob konfigurácie projektu
    V súbore musím uviesť minimálne názov môjho projektu a verziu.
    Závislosti na knižniciach uvádzam do atribútu dependencies
    Lokálne nainštalujem príkazom pip install -e .
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je nástroj pip, ako sa používa
    - Čo je virtuálne prostredie v Pythone, na čo sa používa
    - Stránka pypi.org, na čo slúži
    - Súbor requirements.txt, na čo slúži
    - Ako sa píšu závislosti na knižniciach
    - Ako nainštalujem závislosti zo súboru requirements.txt
    - pyproject.toml, na čo slúži, aké má minimálne atribúty
    - Ako zapíšem závislosti do súboru pyproject.toml
    - Ako nainštalujem lokálne projekt, ktorý má pyproject.toml
