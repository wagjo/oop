# Pokročílí 10: Metódy a JSON

Dnes pokračujeme v objektovo orientovanom programovaní a vysvetlíme si aj prácu s dátovým formátom JSON.

## Súkromné atribúty

Python nemá modifikátory prístupu ako ich má napr. Java. Ak chceme deklarovať nejaký atribút alebo metódu ako súkromnú, robíme tak pomocou dohody a konvencie.

**Názov začínajúci s jedným podčiarníkom** hovorí, že daný atribút alebo metóda je interná a nemá byť používaná mimo triedy. V Jave by sme pre takýto atribút použili modifikátor prístupu `protected`.

```python
class Osoba:
    def __init__(self, meno, vek):
        self._vek = vek       # interný podľa dohody
        self.meno = meno

    def vek(self):
        return self._vek

o = Osoba("Eva", 25)
print(o._vek)  # Dá sa k nemu dostať, ale nemalo by sa
```

Okrem toho v Pythone vieme použiť aj **názvy začínajúce s dvoma podčiarníkmi**, ktoré sa používajú pre súkromné atribúty a metódy. V Jave by sme pre nich použili modifikátor prístupu `private`.
Pri týchto názvoch Python vykoná špeciálne premenovanie (anglicky name mangling), aby sme sa k nim ľahko nedostali a aby sa predišlo kolíziám v názvoch (shadowing) v podtriedach (o tom viac nabudúce)

```python
class Osoba:
    def __init__(self, meno, vek):
        self.__vek = vek    # súkromný s name manglingom
        self.meno = meno

    def zobraz(self):
        print(self.__vek)

o = Osoba("Eva", 25)
o.zobraz()         # funguje
# print(o.__vek)   # AttributeError
``` 

## Vymazanie atribútov a metód

V Pythone sa dajú vymazať atribúty a dokonca aj metódy. Či už ide o inštančné, triedne alebo statické metódy, v Pythone je ich možné vymazať pomocou príkazu `del`.

```python
class Osoba:
    druh = "Clovek"

    def __init__(self, meno, vek):
        self.meno = meno
        self.vek = vek

    def privitanie(self):
        print(f"Ahoj, ja som {self.meno}")

p = Osoba("Fero", 30)

print(p.vek)   # 30
del p.vek      # vymazanie atribútu vek

print(Osoba.druh)    # Clovek
del Osoba.druh       # vymazanie triedneho atribútu druh

p.privitanie()       # Ahoj, ja som Fero
del Osoba.privitanie # Vymazanie metódy privitanie
```

Po vymazaní atribútu alebo metódy už nie je možné ich naďalej používať. Ak sa o to pokúsime, Python vyhodí výnimku AttributeError.

Vymazanie atribútov nie je v Pythone častá vec, ale má svoje použitie:

- Uvoľnenie pamäti
- Dynamická zmena správania triedy
- Pokročilé techniky zapuzdrenia
- Odstránenie testovacieho kódu a dát
- Metaprogramovanie


## Vymazanie objektu

Pomocou príkazu `del` sa dajú odstrániť aj samotné objekty. Robí sa tak, ak je potrebné uvoľniť pamäť ešte pred tým, ako by sa objekt odstránil prirodzene (napr. po skončení funkcie, v ktorej bol objekt vytvorený).

```python

p = Osoba("Fero", 30)
del p;
```

Pri odstránení objektu pomocou `del` sa objekt hneď nevymaže. Na objekt totiž môže ukazovať aj nejaká iná premenná z iného miesta programu.
Python sleduje, koľko premenných ukazuje na daný objekt, a ak na objekt už nebude ukazovaž žiadna premenná, potom ho Python môže vymazať.

Tesne pred vymazaním sa v Pythone zavolá špeciálna metóda `__del__`, ktorá môže byť použitá na vlastné úpravy predtým, než sa objekt vymaže, napr. zatvorenie sieťového spojenia alebo súboru. Metóda, ktorá sa volá tesne pred vymazaním sa v objektovo orientovanom programovaní volá **deštruktor**

```python
class Demo:
    def __del__(self):
        print("Deštruktor sa zavolal!")

d = Demo()
del d  # alebo keď skončí program
```

## Špeciálne metódy

Minule sme si ukázali špeciálnu metódu `__str__`, dnes si vysvetlíme ďalšie.

### Dunder metóda `__repr__`

