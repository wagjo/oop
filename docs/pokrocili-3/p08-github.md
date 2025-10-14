# Pokročílí 8: Vzdialené repozitáry, GitHub

Dnešné cvičenie pokračujeme s prácou s nástrojom git a ukážeme si, ako funguje komunikácia so vzdialenými repozitármi. Taktiež si ukážeme prácu s platformou GitHub, ktorá nám umožňuje okrem iného hostovať a spravovať vzdialené git repozitáre.

## GitHub

[GitHub](https://github.com/) je platforma pre správu git repozitára a poskytuje nástroje pre agilné programovanie. Repozitár vytvorený na minulom cvičení si nahráme na GitHub.

!!! example "Úloha 8.1"

    1. Prihláste sa do stránky [GitHub](https://github.com/) a nájdite tlačidlo na vytvorenie nového repozitára (new repository). Obvykle je to tlačidlo plus (+) vpravo hore.

    1. Ako názov repozitára uveďte `opgp7`. Kliknite na `Create Repository`.

    Repozitár je prázdny a dole nám ukazuje príkazy, ako ho prepojiť s existujúcim lokálnym repozitárom. Budeme pracovať s verziou, ktorá používa `https`

## Vzdialené repozitáre

Git podporuje posielanie alebo prijímanie zmien z tzv. vzdialených repozítárov, anglicky *remote repositories*.

Pripojenie vzdieleného repozitára realizujeme pomocou príkazu `git remote`. Každému pripojenému vzdielenému repozitáru sa zvykne priradiť meno, štandardne sa takýto repozitár volá `origin`.

!!! example "Úloha 8.2"

    Spusťte IDE PyCharm, otvorte projekt `opgp7` a spustite terminál.
    
    1. V termínáli pripojte vzdielený repozitár pomocou príkazu `git remote add origin https://github.com/VAS_GITHUB_USERNAME/opgp7.git`

    1. Uploadnite celý váš repozitár pomocou `git push -u origin master`

    Refreshnite webovú stránku s vašim repozitárom a mali by ste tam vidieť vaše súbory.

## Zasielanie zmien na vzdialený repozitár

Teraz si vyskúšame zasielanie zmien na vzdialený repozitár. Urobíme novú zmenu, commitneme a pošleme na GitHub. Ak máme správne nastavený vzdialený repozitár, zasielanie a prijímanie zmien robíme pomocou príkazov

- `git push` Uploadne všetky lokálne commity na vzdialené repozitár
- `git pull` Stiahne všetky nové commity so vzdialeného repozitára a urobí merge do lokálnej vetvy

!!! example "Úloha 8.3"

    1. V nástroji PyCharm vytvorte nový súbor `README.md`. Napíšte do neho nasledovný text

        ```markdown
        # Skúšobný projekt OPGP

        Projekt spustíte príkazom `python -m main`

        ```

    1. Pridajte súbor do indexu a vytvorte commit so správou "readme"

    1. Odošlite nový commit do vzdialeného repozitára pomocou príkazu `git push`, alebo v PyCharme pomocou voľby `Push...` v ľavom hornom git menu

    1. Refreshnite stránku s projektom na GitHube a uvidíte nový popis projektu


## GitHub Issues

Každý repozitár na GitHube má svoj vlastný zoznam issues. Ide o vec mimo nástroja git, a pracuje sa s ním výlučne cez platformu GitHub. Pomocou issues vieme evidovať a manažovať bugy, požiadavky na zmeny a iné plánované alebo rozpracované veci na našom projekte.

!!! example "Úloha 8.4"

    V nástroji GitHub Issues vytvorte nasledovné issues pomocou tlačítka `New issue`:

    1. Názov: `pyproject.toml`, Popis: `Vytvoriť súbor pyproject.toml s konfiguráciou projektu`

    1. Názov: `Aliasy v __init__.py`, Popis: `Vytvoriť aliasy na tvary v súbore tvary/__init__.py`

    1. Názov: `Trojuholník`, Popis: `Vytvoriť nový modul tvary.trojuholník a doplniť ho o metódy pre výpočet obvodu a obsahu`

Ak si otvoríte tieto novo vytvorené issues, uvidíte ich popis a možete doplniť komentár alebo rôzne metadáta. V pravej časti môžete bližšie určiť typ issue, napr. či ide o bug alebo požiadavku na novú funkcionalitu, a môžete prideliť človeka, ktorý bude za daný issue zodpovedný.

!!! example "Úloha 8.5"

    Nájdite a otvorte issue `Aliasy v __init__.py`.

    1. Priraďte mu lablel **enhancement**

    1. Priraďte seba ako zodpovedného človeka k tomuto issue (Assignees - Assign yourself)

    1. Napíšte nový komentár s popisom: `Vytvorím aliasy obvod_kruhu, obsah_kruhu a podobne.`


## GitHub Projects

Okrem issues nám nástroj GitHub ponúka aj iné nástroje na podporu agilného vývoja. Vyskúšame si urobiť Kanban.

!!! example "Úloha 8.6"

    1. Na stránke vášho repozitára kliknite na `Projects` a vytvorte nový projekt s tlačítkom `New Project`

    1. Vyberte možnosť `Kanban` a názov projektu dajte tiež `Kanban`

    1. Otvorí sa vám okno s prázdnymi stĺpcami. Kliknite na tlačidlo `Add item` na dolnej časti stránky

    1. Kliknite na ikonku plus (+) a vyberte možnosť `Add item from repository`.

    1. Nájdite váš repozitár a pridajte všetky 3 issues

    1. Issue Trojuholnik dajte do stĺpca Backlog, issue pyproject.toml do stĺpca Ready a issue Aliasy dajte do "In progress"


## Viacero vetiev na vzdialenom repozitári

Vyskúšame si urobiť issue `Aliasy v __init__.py` v samostatnej vetve. Normálne takéto jednoduché úlohy robíme priamo na hlavnej vetve, samostatné vetvy používame, ak sú zmeny rozsiahlejšie alebo implementácia náročnejšia.

!!! example "Úloha 8.7"

    1. Otvorte issue `Aliasy v __init__.py` a zistite číslo tohto issue, napr. `#2`

    1. V termináli vytvorte nový branch s názvom `issue-2`, kde číslo bude číslo issue, pomocou príkazu `git checkout -b issue-2`

    1. Do súboru `tvary/__init__.py` doplňte aliasy nasledovným kódom

        ```python
        from .kruh import obvod as obvod_kruhu, obsah as obsah_kruhu
        from .obdlznik import obvod as obvod_obdlznika, obsah as obsah_obdlznika
        from .stvorec import obvod as obvod_stvorca, obsah as obsah_stvorca
        ```
    
    1. Vytvorte nový commit, s popisom `[#2] Vytvorenie aliasov v __init__.py`

    1. V termináli odošlite vetvu na GitHub. Keďže ide o novú vetvu, obyčajný `git push` nestačí, musíme to prvý krát nakonfigurovať. Správny príkaz je `git push -u origin issue-2`

Keď si teraz otvoríme issue v prehiadači, uvidíme, že v popise bude odkaz na náš novo vytvorený commit. GitHub tento issue prepojil, pretože sme v popise commitu spomenuli číslo issue pomocou znaku mriežky (#2). V Githube takto pomocou mriežky vieme vytvárať odkaz na nejaký issue.

## Pull requests

Na GitHube teraz máme 2 vetvy, vetvu `master` a vetvu `issue-2`. Tieto vetvy vieme lokálne zlúčiť pomocou `git merge`. Pri práci v tíme je v prípadoch, že pracujeme na samostatnej vetve často lepšie, ak **zmeny, ktoré chceme zlúčiť si necháme schváliť niekym iným**.

V GitHube na takéto schválenie máme Pull requests. Vyskúšame si teraz z vetvy `issue-2` urobiť takýto pull equest

!!! example "Úloha 8.8"

    1. Na vašom repozitári na GihHube nájdite stránku "Pull requests" a kliknite na "New pull request"

    1. Máte na výber určiť vetvy **base** a **compare**. Base vetvu nechajte `master` a ako compare vetvu vyberte `issue-2`

    1. Kliknite na "Create pull request". Do popisu requestu napíšte `Tento pull request implementuje issue #2`

    1. Stlačte "Create pull request". GitHub ho prepojí aj s issue #2. Samotný pull request má svoje vlastné číslo, napr. #4

    1. Teraz by mala nastať chvíľa, kedy si iný člen tímu prezrie tento pull request a keď je v poriadku tak ho schváli. My si ho schválime sami. V Pull requeste stlačte tlačítko "Merge pull request" a potom na tlačítko "Confirm merge". Pull request bude mať teraz modrú značku "merged"

    1. GitHub nám po schválení pull requestu zlúčil vetvu `issue-2` do vetvy `master`. Teraz si ju lokálne stiahneme. V termináli PyCharmu sa prepnite do vetvy master pomocou `git checkout master` a stiahnite zmeny príkazom `git pull`.

## Zhrnutie cvičenia

![git architecure](../assets/git-flow.png){.on-glb}
/// caption
Štruktúra `git` repozitára a základné príkazy
///

- [x] Vzdialený repozitár
    * [ ] Pripojíme pomocou `git remote add <remote_nick> <remote_url>`
- [x] Odoslanie zmien 
    * [ ] Prvý krát pomocou `git push -u <remote_nick> <branch_name>`
    * [ ] Následné posielania už iba pomocou `git push`
- [x] Prijímanie zmien
    * [ ] Pomocou `git pull`
- [x] GitHub Issues
    * [ ] Slúžia na správu bugov, úloh a pod.
    * [ ] Každý issue má svoje číslo, v komentároch na neho odkazujeme pomocou mriežky, napr. #2
- [x] GitHub projects 
    * [ ] Umožňuje vytvoriť tabuľku s rôznymi stavmi issues, napr. štýlu Kanban
- [x] Pull requests
    * [ ] Nástroj na schvaľovanie zmien, ktoré sú v samostatných vetvách
    * [ ] Pull request vytvoríme vybraním vetvy, ktorá obsahuje zmeny, ktoré chceme dať schváliť
    * [ ] Človek, zodpovedný za schválenie vie pull request zlúčiť do hlavnej vetvy, alebo vrátiť na prepracovanie


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    VZDIALENÝ REPOZITÁR

    Pripojíme pomocou git remote add <remote_nick> <remote_url>

    Odoslanie zmien 
    Prvý krát pomocou git push -u <remote_nick> <branch_name>
    Následné posielania pomocou git push
 
    Prijímanie zmien - git pull

    GitHub Issues - správa bugov, úloh a pod.
    Každý issue má svoje číslo, odkazujeme na neho pomocou mriežky, napr. #2

    GitHub projects - tabuľky s rôznymi stavmi issues, napr. štýlu Kanban
    
    Pull requests - schvaľovanie zmien, ktoré sú v samostatných vetvách
    Po kontrole vieme pull request zlúčiť do hlavnej vetvy, alebo vrátiť na prepracovanie
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Ako sa pripojí vzdialený repozitár
    - Posielanie commitov do vzdialeného repozitára
    - Prijímanie zmien so vzdialeného repozitára
    - Na čo slúži GitHub issues
    - Na čo slúži GitHub projects
    - Na čo slúži GitHub pull requests
