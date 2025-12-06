# Cvičenie 10: Dedičnosť

V dnešných úlohách si precvičíme dedičnosť a rozhrania. Dnešné úlohy nie sú definované presne a dopodrobna. Je na vás, aby ste detaily týchto úloh navrhli tak, aby splnili účel tohto zadania a neprinášali do riešenia zbytočnú zložitosť navyše.

K navrhnutým triedam si vytvoríme aj diagram tried v nástroji Mermaid, podľa pravidiel, ktoré sme si vysvetľovali na predchádzajúcich hodinách teórie.

## Správa tovaru v záložni

Zadanie úlohy:

- Navrhnite systém pre správu tovaru v záložni
- Pre vlastnosti jednotlivých tovarov navrhnite vhodné rozhrania
- Kategórie tovarov reprezentujte formov tried
- Vytvorte triedy pre sklad a pre chod záložne

!!! example "Úloha 10.1: Základná trieda tovaru"

    Napíšte triedu `Tovar` s nasledovnými atribútmi:

    - názov
    - dátum príjmu
    - stav
    - trhova cena
    - zalozna cena
    - doba splatnosti

    Pre atribúty vytvorte getter metódy a vytvorte konštruktor.

    Vytvorte metódu main pre otestovanie funkčnosti.

!!! example "Úloha 10.2: Rozhrania kategórii tovarov"

    Vytvorte rozhrania pre nasledovné kategórie tovarov:

    - `MaDrahyKov` s metódami `getRydzost`, `getMaterial`, `getVaha`
    - `MaZnackuModel` s metódami `getZnacka`, `getModel`
    - `Starozitnost`
    - `VekoveObmedzenie`

!!! example "Úloha 10.3: Triedy tovarov"

    Vytvorte triedy pre nasledovné tovary:

    - `Sperk` - implementuje `MaDrahyKov`
    - `StarozitnySperk` - dedi z driedy `Sperk`, implementuje `Starozitnost`
    - `Minca` - implementuje `MaDrahyKov`
    - `Elektronika` - implementuje `MaZnackuModel`
    - `MobilnyTelefon` - dedi z triedy `Elektronika`
    - `Zbrane` - implementuje `MaZnackuModel` a `VekoveObmedzenie`

    Pre každú triedu naimplementujte aj `toString`.

!!! example "Úloha 10.4: Sklad tovaru"

    Vytvorte triedu `Sklad` s nasledovnou funkcionalitou:

    - pridanie tovaru
    - odobranie tovaru
    - vratenie zoznamu vsetkych veci v sklade
    - celkovaHodnota

    Vytvorte metódu main pre otestovanie funkčnosti.

!!! example "Úloha 10.5: Záložňa"

    Vytvorte triedu `Zalozna` s konštruktorom a atribútmi:
    
    - sklad
    - pokladňa (double)

    Vytvorte metódy:

    - odkup tovaru (zmenší pokladňu a zvýši sklad)
    - predaj tovaru
    - splatenie tovaru - ako predaj, ale použije zaloznu cenu plus úrok

    Vytvorte príklad použitia vyššie uvedených tried. Na sklade aby ste mali aspoň 10 tovarov.

!!! example "Úloha 10.6: Diagram tried"

    Vytvorte diagram tried napr. v nástroji Mermaid pre triedy z predchádzajúcich úloh. V diagrame vhodne znázornite vzťahy medzi jednotlivými triedami.

!!! example "Úloha 10.7: Textové ovládanie"

    Vytvorte triedu `TextoveUI`, ktoré bude poskytovať textové ovládanie pre prácu so 
    záložňou, teda umožní predaj, odkup a splatenie

    Vytvorte hlavný vstup do programu.

!!! example "Úloha 10.8: Trieda Historia"

    Vytvorte triedu `Historia`, ktorá bude uchovávať históriu udalostí v záložni. Bude mať nasledovné metódy:

    - `novyZaznam` - zapíše nový záznam
    - `lastZaznamy` - vráti n posledných záznamov

    Upravte triedu `Sklad` a `Zalozna` tak, aby sa pri zmenéch zapísala udalosť do histórie.

    Upravte triedu `TextoveUI` tak, aby v nej bola možnosť vypísať posledné záznamy z histórie.

!!! example "Úloha 10.9: Analytika"

    Vytvorte triedu `Analytika` s nasledovnými metódami:

    - `predatelnyTovar` - zoznam tovarov, ktorá sú po dobe splatnosti
    - `najdrahsiTovar`
    - `najstarsiTovar`
    - `priemernaCena`
    - `poslednePredane`

    Metódy naimplementujte a napíšte príklady použitia týchto metód.
    