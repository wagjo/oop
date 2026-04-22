# Cvičenie 18: MVC Architektúra

Na dnešnom cvičení si ukážeme použitie MVC architektúry na reálnom príklade. 

## Model-View-Controller

MVC je skratka pre Model-View-Controller architektúru, čo je populárny prístup k rozdeleniu zdrojového kódu pre GUI aplikácie.

Základná myšlienka MVC

- Model: dáta, biznis logika
- View: UI (FXML, komponenty)
- Controller: reaguje na udalosti, prepája View a Model

MVC má význam pre stredne veľké a veľké projekty, a pre projekty, na ktorých pracuje viacero programátorov.

Výhody MVC:

- Jednoduchšia údržba
- Znovupoužiteľnosť kódu
- Lepšie testovanie
- Škálovateľnosť
- Flexibilita UI

## Predpoveď počasia

Dnes budeme pracovať s kódom aplikácie na zobrazenie predpovede počasia.

Na získanie predpovede počasia budeme používať voľne dostupné služby stránky [https://open-meteo.com/](https://open-meteo.com/)

Stránka open-meteo nám umožňuje získať informácie o predpovedi počasia pomocou webových API.

My budeme používať nasledujúce 2 API služby:

- Služba na získanie polohy miesta
- Služba na získanie predpovede počasia pre danú polohu

### Získanie polohy

Služba na získanie polohy (zemepisnej šírky a dĺžky) miesta podľa jeho názvu, (napr. Prešov, Košice, Kysak) je dostupná na stránke [https://open-meteo.com/en/docs/geocoding-api](https://open-meteo.com/en/docs/geocoding-api)

My potrebujeme zistiť zemepisnú šírku a dĺžku nášho miesta.

!!! info "API Request"

    Príklad API requestu pre získanie polohy mesta Prešov: [https://geocoding-api.open-meteo.com/v1/search?name=Presov](https://geocoding-api.open-meteo.com/v1/search?name=Presov)

V našej aplikácii zoberieme miesto prvého výsledku, ktoré nám toto API vráti. Taktiež z výsledku vyhľadávania získame plné meno miesta a názov krajiny, v ktorej leží.

### Predpoveď počasia

Služba predpovede počasia je dostupná na stránke [https://open-meteo.com/en/docs](https://open-meteo.com/en/docs). Pre získanie predpovede potrebujeme uviesť presnú polohu na zemeguli.

Služba nám poskytuje veľké množstvo informácií, my budeme mať záujem o tieto:

- aktuálne počasie
- týždenná predpoveď
- denná predpoveď
- informácie o teplote, vlhkosti a celkových podmienkach (zamračené, jasno, ...)

!!! info "API Request"

    Príklad API requestu pre získanie predpovede pre Prešov: [https://api.open-meteo.com/v1/forecast?latitude=49.00&longitude=21.23&current=temperature_2m,apparent_temperature,weather_code&hourly=temperature_2m,relative_humidity_2m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min&timezone=Europe/Bratislava&forecast_days=7](https://api.open-meteo.com/v1/forecast?latitude=49.00&longitude=21.23&current=temperature_2m,apparent_temperature,weather_code&hourly=temperature_2m,relative_humidity_2m,weather_code&daily=weather_code,temperature_2m_max,temperature_2m_min&timezone=Europe/Bratislava&forecast_days=7)


## Úlohy

!!! example "Úloha 18.1: Dáta zo služby open-meteo.com"

    Prezrite si výsledné dáta, ktoré vám vrátia API so služby open-meteo uvedené vyššie.

    Oboznámte sa so štruktútou JSON dáta, ktoré vám tieto API vrátia

!!! example "Úloha 18.2: Predpoveď počasia"

    Stiahnite si repozitár [https://github.com/wagjo/opg-gui-pocasie.git](https://github.com/wagjo/opg-gui-pocasie.git) a otvorte ho vo vašom IntelliJ IDEA.

    Oboznámte sa so zdrojovým kódom aplikácie

    Aplikácia používa architektúru MVC.

    Ktoré časti zdrojového kódu tvoria Model, ktoré View a ktoré Controller?


!!! example "Úloha 18.3: Úprava týždennej predpovede počasia"

    Karta s dennou predpoveďou počasia používa TableView a jeho property sú prepojené s property objektami v modeli aplikácie.

    - Nájdite v projekte kód, ktorý má na starosti nastavenie dennej predpovede počasia.

    Karta s týždennou predpoveďou však používa bežné layout komponenty a ovládacie prvky. Tieto sa musia vytvoriť nanovo pri každom aktualizovaní počasia a každý jeden komponent sa musí prepojiť s property objektami v module. 
    To má za následok komplikované prepojenia v rámci MVC a zneprehľadnuje kód.

    2. Nájdite v projekte kód, ktorý má na starosti nastavenie týždennej predpovede počasia.

    3. Upravte kód tak, aby aj týždenná predpoveď používala TableView a mala čistejšie prepojenie v rámci MVC architektúry.