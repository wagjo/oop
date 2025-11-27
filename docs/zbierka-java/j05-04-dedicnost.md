# Dedičnosť


## Úloha 5.4.1: Publikácie

1. Vytvorte triedy a atribúty:

    - `Publikacia` - `String nazov`, `int rokVydania`
    - `Kniha` - `String[] autori`
    - `DigitalnaKniha` - `String format` (napr. ebook, audio)
    - `FyzickaKniha` - `String vazba` (napr. tvrdá alebo mäkká)

1. Zadefinujte vhodnú dedičnosť tried

1. Vytvorte `toString()` metódy

1. Vytvorte konštruktory, ktoré budu vhodne volať rodičovské konštruktory a inicializovať všetky atribúty

1. Uveďte príklad použitia


## Úloha 5.4.2: Zvieratá

1. Vytvorte triedy a atribúty:

    - `Zviera` - `String meno`, `int vek`
    - `DomaceZviera`
    - `DiveZviera` - `String miestoOdchytu`
    - `Vlk`
    - `Krava`

1. Zadefinujte vhodnú dedičnosť tried.

1. Vytvorte triedu `Zverinec` a v nej

    - atribút `Zviera[] zvierata`
    - statickú metódu `isDomaceZviera(Zviera zviera)`
    - metódu `pocetDivych()` a `pocetDomacich()`
    - metódu `vypisPovody()`, ktorá vypíše miesta odchytu divých zvierat

1. Vytvorte konštruktory, ktoré budu vhodne volať rodičovské konštruktory a inicializovať všetky atribúty.

1. Uveďte príklad použitia


## Úloha 5.4.3: IT Firma

1. Vytvorte triedy a atribúty:

    - `Zamestnanec` - `String meno`, `int rokPrijatia`, `int mzda`
    - `Programator` - `int projekty`
    - `Manazer` - `int velkostTimu`
    - `SeniorProgramator`
    - `JuniorProgramator`

1. Zadefinujte vhodnú dedičnosť tried.

1. V triede `Zamestnanec` vytvorte metódu `vypocitajMzdu()`, ktorá vráti mzdu zamestnanca navýšenú o bonus. Využite prekrytie metód na výpočet bonusu podla týchto pravidiel:

    - Programátori majú bonus rovný mzda * 0.2 * počet projektov
    - Manažeri majú bonus rovný mzda * 0.05 * počet členov ich tímu
    - Ostatní zamestnanci bonus nemajú

1. Vytvorte konštruktory, ktoré budu vhodne volať rodičovské konštruktory a inicializovať všetky atribúty.

1. Uveďte príklad použitia


## Úloha 5.4.4: Banka

1. Vytvorte triedy a atribúty:

    - `BankovyProdukt` - `String nazov`
    - `Ucet` - `double zostatok`
    - `SporiaciUcet` - `int vypovednaLehota`
    - `Uver` - `double vyska`, `double urokovaSadzba`
    - `HypotekarnyUver` - `double hodnotaZabezpeky`

1. Vytvorte `toString()` metódy

1. Zadefinujte vhodnú dedičnosť tried

1. Vytvorte konštruktory, ktoré budu vhodne volať rodičovské konštruktory a inicializovať všetky atribúty.

1. Uveďte príklad použitia


## Úloha 5.4.5: Autobazár

1. Vytvorte triedy a atribúty:

    - `Vozidlo` - `String vyrobca`, `String model`, `int rocnik`
    - `Motorka` - `double zostatok`
    - `Auto` - `int farbaKaroserie`
    - `NakladneAuto` - `int pocetNaprav`
    - `SUV`

1. Vytvorte metódu `pocetKolies()` pre každý typ vozidla. V triede `Vozidlo` táto metóda nech vyhodí výnimku, v triede `Motorka` nech vráti 2 a v triede `Auto` 4. V triede `NakladneAuto` nech počet kolies bude rovný 2 * pocetNaprav

1. Vytvorte metódu `pocetNaprav()` pre triedu `Auto` a prekyte ju v triede `NakladneAuto` tak, aby vrátilo počet náprav z atribútu. V ostatných prípadoch vráťte číslo 2.

1. Zadefinujte vhodnú dedičnosť tried

1. Vytvorte konštruktory, ktoré budu vhodne volať rodičovské konštruktory a inicializovať všetky atribúty.

1. Uveďte príklad použitia
