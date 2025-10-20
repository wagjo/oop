# Cvičenie 7: Triedy a metódy

Dnešné cvičenie je zamerané na opakovanie a upevnenie znalostí o tvorbe tried a metód v Jave.


## Vytváranie tried

- základná štruktúra triedy
- názov súboru je rovnaký ako názov triedy
- názov balíka je rovnaký ako adresárova cesta

## Vytváranie metód

- vstupné argumenty
- typ návratovej hodnoty
- CTRL-ALT-L formatovanie
- medzery medzi metódami


## Úlohy na precvičenie

!!! example "Úloha 7.9: Hrajte znova"

    Po skončení hry nevypnite program, ale spusťte hru znova. Pamätajte si počet výhier a prehier.

!!! example "Úloha 7.10: Defaultný počet pokusov"

    Vytvorte konštantu `DEFAULT_POKUSY` v triede `Stav`. Vytvorte preťažený konštruktor, ktorý bude prijímať iba hľadané slovo a použite tento defaultný počet pokusov. Odstráňte magické číslo z triedy `Hra`

!!! example "Úloha 7.11: Defaultný zoznam slov"

    Vytvorte konštantu pole slov `DEFAULT_SLOVA` v triede `Stav`. Dajte do neho 20 slov. Vytvorte preťažený konštruktor, ktorý nemá žiaden argument a použije náhodné slovo z tohto zoznamu.

!!! example "Úloha 7.12: Ošetrenie vstupov"

    Ošetrite vstupy do konštruktora, vyhoďte výnimky, ak sú nesprávne. Podobne ošetrite vstupy do metód a vstupy z klávesnice.

!!! example "Úloha 7.13: Počet pokusov"

    Pri výhre vypíšte, na ktorý pokus hráč vyhral

!!! example "Úloha 7.14: Pamäť nesprávnych znakov"

    Pamätajte si nesprávne znaky, ktoré hráč hádal. Pri vypísani stavu hry ich v každom kole vypíšte.

!!! example "Úloha 7.15: Nepozornosť sa nevypláca"

    Odoberte pokus, aj keď hráč zadá znak, ktorý je síce správny, ale už ho pred tým zadal.


## Zhrnutie cvičenia

- [x] Vyriešte úlohy a naprogramujte textovú hru Obesenec
- [x] Getter metódy, ktoré vracajú boolean hodnotu sa zvyknú začínať slovom `is`, ostatné sa začínajú slovom `get`.
- [x] V UML Class diagrame sú statické metódy podčiarknuté

!!! note "Poznámky do zošita"
    Toto cvičenie si do zošita nemusíte písať žiadne poznámky

!!! warning "Skúšanie a kontrola vedomostí"

    Na budúcom cvičení začíname tvoriť väčší projekt, preto ak máte ešte s tvorbou tried a metód problémy a nejasnosti, vypracujte si doma úlohy z tohto a minulých cvičení.

    Okruhy otázok na test:

    - Vedieť vytvoriť a balíky, triedy a metódy
    