Podobne ako špeciálna metóda `__str__`, aj `__repr__` je metóda, ktorá vracia textovú reprezentáciu objektu.
Metóda `__repr__` však neslúži na vypísanie objektu pre človeka, ale o detailnú a technicky presnú reprezentáciu objektu, ideálne tak, aby sa z nej dal spätne vytvoriť rovnaký objekt.

Metóda `__repr__` sa zavolá pri volaní funkcie `repr`. Spätné vytvorenie objektu z textu je možné pomocou funkcie `eval`.

```python
class Osoba:
    def __init__(self, meno, vek):
        self.meno = meno
        self.vek = vek

    def __repr__(self):
        return f"Osoba(meno={self.meno!r}, vek={self.vek!r})"

o = Osoba("Eva", 30)
print(repr(o)) # Osoba(meno='Eva', vek=30)

eval(repr(o))  # vytvorenie objektu z textovej reprezentácie
```

Pri formátovaných stringoch vieme povedať, aby sa jednotlivé hodnoty tak isto vypisovali pomocou ich vlastnej `__repr__` metódy a to tak, že uvedieme modifikátor `!r` (viď predošlý príklad)

### Dunder metóda `__eq__`

Ak chceme, aby sa objekty našej triedy porovnávali podľa hodnoty a nie podľa identity, je potrebné definovať dunder metódu `__eq__`, ktorá sa zavolá pri porovnaní objektov. Ak ju nemáme, Python bude porovnávať podľa identity.

```python
class Osoba:
    def __init__(self, meno, vek):
        self.meno = meno
        self.vek = vek

    def __eq__(self, other):
        if not isinstance(other, Osoba):
            return NotImplemented
        return self.meno == other.meno and self.vek == other.vek

a = Osoba("Eva", 30)
b = Osoba("Eva", 30)
c = Osoba("Adam", 30)

print(a == b)  # True
print(a == c)  # False
print(a == 5)  # False, ale bez chyby
```

Pri implementácii si treba dávať pozor a na začiatku vždy skontrolovať, či je porovnávaný objekt tej istej triedy. Ak nie, je potrebné vrátiť hodnotu `NotImplemented`, ktorá Pythonu hovorí, že naša metóda to nevie porovnať a Python bude skúšať porovnať objekty inými spôsobmi.

### Aritmetické a relačné dunder metódy

Ak chceme, aby objekty našej triedy podporovali vybrané aritmetické a relačné operátory, je potrebné definovať príslušné dunder metódy. V nasledujúcej tabuľke je prehľad operátorov a ich dunder metód.

| Operátor | Metóda        |
| -------- | ------------- |
| `+`      | `__add__`     |
| `-`      | `__sub__`     |
| `*`      | `__mul__`     |
| `/`      | `__truediv__` |
| `==`     | `__eq__`      |
| `<`      | `__lt__`      |
| `<=`     | `__le__`      |
| `>`      | `__gt__`      |
| `!=`     | `__ne__`      |

```python
class Bod:
    def __init__(self, x, y):
        self.x, self.y = x, y
    def __add__(self, other):
        return Bod(self.x + other.x, self.y + other.y)
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    def __repr__(self):
        return f"Bod({self.x}, {self.y})"

print(Bod(1,2) + Bod(3,4))   # Bod(4, 6)
print(Bod(1,2) == Bod(1,2))  # True
```

## Variabilný počet argumentov

Funkcie a metódy v Pythone môžu prijímať premenlivý počet argumentov, podobne ako v Jave. Keďže Python podporuje aj kľúčové argumenty, problematika variabilného počtu argumentov je tu trošku zložitejšia.

### Premenlivý počet pozičných argumentov

Premenlivý počet pozičných argumentov deklarujeme pomocou hviezdičky, napr. `*args`. Vo vnútri funkcie budú argumenty v dátovom type **tuple**.

```python
def sucet(*args):
    print(args)  # tuple všetkých argumentov
    return sum(args)

print(sucet(1, 2, 3))  # → 6
print(sucet(10, 20))   # → 30
```

### Premenlivý počet kľúčových argumentov

Premenlivý počet kľúčových argumentov dekladujeme pomocou dvoch hviezdičiek, napr. `**kwargs`. Vo vnútri funkcie budú argumenty v dátovom type **dict**

