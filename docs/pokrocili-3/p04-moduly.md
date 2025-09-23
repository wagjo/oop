# Pokročílí 4: Moduly

Ak máme väčší program, je vhodné zdrojový kód rozdeliť do viacerých súborov. Na dnešnom cvičení si vysvetlíme, akým spôsobom Python umožňuje spravovať väčšie projekty.

Na zopakovanie si pripomenieme, že doteraz sme si ukázali 2 módy spúšťania programov v Pythone

- interaktívny mód - jednoducho spustíme python príkazom `python`
- skriptovací mód - ak máme súbor `script.py` so skriptom, spustíme ho príkazom `python script.py`

## Modul

<div class="md-has-sidebar" markdown>
<main markdown>
Pri väčších projektoch už jeden súbor nestačí. V takomto prípade rozdelíme kód do viacerých modulov.

Modul napísaný v jazyku Python je súbor s príponou `.py`, ktorý obsahuje definície funkcií, tried, premenných a iný spustiteľný kód.

Ak daný súbor používame ako "hlavný súbor", pomocou ktorého spúšťame program, Python nám automaticky nastaví špeciálnu premennú `__name__` na hodnodu `__main__`. Pomocou tejto hodnoty vieme v programe rozlíšiť, či bol súbor použitý iba "ako modul" alebo bol tento súbor priamo spustený "ako program".

!!! doc "Špeciálne atribúty"

    V Pythone existujú tzv. špeciálne atribúty, ktoré nám poskytujú dodatočné informácie o module, funkcii alebo triede. Všetky špeciálne atribúty začínaju a končia dvoma podčiarkovníkmi, napríklad `__name__`. Ľudovo tieto atribúty Python programátori nazývajú aj "dunder" atribúty, čo je skratka z "double underscore".

 </main>
  <aside markdown>
Prečo sa to volá modul a nie jednoducho súbor? Moduly totižto nemusia byť iba súbory alebo napísané iba v Pythone. Niektoré moduly sú napísané aj v iných jazykoch a niektoré moduly sú interné Python moduly, vždy k dispozícii.
</div>

=== "Modul `util.py`"

    ```python
    def faktorial(n):
        if n == 0: return 1
        return n * faktorial(n - 1)

    def fib(n):
        if n == 0: return 0
        if n == 1: return 1
        return fib(n - 1) + fib(n - 2)

    def obrat(s):
        if len(s) < 2: return s
        return s[-1] + obrat(s[:-1])

    def main():
        print("V tomto module su nasledovné funkcie:")
        print(f"- Faktoriál (faktoriál čísla 30 je {faktorial(30)})")
        print(f"- Fibonacciho číslo (fibonacci na pozícii 30 je {fib(30)})")
        print(f"- Obrátenie reťazca ({obrat("Obrátenie reťazca")})")

    if __name__ == '__main__':
        main()
    ```

Modul má svoj vlastný izolovaný priestor na premenné a funkcie, takže veci zadefinované v module sú oddelené od iných modulov a navzájom sa neovplyvňujú. Ináč povedané, **každý modul má svoj vlastný priestor mien** *(namespace)* – teda slovník premenných a funkcií, ktoré sú v ňom definované.

### Používanie modulov

Moduly, ktoré si vytvorím viem použiť v iných moduloch môjho programu. Na to, aby som mohol daný modul plne využívať je potrebné ho 'importovať' pomocou príkazu `import ...`. Po úspešnom importovaní môžem napr. volať funkcie daného modulu. Importované funkcie musím pri volaní prefixovať s názvom modulu, napr. `util.faktorial(10)`

<div class="md-has-sidebar" markdown>
<main markdown>
Príkaz import nájde súbor, ktorý reprezentuje daný modul, spustí ho a vykoná v ňom všetky jeho príkazy. Nakoniec vytvorí premennú, pomocou ktorej viem pristupovať k funkciám a premenným daného modulu. Pozor však! Pri importovaní sa **modul spustí a vykoná iba pri prvom importe**. Ak ho importujem viackrát alebo vo viacerých súboroch, každý ďalší import už modul znova nespúšťa!
 </main>
  <aside markdown>
  Ak potrebujem importovaný modul znovunačítať a znovu spustiť, musím použiť špeciálnu funkciu `importlib.reload(moj_modul)` z balíka `importlib`.
</div>

=== "Modul `main.py` s klasickým importom"

    ```python
    import util

    print(f"Faktorial cisla 10 je {util.faktorial(10)}")

    postupnost = [util.fib(x) for x in range(12)]
    print(f"Fibonacciho postupnost je {postupnost}")

    print(util.obrat("ahoj"))
    ```

Druhá možnosť je importovať priamo funkcie, ktoré budem používať. To sa robí pomocou príkazu `from ... import ...`. V tomto prípade **budú importované iba vybrané funkcie, modul samotný importovaný nebude**.

=== "Modul `main.py` pomocou from ... import ..."

    ```python
    from util import faktorial, fib

    print(f"Faktorial cisla 10 je {faktorial(10)}")

    postupnost = [fib(x) for x in range(12)]
    print(f"Fibonacciho postupnost je {postupnost}")

    # print(util.obrat("ahoj"))  # toto fungovať nebude
    ```

Existuje ešte tretia možnosť, a to importovať všetky veci z daného modulo. Robí sa to pomocou príkazy `from ... import *`. Tento spôsob môže byť ale veľmi nebezpečný, preto ho neodporúčame používať.

### Vytváranie aliasov

Niekedy sa stane, že vo svojom programe už máme funkciu s rovnakým názvom, ako tú, ktorú chcem importovať. V takých prípadoch máme možnosť importovať funkciu pod iným menom, tzv. aliasom. Iné meno môžeme priradiť funkcii alebo aj celému modulu. V oboch prípadoch to robíme v importe pomocou voľby `as ...`.

