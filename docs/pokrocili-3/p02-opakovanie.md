# Pokročílí 2: Opakovanie

Na tomto cvičení pokračujeme v opakovaní základov Pythonu. Pripomenieme si základné dátové typy a tvorbu funkcií. Zopakujeme si tiež vetvenia, cykly a výnimky.

## Literál

Dátové typy majú svoje konkrétne hodnoty. Zápis takejto hodnoty priamo v kóde sa nazýva **literál**. Syntax literálov je súčasťou programovacieho jazyka.

<div class="md-has-sidebar" markdown>
  <main markdown>


=== "Príklady literálov v Pythone"

    ``` python
    42              # int
    -7              # negative int
    3.14            # float
    1.2e3           # float in scientific notation
    "Hello"         # string (double quotes)
    'World'         # string (single quotes)
    True            # bool
    None            # NoneType
    [1, 2, 3]       # list
    (4, 5, 6)       # tuple
    {"name": "Alice", "age": 30}  # dict
    {1, 2, 3}       # set
    {}              # empty dict
    ```
  
  
  </main>
  <aside markdown>
Reťazce znakov (strings) majú viacero typov zápisov, aby sa uľahčilo ich písanie. Ak potrebujem mať v reťazci dvojité úvodzovky, zvolím zápis s obyčajnými úvodzovkami, a naopak. `"McDonald's"`

Python podporuje taktiež aj viacriadkový reťazec, ktorý píšeme s troma úvodzovkami.
```python
"""Tento text
je na viac
riadkov"""
```
  </aside>
</div>
V Pythone každý **literál vytvorí nový objekt v pamäti počítača**. Výnimku tvoria malé nemenné objekty, ktoré Python môže, ak chce, vytvoriť v pamäti iba raz, a znovupoužiť ich. 

Na rozdiel of Javy Python nemá primitívne dátové typy, každá hodnota je objekt. Teda aj hodnoty všetkých dátových typov sú objekty. Či už máme číslo `4` alebo zoznam `[1, 2, 3]`, obidve sú v pamäti uložené ako objekty.

### None

V Pythone existuje špeciálna hodnota, ktorá signalizuje, že žiadna hodnota nie je prítomná. Zapisujeme ju `None`. Používa sa napríklad v prípadoch, keď napr. funkcia nevracia žiadnu hodnotu.

## Príkazy vs Výrazy

Kód v Pythone je postupnosť príkazov. Príkaz, anglicky *statement*, je časť kódu, väčšinou jeden riadok, ktorý vykoná nejakú akciu. Je samostatný a nemôže byť súčasťou iných príkazov. Niektoré príkazy sú zložené, čo znamená, že môžu obsahovať viacero príkazov za sebou. Príkladom je if elif else.

Príklad príkazov: `=`, `if`, `while`, `return`, `break`

Oproti tomu je výraz, anglicky *expression*, čo je časť kódu, ktorá vracia hodnotu a môže byť súčasťou iných výrazov alebo aj príkazov.