```python
def pozdrav(**kwargs):
    for meno, vek in kwargs.items():
        print(f"{meno} má {vek} rokov")

pozdrav(Jozef=25, Anna=30)
# Output:
# Jozef má 25 rokov
# Anna má 30 rokov
```

### Rozbalenie pri volaní funkcie

Do argumentov funkcií môžeme rozbaliť hodnoty so zoznamu/tuple alebo aj celého slovnika (dict). Použijeme rovnaký symbol ako pri deklarácií premenlivého počtu argumentov.

```python
def add(x, y, z):
    return x + y + z

nums = [1, 2, 3]
print(add(*nums))  # rozbali tuple/list → 6

data = {"x": 10, "y": 20, "z": 5}
print(add(**data))  # rozbali dict → 35
```

Rozbaľovanie môžeme kombinovať s premenlivými argumentami.

```python
def vypis_vsetko(*args):
    for item in args:
        print(item)

zoznam = [1, 2, 3]
vypis_vsetko(*zoznam)
```

## Práca s dátovým formátom JSON

JSON (JavaScript Object Notation) je jednoduchý formát pre štruktúrované dáta, ktorý je čitateľný pre ľudí a jednoducho spracovateľný počítačom. Podporuje vnorené dáta a kolekcie, a je flexibilný. Patrí medzi najpopulárnejšie dátové formáty a je hojne používaný vo webových službách a aplikáciách.

JSON je vlastne textový formát, ktorý reprezentuje dve základné štruktúry:

1. Objekt (object) - pár kľúč-hodnota, podobne ako slovník v Pythone:

    ```json
    {
    "meno": "Jozef",
    "vek": 25,
    "student": true
    }
    ```

1. Zoznam (array) - usporiadaný zoznam, podobne ako list v Pythone:

    ```json
    [1, 2, 3, 4, 5]
    ```

Z dátových typov JSON podporuje čísla, reťazce a boolean hodnoty

Na čítanie JSON dát sa v pythone používa modul `json`.

```python
import json

data = '{"meno": "Jozef", "vek": 25, "student": true}'

# Konverzia textu na Python objekt (dict, list, atď.)
obj = json.loads(data)

print(obj)
print(obj["meno"])  # → Jozef
```

Metóda `json.loads` načíta JSON z reťazca. Na priame načítanie JSON dát zo súboru máme metódu `json.load`.

```python
import json

with open("data.json", "r", encoding="utf-8") as file:
    data = json.load(file)

print(data)
print(data["meno"])
```

Pre zápis dát do formátu JSON sa používajú metódy `json.dump` a `json.dumps`

### Načítanie objektu z JSON dát

Ak je štruktúra JSON dát známa a máme v našom programe triedu pre ich reprezentáciu, môžeme namiesto vytvorenia slovníkov a zoznamov z JSON dát vytvoriť objekty našich tried.

```python
import json

class Osoba:
    def __init__(self, meno, vek, student):
        self.meno = meno
        self.vek = vek
        self.student = student

    @classmethod
    def from_json(cls, json_string):
        data = json.loads(json_string)
        return cls(**data)

osoba = Osoba.from_json('{"meno": "Jozef", "vek": 25, "student": true}')
print(osoba.meno)        
```

## Úlohy na precvičenie

!!! example "Úloha 10.1: Trieda s utilitkami"

    1. Do vášho projektu si nainštalujte knižnicu `psutil` napr. pomocou `pip install psutil`

    1. Vytvorte si modul s názvom `movies`. Vložte do neho nasledovný kód triedy Utils:

    ```python
    import json
    import urllib.request
    import psutil

    # pip install psutil
    class Util:
        @staticmethod
        def get_memory_usage():
            process = psutil.Process()
            return f"{process.memory_info().rss / 1024 ** 2:.2f} MB"

        @staticmethod
        def read_url(url):
            print("Downloading " + url + "...")
            req = urllib.request.Request(url)
            with urllib.request.urlopen(url) as response:
                return response.read().decode('utf-8')

        @staticmethod
        def read_file(fname):
            print("Reading " + fname + "...")
            with open(fname, "r", encoding="utf-8") as file:
                return file.read()
    ```