=== "Alias celého modulu"

    ```python
    import util as u

    print(f"Faktorial cisla 10 je {u.faktorial(10)}")
    ```

=== "Alias konkrétnej funkcie"

    ```python
    from util import faktorial as fak

    print(f"Faktorial cisla 10 je {fak(10)}")
    ```

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Python. Vysvetli mi [ako rôzne viem importovať modul alebo jeho časť?](https://grok.com/share/c2hhcmQtMg%3D%3D_3e316290-00ab-438b-aff2-29796e37396a)


### Spúšťanie 'modulárnych' projektov

Ak mám už svoj program napísaný vo forme viacerých modulov, má sa už spúšťať ináč ako keď som mal iba jeden súbor (skript). Pri spúšťaní takého projektu mám zvyčajne jeden "hlavný" modul, ktorý obsahuje vstupný bod programu. Na spustenie tohto modulu použijem príkaz `python -m` a názov modulu, ktorý chcem spustiť, teda napr. `python -m main`. Všimnite si, že som už nenapísal názov súboru, ale názov modulu a použil som voľbu `-m`, ktorá hovorí, že spúšťam modul a nie skript.

```
# Python v interaktívnom móde
python

# Spustenie Python skriptu
python script.py

# Spustenie Python modulu
python -m modul
```

![import antigravity](../assets/antigravity-top.webp)
/// caption
V Pythone na lietanie používame `import antigravity`
///

## Balík



module.py

import module

absolute imports

relative imports

import .module
import ..module

sys.modules - cache už importovaných modulov

module.foo()

sys.path ... module shadowing

PYTHONPATH

sys.path.append("/path/to/project")

Balík (Package) v Pythone je akýkoľvek adresár, ktorý obsahuje moduly (súbory s príponou .py)

V starších verziách Pythonu bolo povinné, aby adresár, ktorý predstavuje balík, mal v sebe špeciálny súbor __init__.py, ináč ho Python nebral ako balík.



import importlib
>>> importlib.reload(mod)


__all__ = ['foo']


Zatiaľ sme si vysvetlili interaktívne programovanie pomocou konzoly a programovanie pomocou skriptu. Skript je súbor s príponou `.py` a spúšťam ho pomocou príkazu `python moj-skript.py`


## Úlohy na precvičenie

Pri nasledujúcich úlohách si zmerajte rýchlosť rekurzívnej a iteratívnej verzie pomocou knižnice `timeit`.

```python title="Meranie rýchlosti funkcie pomocou timeit"
import timeit

def faktorial(n):
    if n <= 1:
        return 1
    return n * faktorial(n - 1)

opakovania = 1000

cas = timeit.timeit(
    stmt='faktorial(100)',  # Kód, ktorý meriame
    setup='from __main__ import faktorial',
    number=opakovania
)

print(f"Čas funkcie: {cas:.6f} sekúnd ({opakovania} opakovaní)")
```

!!! example "Úloha 3.1: Fibonacciho postupnosť"

    Vytvorte program na vypis n-tého prvku Fibonacciho postupnosti pomocou rekurzívnej a iteratívnej metódy

!!! example "Úloha 3.2: Mocnina"

    Napíš rekurzívnu funkciu, ktorá vypočíta a^n^ (mocninu čísla a na n). Ako by vyzerala iteratívna verzia?

!!! example "Úloha 3.3: Hanojské veže"

    Napíš rekurzívnu funkciu, ktorá vyrieši problém Hanojských veží pre n diskov. Funkcia vypíše kroky na presun diskov z jedného kolíka (A) na druhý (C) s použitím pomocného kolíka (B).

!!! example "Úloha 3.4: Obrátenie reťazca"

    Napíš rekurzívnu funkciu, ktorá obráti zadaný reťazec (napr. "ahoj" → "joha").

!!! example "Úloha 3.5: Kontrola palindrómu"

    Rekurzívne zisti, či je reťazec palindróm (čítaný spredu aj odzadu je rovnaký).


## Zhrnutie cvičenia

- [x] Iterácia je proces opakovania určitej operácie pomocou cyklov
- [x] Rekurzia je proces, pri ktorom funkcia volá samu seba, aby vyriešila problém rozdelením na menšie podproblémy rovnakého typu
- [x] Rekurzívna funkcia má dve základné časti, prvá je základný prípad a druhá je rekurzívny prípad
    * [ ] Základný prípad je stav, kedy sa rekurzia zastaví, a predstavuje nejaký konečný stav úlohy
    * [ ] Rekurzívny prípad je stav, kedy funkcia v určitom bode zavolá samú seba
- [x] Rekurzia môže byť elegantnejšia, iterácia je väčšinou efektívnejšia
- [x] Zásobník
    * [ ] Rekurzia pri veľkom počte vnorení zaplní zásobník
    * [ ] Python má predvolený limit rekurzie (zvyčajne 1000 volaní), ktorý sa dá upraviť pomocou funkcie `sys.setrecursionlimit()`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Iterácia - používanie cyklov
    Rekurzia - volanie samého seba

    2 časti rekurzívnej funkcie
    - základný prípad - kedy sa rekurzia zastaví
    - rekurzívny prípad - keď volá samú seba

    Rekurzia môže byť elegantnejšia, iterácia je väčšinou efektívnejšia

    Rekurzia pri veľkom počte vnorení zaplní zásobník, nastane stack overflow

    Python má limit na rekurziu, mení sa pomocu sys.setrecursionlimit()
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Ústne skúšanie alebo krátka 5-minútovka:

    - Rozdiel medzi rekurziou a iteráciou
    - 2 časti rekurzívnej funkcie
    - Čo je zásobník volaní, ako súvisí s rekurziou

