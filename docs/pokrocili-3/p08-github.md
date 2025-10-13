# Pokročílí 8: Vzdialené repozitáry, GitHub

Dnešné cvičenie pokračujeme s prácou s nástrojom git a ukážeme si, ako funguje komunikácia so vzdialenými repozitármi. Taktiež si ukážeme prácu s platformou GitHub, ktorá nám umožňuje okrem iného hostovať a spravovať vzdialené git repozitáre.

## GitHub

[GitHub](https://github.com/) je platforma pre správu git repozitára a poskytuje nástropje pre agilné programovanie. Repozitár vytvorený na minulom cvičení si nahráme na GitHub.

!!! example "Úloha 8.1"

    1. Prihláste sa do stránky [GitHub](https://github.com/) a nájdite tlačidlo na vytvorenie nového repozitára (new repository). Obvykle je to tlačidlo plus (+) vpravo hore.

    1. Ako názov repozitára uveďte `opgp7`. Kliknite na `Create Repository`.

    Repozitár je prázdny a dole nám ukazuje príkazy, ako ho prepojiť s existujúcim lokálnym repozitárom. Budeme pracovať s verziou, ktorá používa `https`

## Vzdialené repozitáre

Git podporuje posielanie alebo prijímanie zmien z tzv. vzdialených repozítárov, anglicky *remote repositories*.

Pripojenie vzdieleného repozitára realizujeme pomocou príkazu `git remove`. Každému pripojenému vzdielenému repozitáru sa zvykne priradiť meno, štandardne sa takýto repozitár volá `origin`.

!!! example "Úloha 8.2"

    Spusťte IDE PyCharm, otvorte projekt `opgp7` a spustite terminál.
    
    1. V termínály pripojte vzdielený repozitár pomocou príkazu `git remote add origin https://github.com/VAS_GITHUB_USERNAME/opgp7.git`

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

Kaťdý repozitár na GitHube má svoj vlastný zoznam issues. Ide o vec mimo nástroja git, a pracuje sa s ním výlučne cez platformu GitHub. Pomocou issues vieme evidovať a manažovať bugy, požiadavky na zmeny a iné plánované alebo rozpracované veci na našom projekte.

!!! example "Úloha 8.4"

    V nástroji GitHub Issues vytvorte nasledovné issues pomocou tlačítka `New issue`:

    1. Názov: pyproject.toml, Popis: Vytvoriť súbor pyproject.toml s konfiguráciou projektu

    1. Názov: Aliasy v __init__.py, Popis: Vytvoriť aliasy na tvary v súbore tvary/__init__.py

    1. Názov: Trojuholník, Popis: Vytvoriť nový modul tvary.trojuholník a doplniť ho o metódy pre výpočet obvodu a obsahu

Ak si otvoríte tieto novo vytvorené issues, uvidíte ich popis a možete doplniť komentár alebo rôzne metadáta. V pravej časti môžete bližšie určiť typ issue, napr. či ide o bug alebo požiadavku na novú funkcionalitu, a môžete prideliť človeka, ktorý bude za daný issue zodpovedný.

!!! example "Úloha 8.5"

    Nájdite a otvorte issue `Aliasy v __init__.py`.

    1. Priraďte mu lablel **enhancement**

    1. Priraďte seba ako zodpovedného človeka k tomuto issue (Assignees - Assign yourself)

    1. Napíšte nový komentár s popisom: Vytvorím aliasy obvod_kruhu, obsah_kruhu a podobne.


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



## Pull requests


## Zhrnutie cvičenia

![Git cheatsheet](../assets/git-cheatsheet.png){.on-glb}
/// caption
Ťahák s najpoužívanejšími `git` príkazmi
///

- [x] Nástroj git - distribuovaný systém kontroly verzií
    * [ ] Konfiguráciu nástroja git robíme pomocou príkazu `git config`
    * [ ] `git config --global user.name Janko Mrkvicka`
    * [ ] `git config --global user.email janko.mrkvicka@gmail.com`
- [x] Vytvorenie git repozitára
    * [ ] Repozitár vytvoríme pomocou príkazu `git init`
    * [ ] Existujúci repozitár stiahneme pomocou príkazu `git clone <repo_url>`
    * [ ] Súbor `.gitignore` obsahuje názvy súborov a adresárov, ktoré má git nástroj ignorovať
- [x] Working tree a index/staging
    * [ ] Working tree - Aktuálne súbory na disku s ktorými pracujeme
    * [ ] Index/staging - Sada zmien, ktoré chceme zapísať do repozitára
    * [ ] Do indexu pridáme zmeny pomocou príkazu `git add`
    * [ ] Stav repozitára zistíme príkazom `git status`
- [x] Commit je zápis súboru zmien v repozitári. Obsahuje:
    * [ ] Samotné zmeny v súboroch nášho projektu
    * [ ] Krátka správa s popisom, čoho sa zmena týka
    * [ ] Dátum, kedy zápis zmien nastal
    * [ ] Autora, ktorý zápis urobil
    * [ ] Unikátny identifikátor zmeny, nazývaný hash (napríklad e3a1f5b7c2d4)
    * [ ] Odkaz na predchádzajúci commit, aby sme vedeli vyskladať postupnosť zmien
- [x] Zápis zmien do repozitára
    * [ ] Zápis sady zmien do repozitára urobíme príkazom `git commit -m "popis zmien"`
    * [ ] Zoznam posledných commitov vypíšeme pomocou príkazu `git log`
    * [ ] Prehľad zmien, ktoré neboli pridané do indexu si vieme zobraziť pomocou `git diff`
    * [ ] GUI nástroj na prehľad zmien a histórie spustíme príkazom `gitk`
- [x] Vetvy zmien
    * [ ] Novú vetvu vytvoríme pomocou príkazu `git checkout -b <branch_name>`
    * [ ] Do inej vetvy sa prepneme príkazom `git checkout <branch_name>`
    * [ ] Inú vetvu zlúčime do aktuálne otvorenej pomocou `git merge <branch_name>`
    * [ ] Zoznam všetkých vetiev zobrazíme príkazom `git branch -av`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    GIT

    Konfiguráciu robíme pomocou git config
    Repozitár vytvoríme pomocou git init
    Existujúci repo stiahneme pomocou git clone <repo_url>
    Do súboru .gitignore napíšeme, ktoré súbory má git ignorovať

    Working tree - Aktuálne súbory na disku s ktorými pracujeme
    Index/staging - Sada zmien, ktoré chceme zapísať do repozitára
    git add - Do indexu pridáme zmeny pomocou 
    git status - Stav repozitára zistíme príkazom 

    Commit je zápis súboru zmien v repozitári. Obsahuje:
    - Samotné zmeny v súboroch
    - Krátka správa, čoho sa zmena týka
    - Dátum zápisu
    - Autora zápisu
    - Unikátny identifikátor zmeny (hash)
    - Odkaz na predchádzajúci commit

    git commit -m "popis zmien" - Zápis sady zmien 
    git log - Zoznam posledných commitov
    git diff - Prehľad zmien, ktoré nie sú ešte v indexe
    gitk - GUI nástroj

    Vetvy zmien
    git checkout -b <branch_name> - vytvorenie novej vetvy
    git checkout <branch_name> - prepnutie to inej vetvy
    git merge <branch_name> - zlúčenie inej vetvy do aktuálnej
    git branch -av - Zoznam všetkých vetiev
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je working tree
    - Čo je index / staging area
    - Čo obsahuje commit
    - Základné príkazy na prácu s gitom