Príklad: `3 + 1`, `foo()`,` a > 5`

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Python. Vysvetli mi:

    - [rozdiel medzi príkazmi a výrazmi a uveď príklady](https://grok.com/share/c2hhcmQtMg%3D%3D_935f4e0f-5328-4e79-8a40-9a50be70c424)

## Základné dátové typy

V Pythone máme tieto základné dátové typy:


| Kategória | Názov | Popis | Príklad literálu |
| --- | --- | --- | --- |
| Čísla | `int` | celé číslo | `10`, `-5` |
| Čísla | `float` | desatinné číslo | `3.14` |
| Text | `str` | reťazec znakov | `"Ahoj Fero"`, `'Ako sa máš'` |
| Logické | `bool` | pravda / nepravda | `True`, `False` |
| Sekvencie | `list` | zoznam, meniteľný | `[1, "Fero", False]` |
| Sekvencie | `tuple` | zoznam, nemenný | `(1, "Fero", False)` |
| Množiny | `set` | množina, bez poradia, meniteľná | `{1, "Fero", False}` |
| Mapovanie | `dict` | slovník, kľúč -> hodnota, meniteľná | `{"meno": "Fero", "známka": 1}` |

<div class="md-has-sidebar" markdown>
<main markdown>

!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Python. Napíš 4 nie zložité príklady na:

    - [dátové typy int, float, bool a str](https://grok.com/share/c2hhcmQtMg%3D%3D_a3187edc-dfd0-47f4-8f5c-a300115fb98f)
    - [dátové typy list a tuple](https://grok.com/share/c2hhcmQtMg%3D%3D_0a0dbf7f-3eea-4cbd-92c1-16bf8f6036f7)
    - [dátové typy set a dict](https://grok.com/share/c2hhcmQtMg%3D%3D_23cd9ba6-d77c-4ba7-827b-f0aaa3e1c8fc)

</main>
  <aside markdown>
Na rozdiel od Javy v Pythone nemáme maximálnu hodnotu daného typu. Čísla môžu byť akokoľvek veľké, a jediným obmedzením je pamäť vášho počítača.
  </aside>
</div>

## Funkcie

Funkcia v Pythone je blok kódu, ktorému priradíme nejaké meno. Tento blok kódu môže mať nejaké vstupné hodnoty, a po skončení funkcia môže vrátiť nejaký výsledok pomocou príkazu `return`. Funkcie používame na znovupoužitie kódu, zlepšenie prehľadnosti a modularity.

Funkciu zadefinujeme slovíčkom `def`. Telo samotnej funkcie musí byť odsadené štyrmi medzerami.

=== "Príklad funkcie"

    ```python
    def sucet(a, b):
        vysledok = a + b
        return vysledok
    ```

**Nezabudnúť na dvojbodku na konci hlavičky funkcie!**

Takúto funkciu potom vieme v ďalšom kóde volať.

=== "Volanie funkcie"

    ```python
    sucet(1, 2)
    ```

Kedže Python interpretuje kód riadok po riadku, funkciu môžem zavolať až po tom, čo ju zadefinujem. To je rozdiel od napr. Javy, kde poradie nehrá úlohu.

Ak má funkcia parameter, pri jej volaní musím do týchto parametrov dať nejaké hodnoty, v takom pradí, v akom boli zadefinované. Ináč nastane chyba. V definícii funkcie mám ale možnosť niektorým parametrom priradiť *defaultnú hodnotu*, ktorá sa použije, ak pri volaní funkcie tento parameter nevyplním.

=== "Príklad funkcie s defaultnými parametrami"

    ```python
    def vypocitaj_cenu(cena, dph=0.20, zlava=0.0):
        cena_s_dph = cena * (1 + dph)
        cena_po_zlave = cena_s_dph * (1 - zlava)
        return cena_po_zlave

    print(vypocitaj_cenu(100))             # 120.0 (iba DPH 20 %)
    print(vypocitaj_cenu(100, 0.10))       # 110.0 (DPH 10 %)
    ```

Vyššie uvedené zadávanie parametrov sa vola pozičné. Okrem neho môžem v Pythone použiť aj kľúčové zadávanie parametrov. To je také, kde uvediem názov premennej. V takom prípade nemusím dodržiavať poradie. Pozičné a kľúčové zadávanie môžem kombinovať, ale kľúčové musí ísť až po pozičnom.

=== "Príklad kľúčových argumentov"

    ```python
    print(vypocitaj_cenu(zlava=0.15, cena=100)) # 102.0 (DPH 20 % a zľava 15 %)
    print(vypocitaj_cenu(100, zlava=0.15))      # 102.0 (DPH 20 % a zľava 15 %)
    ```


## Vetvenie programu

Základné vetvenie pomocou podmienok:

<div class="md-has-sidebar" markdown>
<main markdown>

=== "Podmienka v Pythone"

    ```python
    if podmienka1:
        pass # vykoná sa, ak je podmienka1 pravdivá (True)
    elif podmienka2: # voliteľná časť
        pass # vykoná sa, ak je podmienka1 nepravdivá a podmienka2 pravdivá
    else: # voliteľná časť
        pass # vykoná sa, ak žiadna podmienka nebola pravdivá
    ```

</main>
  <aside markdown>
  V Pythone je `pass` špeciálny príkaz, ktorým označujeme prázdny blok, ak v ňom nechceme nič vykonávať. Zložené príkazy ako `if` alebo `while` totiž čakajú nejaký blok kódu, a musíme tam niečo napísať.
  </aside>
</div>



=== "Použitie podmienky"

    ```python
    vek = 18

    if vek < 18:
        print("Si mladší ako 18 rokov.")
    elif vek == 18:
        print("Máš presne 18 rokov!")
    else:
        print("Si starší ako 18 rokov.")
    ```

`if` a aj iné rozhodovacie bloky môžem ľubovoľne vnárať.



<div class="md-has-sidebar" markdown>
<main markdown>

=== "Vnorená podmienka"

    ```python
    vek = 13
    ma_dospely_doprovod = True

    if vek >= 15:
        print("Môžeš ísť do kina na film od 15 rokov.")
    else:
        print("Si mladší ako 15.")
        if ma_dospely_doprovod and vek >= 12:
            print("Môžeš ísť s dospelým doprovodom.")
        else:
            print("Nemôžeš ísť na film od 15 rokov.")
    ```
</main>
  <aside markdown>

Do podmienky vieme vložiť akýkoľvek výraz. Často sa v podmienkach používajú aj logické výrazy `and`, `or` a `not`.

  </aside>
</div>


  V pythone existuje aj tzv. ternárny operátor, čo je podmienka, ktorú môžeme zapísať do jedného riadku a predstavuje výraz a nie príkaz, teda môžme je použiť v rámci iného kódu.

=== "Ternárny operátor"

    ```python
    status = "dospelý" if x >= 18 else "nedospelý"
    ```



## Cykly

Cykly slúžia na opakovanie nejakej časti kódu. Poznáme dva hlavné fory cyklov

`for` cyklus sa používa sa na iterovanie cez kolekcie (zoznamy, reťazce, množiny, atď.) alebo rozsahy čísel. Používame ho keď vieme, koľko iterácií nastane alebo keď potrebujeme iterovať cez nejakú kolekciu.

=== "Cyklus for"

    ```python
    # iteracia cez rozsah
    for i in range(5):
        print("Číslo:", i)

    # iteracia cez zoznam
    farby = ["červená", "modrá", "zelená"]
    for farba in farby:
        print(farba)

    # iteracia cez slovnik
    student = {"meno": "Ferp", "vek": 20}
    for kluc, hodnota in student.items():
        print(kluc, ":", hodnota)
    ```

`while` cyklus sa opakuje, kým platí podmienka (hodnota je `True` alebo true like). Používame ho keď dopredu nevieme, koľko krát sa cyklus má opakovať.

=== "Cyklus while"

    ```python
    x = 0
    while x < 5:
        print("x je", x)
        x += 1
    ```

Python má naviac špeciálne príkazy, ktoré menia správanie cyklov. `break` ukončí celý cyklus a `continue` preskočí aktuálnu iteráciu a pokračuje ďalšou.

=== "Riadenie cyklu"

    ```python
    for i in range(10):
        if i == 3:
            break
        print(i)

    for i in range(5):
        if i == 2:
            continue
        print(i)
    ```

Niekedy potrebujeme zistiť, či náš cyklus skončil normálne, alebo bol predčasne ukončený pomocou `break`. V Pythone na to máme špeciálnu formu použitia príkazu `else`, ktorý sa vykoná, keď cyklus skončí normálne, bez breaknutia

=== "Použitie for-break-else v príklade"

    ```python
    cisla = [2, 4, 6, 8, 10]
    hladane = 6

    for n in cisla:
        if n == hladane:
            print("Našiel som číslo:", n)
            break
    else:
        print("Číslo sa v zozname nenachádza")
    ```

## Výnimky

Výnimky v Pythone sú spôsob, ako program reaguje na chyby alebo neočakávané situácie počas behu programu. Namiesto toho, aby pri chybe program hneď spadol, môžeme výnimku zachytiť a spracovať.

```python
try:
    x = int(input("Zadaj číslo: "))
    y = int(input("Zadaj deliteľ: "))
    print("Výsledok:", x / y)
except ValueError:
    print("Zadali ste neplatné číslo!")
except ZeroDivisionError:
    print("Delenie nulou nie je povolené!")
```

Pri výnimkách sa často používa aj príkaz `finally`, ktorý sa spustí vždy po ukončení `try` bloku, či výnimka nastala alebo nie.

```python
try:
    f = open("subor.txt", "r")
    obsah = f.read()
except FileNotFoundError:
    print("Súbor sa nenašiel")
finally:
    print("Tento kód sa vykoná vždy")
```

Výnimku vieme vyvolať v kóde aj sami pomocou príkazu `raise`

```python
def faktorial(n):
    if n < 0:
        raise ValueError("Faktoriál pre záporné číslo neexistuje")
```

## Úlohy na precvičenie

!!! example "Úloha 2.1: Vykreslenie štvorca do konzoly"

    Vytvorte funkciu, ktorá vykreslí štvorec do konzoly. Vstupom do funkcie je veľkosť štvorca.

!!! example "Úloha 2.2: Výpočet obsahu a objemu valca"

    Vytvorte funkciu, ktorá na vstupe prijíma výšku a polomer a vypočíta obsah a objem valca. Výsledok vráťťe ako dict, a na vypísanie na obrazovku vytvorte samostatnú funkciu.

!!! example "Úloha 2.3: Výpočet faktoriálu"

    Vytvorte funkciu na výpočet faktoriálu

!!! example "Úloha 2.4: Orámovaný text"

    Vytvorte funkciu na vypísanie orámovaného textu do konzoly. Majte aj maximálnu šírku a text zalomte po slovách, ak je dlhý.

!!! example "Úloha 2.5: Prvočísla"

    Vytvorte funkciu na vrátenie najbližieho prvočísla väčšieho ako zadaný vstup



## Zhrnutie cvičenia

- [x] Literál je zápis konkrétneho údaja priamo v kóde
    * [ ] Každý literál vytvorí nový objekt v pamäti počítača
- [x] `None` je špeciálna hodnota, ktorá signalizuje, že žiaden údaj nie je k dispozícii
- [x] Príkaz, anglicky statement, je časť kódu, ktorý vykoná nejakú akciu.
    * [ ] Príklad: `=`, `if`, `while`, `return`, `break`
- [x] Výraz, anglicky expression, čo je časť kódu, ktorá vracia hodnotu a môže byť súčasťou iných výrazov alebo aj príkazov.
    * [ ] Príklad: `3 + 1`, `foo()`,` a > 5`
- [x] Základné dátové typy v Pythone
    * [ ] `int`, `float`, `str`, `bool`, `list`, `tuple`, `set`, `dict`
- [x] Funkcia v Pythone je blok kódu, ktorému priradíme nejaké meno. Funkciu zadefinujeme slovíčkom `def`
    * [ ] Nezabudnúť na dvojbodku na konci hlavičky funkcie!
    * [ ] V definícii funkcie môžme parametrom priradiť defaultnú hodnotu
    * [ ] Kľúčové zadávanie parametrov je také, kde uvediem názov premennej
- [x] Vetvenie programu pomocou `if`, `elif`, `else`
    * [ ] `if` a aj iné rozhodovacie bloky môžem ľubovoľne vnárať
    * [ ] ternárny operátor je podmienka, ktorú môžeme zapísať do jedného riadku a predstavuje výraz
- [x] Cykly slúžia na opakovanie nejakej časti kódu. Poznáme dva hlavné fory cyklov
    * [ ] `for` cyklus sa používa sa na iterovanie cez kolekcie alebo rozsahy čísel.
    * [ ] `while` cyklus sa opakuje, kým platí podmienka
    * [ ] `break` ukončí celý cyklus
    * [ ] `continue` preskočí aktuálnu iteráciu a pokračuje ďalšou
    * [ ] `else` sa vykoná, keď cyklus skončí normálne, bez breaknutia
- [x] Výnimky v Pythone sú spôsob, ako program reaguje na chyby
    * [ ] `try`, `except`
    * [ ] `finally` sa spustí vždy po ukončení try bloku, či výnimka nastala alebo nie.
    * [ ] výnimku vieme vyvolať v kóde aj sami pomocou príkazu `raise`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Literál je zápis údaja priamo v kóde
    Príkaz vykoná akciu, je samostatný
    Výraz vracia hodnotu a môže byť súčasťou iných výrazov a príkazov

    Základné dátové typy v Pythone:
    - int, float, str, bool, list, tuple, set, dict

    Funkciu vytvorím pomocou def
    - parametrom môžem priradiť defaultnú hodnotu
    - vieme použiť aj kľúčové zadávanie parametrov

    Vetvenie programu:
    - if, elif, else
    - ternárny operátor je if ako výraz

    Cykly:
    - for pre kolekcie a rozsahy
    - while ak máme ukončovaciu podmienku
    - break, continue, else

    Výnimky v Pythone
    - try, except
    - finally, raise
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Ústne skúšanie alebo krátka 5-minútovka:

    - Vedieť napísať funkciu
    - Vedieť ošetriť výnimku
    - Vedieť napísať cyklus a podmienku
    - Ternárny operátor
