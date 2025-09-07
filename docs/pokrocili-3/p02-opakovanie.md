# Pokročílí 2: Opakovanie

## Literál

Dátové typy majú svoje konkrétne hodnoty. Zápis takejto hodnoty priamo v kóde sa nazýva **literál**. Syntax literálov je súčasťou programovacieho jazyka.

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

V Pythone každý **literál vytvorí nový objekt v pamäti počítača**. Výnimku tvoria malé nemenné objekty, ktoré Python môže, ak chce, vytvoriť v pamäti iba raz, a znovupoužiť ich. 

Na rozdiel of Javy Python nemá primitívne dátové typy, každá hodnota je objekt. Teda aj hodnoty všetkých dátových typov sú objekty. Či už máme číslo `4` alebo zoznam `[1, 2, 3]`, obidve sú v pamäti uložené ako objekty.

### None

V Pythone existuje špeciálna hodnota, ktorá signalizuje, že žiadna hodnota nie je prítomná. Zapisujeme ju `None`. Používa sa napríklad v prípadoch, keď napr. funkcia nevracia žiadnu hodnotu.

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


!!! tip "Učím sa s pomocou umelej inteligencie"

    Som študent strednej školy, učím sa Python. Napíš 4 nie zložité príklady na:

    - [dátové typy int, float, bool a str](https://grok.com/share/c2hhcmQtMg%3D%3D_a3187edc-dfd0-47f4-8f5c-a300115fb98f)
    - [dátové typy list a tuple](https://grok.com/share/c2hhcmQtMg%3D%3D_0a0dbf7f-3eea-4cbd-92c1-16bf8f6036f7)
    - [dátové typy set a dict](https://grok.com/share/c2hhcmQtMg%3D%3D_23cd9ba6-d77c-4ba7-827b-f0aaa3e1c8fc)

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

```python
if podmienka1:
    pass # vykoná sa, ak je podmienka1 pravdivá (True)
elif podmienka2: # voliteľná časť
    pass # vykoná sa, ak je podmienka1 nepravdivá a podmienka2 pravdivá
else: # voliteľná časť
    pass # vykoná sa, ak žiadna podmienka nebola pravdivá
```

pass je klucove slovo, ktorým označujeme prázdny blok, ak v ňom nechceme nič vykonávať

```python
vek = 18

if vek < 18:
    print("Si mladší ako 18 rokov.")
elif vek == 18:
    print("Máš presne 18 rokov!")
else:
    print("Si starší ako 18 rokov.")
```

If a aj iné rozhodovacie bloky môžem ľubovoľne vnárať.

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

## Cykly

```python
for i in range(5):
    print("Číslo:", i)
```

```python
x = 0
while x < 5:
    print("x je", x)
    x += 1
```

```python
for i in range(10):
    if i == 3:
        break
    print(i)
```

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

## výnimky

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

finally

## Príkazy vs Výrazy

Kód v Pythone je postupnosť príkazov. Príkaz, anglicky statement, je časť kódu, väčšinou jeden riadok, ktorý vykoná nejakú akciu. Je samostatný a nemôže byť súčasťou iných príkazov. Niektoré príkazy sú zložené, čo znamená, že môžu obsahovať viacero príkazov za sebou. Príkladom je if elif else.

Príklad: =, if, while, return, break

Oproti tomu je výraz, anglicky expression, čo je časť kódu, ktorá vracia hodnotu a môže byť súčasťou iných výrazov alebo aj príkazov.

Príklad: 3 + 1, foo(), a > 5