!!! example "Úloha 10.2: Trieda Movies"

    1. Vytvorte triedu Movies s nasledovným kódom

        ```python
        class Movie:
            def __init__(self, title, year, cast = None, genres = None, extract = None, **kwargs):
                self._title = title
                self._year = year
                self._cast = cast
                self._genres = genres
                self._extract = extract

            @classmethod
            def from_dict(cls, data):
                return cls(**data)
        ```

        Všimnite si použitie `**kwargs` a `**data`. **Prečo je nutný `**kwargs`, ak keď sa v konštruktore nepoužíva?**

    1. Do triedy pridajte špeciálnu metódu `__str__` a naimplementujte ju

!!! example "Úloha 10.3: Inšpekcia dát"

    Vyskúšame si načítať dáta z JSON súboru a vytvoriť z nich objekty.

    Použite nasledovný kód a skúste pochopiť akú štruktúru má JSON

    ```python
    url = "https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json"

    s = Util.read_url(url)
    print("Vstupny JSON:")
    print(s[0:200])

    print("Nacitane data:")
    data = json.loads(s)
    print(data[33420])

    print("Objekt Movie:")
    movies = [Movie.from_dict(item) for item in data]
    print(movies[33420])
    ```

!!! example "Úloha 10.4: Trieda DB"

    1. Vytvorte triedu DB, ktorá bude mať na vstupe konštruktora JSON string a ten zparsuje do dátových štruktúr pythonu.

        Použite nasledovný kód:

        ```python
        class DB:
            def __init__(self, movies_str):
                self._raw = movies_str
                movies = json.loads(movies_str)
                self._movies = movies
                # index podľa názvu
                self._title_index = {}
                for item in movies:
                    title = item["title"]
                    if title not in self._title_index:
                        self._title_index[title] = []
                    self._title_index[title].append(Movie.from_dict(item))
        ```

        V konštruktore triedy sme vytvorili slovník (dict), ktorý zaindexoval filmy podľa názvu

    1. V triede vytvorte metódu `by_title`, ktorá vráti filmy podľa názvu z atribútu `_title_index`

    1. Vyskúšajte si funkčnosť napr. nasledovným kódom

        ```python
        url = "https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json"
        s = Util.read_url(url)
        db = DB(s)
        filmy = db.by_title("Up")
        print(", ".join(str(film) for film in filmy))
        ```

!!! example "Úloha 10.5: Index podľa hercov"

    1. Podobne ako máme index podľa hercov, tak upravte triedu DB tak, aby v konštruktore naviac
    vytvoril atribút `_actor_index`, do ktorého zaindexuje filmy podľa hercov.

    1. V triede DB vytvorte metódu `by_actor`, ktorá vráti filmy podľa herca z atribútu `_actor_index`

    1. Vytvorte metódu `actors_movies`, ktorá vypíše všetky filmy, kde daný herec hral, každý na samostatný riadok

    1. Vyskúšajte si funčnosť napr. nasledovným kódom

        ```python
        url = "https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json"
        s = Util.read_url(url)
        db = DB(s)
        db.actors_movies("Leonardo DiCaprio")
        ```

!!! example "Úloha 10.6: Uvoľnenie pamäti"

    Vyskúšame si zistiť, koľko pamäti zaberá náš program, a či sa po vymazaní niektorých atribútov uvoľní pamäť.

    Vyskúšajte si nasledovný kód:

    ```python
    print(Util.get_memory_usage())
    del db._raw
    del s
    print(Util.get_memory_usage())
    del db._actor_index
    print(Util.get_memory_usage())
    del db._title_index
    print(Util.get_memory_usage())
    ```

    Na základe hodnôt, ktoré sa vypíšu do konzoly skúste odhadnúť, koľko miesta v pamäti zaberali jednotlivé atribúty



## Zhrnutie cvičenia

- [x] Súkromné atribúty - iba dohodou
    * [ ] Názov začínajúci s jedným podčiarníkom hovorí, že daný atribút alebo metóda je interná a nemá byť používaná mimo triedy. 
    * [ ] Názvy začínajúce s dvoma podčiarníkmi pre súkromné atribúty a metódy. Python vykoná špeciálne premenovanie (anglicky name mangling)
- [x] Vymazanie atribútov a metód 
    * [ ] Pomocou `del` sa v Pythone sa dajú vymazať atribúty a dokonca aj metódy.
    * [ ] Využitie: Uvoľnenie pamäti, Dynamická zmena správania triedy, Odstránenie testovacieho kódu a dát
- [x] Deštruktor
    * [ ] Po zmazaní objektu a pri uvoľnení pamäti daného objektu sa zavolá deštruktor
    * [ ] Vlastný deštruktor implementujeme pomocou špeciálnej metódy `__del__`
- [x] Špeciálne metódy
    * [ ] `__repr__` pre detailnú a technicky presnú reprezentáciu objektu vo forme reťazca
    * [ ] Metóda `__repr__` sa zavolá pri volaní funkcie `repr`. Spätné vytvorenie objektu z textu je možné pomocou funkcie `eval`.
    * [ ] `__eq__` je metóda, ktorá sa volá pri porovnávaní objektov. Ak ju nevytvoríme, objekty sa budú porovnávať podľa identity
- [x] Aritmetické a relačné dunder metódy 
    * [ ] `__add__`, `__sub__`, `__mul__`, `__lt__`, `__gt__`, ...
- [x] Variabilný počet argumentov
    * [ ] Premenlivý počet pozičných argumentov deklarujeme pomocou hviezdičky, napr. `*args`. Vo vnútri funkcie budú argumenty v dátovom type `tuple`.
    * [ ] Premenlivý počet kľúčových argumentov dekladujeme pomocou dvoch hviezdičiek, napr. `**kwargs`. Vo vnútri funkcie budú argumenty v dátovom type `dict`
    * [ ] Rozbalenie pri volaní funkcie: Do argumentov funkcií môžeme rozbaliť hodnoty so zoznamu/tuple alebo aj celého slovnika (dict). Použijeme rovnaký symbol ako pri deklarácií premenlivého počtu argumentov.
- [x] Práca s dátovým formátom JSON
    * JSON (JavaScript Object Notation) je jednoduchý formát pre štruktúrované dáta, ktorý je čitateľný pre ľudí a jednoducho spracovateľný počítačom.
    * Na čítanie JSON dát sa v pythone používa modul `json`
    * Metóda `json.loads` načíta JSON z reťazca. Na priame načítanie JSON dát zo súboru máme metódu `json.load`
    * Pre zápis dát do formátu JSON sa používajú metódy `json.dump` a `json.dumps`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    METÓDY

    Súkromné atribúty - iba dohodou
    - jeden podčiarník - atribút alebo metóda je interná a nemá byť používaná mimo triedy. 
    - dva podčiarníky - súkromné atribúty. Vykoná sa špeciálne premenovanie (anglicky name mangling)

    Vymazanie atribútov a metód 
    - Pomocou del sa v Pythone sa dajú vymazať atribúty a dokonca aj metódy.
    - Využitie: Uvoľnenie pamäti, Dynamická zmena správania, Odstránenie testovacieho kódu

    Deštruktor
    - Po zmazaní objektu a pri uvoľnení pamäti daného objektu sa zavolá deštruktor
    - Vlastný deštruktor implementujeme pomocou špeciálnej metódy __del__

    Špeciálne metódy
    __repr__ pre technicky presnú reprezentáciu objektu vo forme reťazca
    __repr__ sa zavolá pri volaní funkcie repr. Spätné vytvorenie objektu pomocou funkcie eval.
    __eq__ pre porovnávanie objektov. Ak ju nevytvoríme, objekty sa budú porovnávať podľa identity
    
    Aritmetické a relačné dunder metódy 
    - __add__, __sub__, __mul__, __lt__, __gt__, ...

    VARARGS

    Variabilný počet argumentov
    - Premenlivý počet pozičných argumentov - *args - argumenty budú v tuple
    - Premenlivý počet kľúčových argumentov - **kwargs - argumenty budú v dict
    - Do argumentov funkcií môžeme rozbaliť hodnoty so zoznamu/tuple alebo aj celého slovníka (dict).
    - Pre rozbaľovanie použijeme rovnaký symbol ako pri deklarácií varargs v metóde (*, **)

    JSON

    JSON je formát pre štruktúrované dáta, čitateľný pre ľudí a jednoducho spracovateľný počítačom.
    - Metóda json.loads načíta JSON z reťazca, json.load so súboru
    - Pre zápis dát do JSON sa používajú metódy json.dump a json.dumps
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Pravidlá pre súkromné atribúty
    - Vymazanie atribútov a metód
    - Deštruktor
    - Špeciálne metódy repr, eq, aritmetické a relačné
    - Premenlivý počet argumentov
    - Rozbalenie pri volaní funkcie
    - Načítanie a zápis JSON dát
