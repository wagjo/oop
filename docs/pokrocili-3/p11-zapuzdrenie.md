# Pokročílí 11: Zapuzdrenie

V objektovo orientovanom programovaní máme 4 základné princípy: *zapuzdrenie, dedičnosť, polymorfizmus* a *abstrakciu*. 

Dnešná téma je o zapudrení a rôznych technikách v Pythone na dosiahnutie zapuzdrenia.

## Zapuzdrenie

Objekt v sebe spája stav (atribúty) a správanie (metódy) do jedného celku. Nie všetky jeho časti sú však určené na bežné použitie. Implementačné detaily (ako sa daná vec robí) by mali byť oddelené a skryté od verejných metód poskytujúcich hlavnú funkcionalitu (čo sa dá s objektom robiť). 

Zapuzdrenie (anglicky encapsulation) sa používa na skrytie interných detailov objektu a ochranu jeho dát pred neoprávneným prístupom alebo modifikáciou.

Zapuzdrenie umožňuje:

- Zvýšiť bezpečnosť
- Zlepšiť modularitu 
- Jednoduchšiu udržiavateľnosť kódu

Viac informácií o zapuzdrení nájdete v [teórii 10](../teoria-3/t10-zapuzdrenie.md), ktorá sa detailne venuje zapuzdreniu v Jave.

## Modifikátory prístupu

Základné zapuzdrenie je riešené pomocou obmedzenia prístupu k atribútom a metódam triedy. V Jave na to máme špeciálne kľúčové slová `public`, `protected` a `private`, v Pythone je to však riešené ináč.

Atribúty a metódy sú štandardne verejné, nie je potrebné to nijako špeciálne definovať. Na vytvorenie interných (protected) a súkromných atribútov a metód v Pythone používame podčiarníky v názve, tak ako sme si to vysvetlili na minulej hodine. Automatické premenovanie pri súkromných názvoch pomáha chrániť pred kolíziou mien pri dedení (o tom viac nabudúce).

| Úroveň    | Notácia     | Účel                                               |
| --------- | ----------- | -------------------------------------------------- |
| public    | `nazov`     | bežné použitie                                     |
| protected | `_nazov`    | interné, pre podtriedy                             |
| private   | `__nazov`   | ochrana pred kolíziou v dedičnosti (name mangling) |

## Getter, Setter a Deleter

Atribúty (inštančné premenné) uchovávajú stav daného objektu. Ich zverejnenie však nie je väčšinou žiadúce, nakoľko trieda prestáva mať kontrolu nad tým, aké hodnoty tieto atribúty budú mať. Preto je zvykom atribúty nezverejňovať, ale mať ich súkromné (private).

Na samotný prístup k hodnotám súkromných atribútov sa potom vytvárajú tvz. getter a setter metódy. V týchto metódach môže trieda ošetriť hodnoty a mať tak kontrolu nad stavom svojich atribútov. Ak trieda nezverejní setter metódu, používatelia triedy nebudú môcť priamo zmeniť daný atribút triedy.

Getter a setter metódy si vieme naimplementovať pomocou obyčajných inštančných metód a tento návrh sa bude podobať prístupu Javy.

=== "Getter a Setter metódy"

    ```python
    class Osoba:
        def __init__(self, meno):
            self._meno = meno

        def get_meno(self):
            return self._meno

        def set_meno(self, meno):
            if not meno:
                raise ValueError("Meno je povinne")
            self._meno = meno

    fero = Osoba("Fero")
    print(fero.get_meno())
    fero.set_meno("Jozo")
    print(fero.get_meno())
    ```

Tento prístup k tvorbe getter a setter metód však nie je v Pythone idiomatický (správny, odporúčaný postup). Python poskytuje elegantnejší spôsob definovania getter a setter metód, a to pomocou **property**.

### Property

Property funkcionalita v Pythone nám umožňuje upraviť spôsob prístupu k atribútom triedy tak, že navonok sa bude poskytovať akoby priamy prístup k atribútu, ale v skutočnosti sa budú volať getter a setter metódy, ktoré budú kontrolovať hodnoty atribútu.

Property nám umožňuje definovať 3 druhy metód pre daný atribút:

- getter metódu - zavolá sa pri pokuse o čitanie hodnoty atribútu
- setter metódu - zavolá sa pri pokuse o zápis hodnoty do atribútu
- deleter metódu - zavolá sa pri pokuse o zmazanie atribútu pomocou `del`

Na definovanie property v Pythone používme funkciu `property(fget, fset, fdel, doc)`, ktorej argumenty sú názvy getter, setter a deleter metód.

=== "Property funkcia"

    ```python
    class Osoba:
        def __init__(self, meno):
            self._meno = meno

        def get_meno(self):
            return self._meno

        def set_meno(self, meno):
            if not meno:
                raise ValueError("Meno je povinne")
            self._meno = meno

        meno = property(get_meno, set_meno)

    fero = Osoba("Fero")
    print(fero.meno)
    fero.meno ="Jozo"
    print(fero.meno)
    ```

Ešte elegantnejší prístup je použitie tzv. property dekorátorov, ktoré nám umožnia definovať getter a setter metódy bez vymýšľania špeciálnych názvovo pre getter a setter metódy.

=== "Property dekorátory"

    ```python
    class Osoba:
        def __init__(self, meno):
            self._meno = meno

        @property
        def meno(self):
            return self._meno

        @meno.setter
        def meno(self, meno):
            if not meno:
                raise ValueError("Meno je povinne")
            self._meno = meno

    fero = Osoba("Fero")
    print(fero.meno)
    fero.meno ="Jozo"
    print(fero.meno)
    ```

Tento prístup je odporúčaný a budeme ho v našom kóde používať aj my.

**Pri property metódach s dekorátormi musí byť v kóde prvá uvedená getter metóda!**

## Úlohy na hodine

Dnes si vyskúšame vytvoriť jednoduchú textovú hru. V tejto hre budeme prechádzať medzi miestnosťami, komplikovanejšie veci si necháme na inokedy. 

Hra čaká na náš príkaz. Príkazy sa zadávajú vo forme `sloveso podstatné_meno`, napr. `go vychod`, čo je príkaz na to, aby sme sa presunuli do miestnosti, ktorá je východným smerom.

Vytvoríme si balík `adventura` a 4 moduly s triedami:

- Hráč - `player.py`
- Miestnosť - `room.py`
- Stav hry - `hra.py`
- Logika hry - `engine.py`

Vstupný bod do hry bude v súbore `__main.py__`


!!! example "Úloha 11.1: Projekt adventúra"

    1. Vytvorte si nový projekt s názvom `adventura`

    1. Vytvorte adresár `assets` a stiahnite do neho súbor [hra11.json](http://oop.wagjo.com/assets/hra11.json)

    1. Vytvorte súbor `pyproject.toml` s nasledovným obsahom

        ```toml
        [project]
        name = "adventura"
        version = "0.0.1"
        dependencies = [
        ]
        ```

    1. Vytvorte adresár `src/adventura` a v ňom súbor `__main__.py` s kódom `print("hello world!")`

    1. Lokálne nainštalujte projekt pomocou `pip install -e .`
     
    1. Spustite projekt a overte, že funguje

    1. Vytvorte súbor `.gitignore` s obsahom [Python.gitignore](https://github.com/github/gitignore/blob/main/Python.gitignore) súboru

    1. Vytvorte si na svojom GitHube nový repozitár s názvom adventura

    1. Vytvorte si lokálne git repozitár a prepojte ho s GitHubom. Nahrajte svoj kód na GitHub


!!! example "Úloha 11.2: Modul hráča"

    Hráča budeme reprezentovať triedou `Player`. Zatiaľ si do tejto triedy uložíme iba informáciu o tom, v ktorej miestnosti sa hráč nachádza.

    1. Vytvorte modul `adventura/player.py`

    1. V module vytvorte triedu `Player` s atribútom `_room_id`, ktorý sa načíta v konštruktore. Argument konštruktora nech sa volá `start_room_id`

    1. Vytvorte getter a setter metódy `room_id` cez property dekorátor

    1. Vytvorte továrenskú metódu `from_dict(cls, data)`, ktorá vytvorí objekt zo slovníka `data` (pomocou rozbalenia parametrov ako sme si ukázali na minulej hodine)

    1. Overte funkčnosť pomocou nasledovného kódu:

        ```python
        def test():
            test_player = Player.from_dict({"start_room_id": 1})
            test_player.room_id = 2
            print("Test player.py: ", test_player.room_id)

        test()
        ```

!!! example "Úloha 11.3: Modul miestnosti"

    Miestnoť budeme reprezentovať triedou `Room`. Každá miestnosť má nasledovné informácie:

    - `room_id` - id miestnosti
    - `label` - názov miestnosti
    - `desc` - popis miestnosti
    - `exits` - list východov z miestnosti. Každý východ je dict s hodnotami `label` a `room_id`
    - `game_over` - reťazec so správou o ukončení hry. Ak je prítomný, vstupom do tejto miestnosti sa hra končí

    Postup:

    1. Vytvorte modul `adventura/room.py`

    1. V module vyvorte triedu `Room`, ktorá bude mať atribúty `_room_id`, `_label`, `_desc`, `_exits` a `_game_over`. Konštruktor nech má nasledovnú formu: `__init__(self, room_id, label, desc, exits=None, game_over=None)`

    1. Vytvorte getter metódy `game_over`, `desc`, `label` pomocou dekorátorov

    1. Vytvorte továrenskú metódu `from_dict(cls, data)`, ktorá vytvorí objekt zo slovníka `data` 

    1. Hodnoty `exits` upravte tak, aby sa v triede uložil nie zoznam východov, ale index východov podľa ich názvu (`label`)

        - V konštruktore upravte spracovanie vstupu `exists` nasledovne:

            ```python
            if exits is None:
                exits = []
            self._exits = {exit["label"]: exit for exit in exits}    
            ```

        - Getter metóda `exits` má vrátiť iba zoznam názvov východov, teda `list(self._exits.keys())`

        - Vytvorte metódu `exit(self, direction)`, ktorá vráti východ (celý dict) s daným názvom

    1. Overte funkčnosť pomocou nasledovného kódu:

        ```python
        def test():
            test_room = Room.from_dict({"room_id": "1",
                                        "label": "Čistinka",
                                        "desc": "Stojíš na kraji čistinky",
                                        "exits": [{
                                            "label": "zapad",
                                            "room_id": "2"
                                        }, {
                                            "label": "vychod",
                                            "room_id": "3"}]})
            print(test_room.label, test_room.desc, test_room.game_over)
            print(test_room.exits)
            print(test_room.exit("zapad"))

        test()
        ```

!!! example "Úloha 11.4: Modul stavu hry"

    Stav hry v sebe bude obsahovať slovník s miestnosťami, zaindexovaný podľa id miestnosti, a tiež atribút so stavom hráča.

    1. Vytvorte modul `adventura/hra.py` a importnite v ňom  nasledovné triedy:

        ```python
        from adventura.player import Player
        from adventura.room import Room
        ```

    1. Vytvorte triedu `Hra` s atribútmi `_player` a `_rooms`. Argumenty konštruktora nech sú `player` a `rooms`.

    1. Vytvorte getter metódu `player` pomocou property dekorátora

    1. Vytvorte továrenskú metódu `from_dict` s nasledovným kódom. Všimnite si, ako sa vytvára index miestností podľa ich id.

        ```python
        @classmethod
        def from_dict(cls, data):
            player = Player.from_dict(data['player'])
            rooms = {room["room_id"]: Room.from_dict(room) for room in data['rooms']}
            return cls(player, rooms)
        ```

    1. Vytvorte metódu `active_room(self)`, ktorá vráti slovník miestnosti, v ktorej sa práve hráč nachádza

    1. Vytvorte metódu `game_over(self)`, ktorá vráti hodnotu `game_over` z aktívnej miestnosti

    1. Overte funkčnosť pomocou nasledovného kódu:

        ```python
        def test():
            test_hra = Hra.from_dict({"player": {"start_room_id": "1"},
                                    "rooms": [{"room_id": "1",
                                                "label": "Čistinka",
                                                "desc": "Stojíš na kraji čistinky",
                                                "exits": [{
                                                    "label": "zapad",
                                                    "room_id": "2"
                                                }, {
                                                    "label": "vychod",
                                                    "room_id": "3"}]}]})
            print(test_hra.player.room_id)
            print(test_hra.active_room().label)

        test()
        ```

!!! example "Úloha 11.5: Modul logiky hry"

    Logiku hry nám bude zabezpečovať modul engine

    1. Vytvorte modul `adventura/engine.py` a importnite nasledovné veci

        ```python
        import json
        import textwrap

        from adventura.hra import Hra
        ```

    1. Vytvorte triedu `Engine` s troma atribútmi, `_hra`, `_intro` a `_outro`. Argumenty konštruktora nazvite podobne.

    1. Pridajte do triedy nasledovné továrenské metódy:

        ```python
        @classmethod
        def from_dict(cls, data):
            hra = Hra.from_dict(data)
            return cls(hra, data['intro'], data['outro'])

        @classmethod
        def load_game(cls, json_path):
            with open(json_path, encoding='utf-8') as json_file:
                data = json.load(json_file)
                return Engine.from_dict(data)
        ```

    1. Vytvorte metódu `intro(self)`, ktorá vypíše hodnotu atribútu intro na obrazovku a potom počká na stlačenie klávesy pomocou príkazu `input()`

    1. Vytvorte metódu `game_over(self, msg)`, ktorá vypíše "GAME OVER", potom vypíše msg a nakoniec vypíše hodnotu atribútu `outro`

    1. Vytvorte metódu `describe(self)`, ktorá Vypíše "Nachádzaš sa: " a názov aktívnej miestnosti. Aktívnu miestnosť získate z atribútu `_hra` zavolaním metódy `active_room()`. Potom nech vypíše popis miestnosti (atribút mestnosti `desc`). Nakoniec nech vypíše "Môžeš ísť: " a zoznam východov z miestnosti.

    1. Pridajte do triedy metódu `action` s nasledovným kódom

        ```python
        def action(self):
            try:
                commands = input("Čo chceš urobiť?: ").split()
                verb = commands[0]
                noun = commands[1] if len(commands) > 1 else None
                if verb == "help":
                    print()
                    print("!!!!!!!!!!!! HELP !!!!!!!!!!!!!!!!!!!!")
                    print("Pre ukončenie hry napíš exit")
                    print("Z miestnosti sa vieš presunúť príkazom go a smer, napríklad go vychod.")
                    print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
                elif verb == "exit":
                    return True
                elif verb == "go":
                    room = self._hra.active_room()
                    exit = room.exit(noun)
                    new_room_id = exit['room_id']
                    self._hra.player.room_id = new_room_id
            except KeyboardInterrupt as ex:
                return True
            except Exception as ex:
                print()
                print("!!!!!!!!!!!! CHYBA !!!!!!!!!!!!!!!!!!!!")
                print("Nerozumiem príkazu. Ak si nevieš dať rady napíš help")
                print("!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!")
        ```

    1. Pridajte do triedy metódu `play` s nasledovným kódom

        ```python
        def play(self):
            self.intro()
            while True:
                self.describe()
                game_over_msg = self._hra.game_over()
                if game_over_msg:
                    self.game_over(game_over_msg)
                    break
                should_quit = self.action()
                if should_quit:
                    self.game_over("Vzdal si to skôr ako si stihol nájsť poklad!")
                    break
        ```

!!! example "Úloha 11.6: Vstupný bod programu"

    Hra je takmer hotová, stačí pridať hlavný vstup programu.

    1. Do súboru `__main__.py` pridajte nasledovný kód.

        ```python
        from adventura.engine import Engine

        nova_hra_file = "assets/hra11.json"

        engine = Engine.load_game(nova_hra_file)

        engine.play()
        ```

    1. Nastavte konfiguráciu projektu tak, aby sa spúšťal modul adventura. Hore kliknite na `Edit Configuration`, vyberte `Python`, potom namiesto `script` vyberte `module` a názov modulu dajte `adventura`

    1. Hra je pripravená, spustite ju a skúste ju dohrať tak, aby skončila s Game Over hláškou


## Úlohy na precvičenie

!!! example "Úloha 11.7: Krajší výstup"

    1. Upravte vypísanie popisu miestnosti tak, aby sa text zalomil na 70 znakov

    1. Upravte text pri Game over tak, aby text vypisoval medzi riadkami výkričníkov, tak ako sa to vypisuje pri HELP

    1. Upravte text v metóde `describe`, aby bol výpis prehľadnejší, napr. aby sa to vypisovalo takto:

        ```
        --------------------------------------------------------------------------
        Nachádzaš sa: Čistinka
        --------------------------------------------------------------------------
        Stojíš na kraji čistinky, cez ktorú preteká zurčivý potôčik. Slniečko
        svieti a vo vzduchu cítiť rannú vôňu trávy. V strede čistinky je veľký
        pník a okolo neho rastú huby. Na západ vedie cesta do lesa. Cez
        potôčik ide drevená lávka, a po nej pokračuje chodník na východ.

        Môžeš ísť: zapad, vychod
        --------------------------------------------------------------------------
        Čo chceš urobiť?: _
        ```

!!! example "Úloha 11.8: Jednoduchší prechod medzi miestnosťami"

    Na prechod medzi miestnosťami je potrebné napísať príkaz `go` a potom smer, ktorým sa chcem vydať, napríklad `go vychod`.

    Upravte metódu action tak, aby sa naviac dalo prechádzať medzi miestnosťami iba tak, že sa napíše smer cesty, napr. `vychod`. Teda `go` sa bude dať použiť, ale pôjde to aj bez neho.

!!! example "Úloha 11.9: Vlastný svet"

    Miestnosti a ich prepojenia sa načítavaju zo súboru `hra11.json`. Vytvorte si vlastný súbor a vymyslite si aspon 5 prepojených miestností. Dajte hru zahrať svojmu susedovi.


## Zhrnutie cvičenia

- [x] V objektovo orientovanom programovaní máme 4 základné princípy: zapuzdrenie, dedičnosť, polymorfizmus a abstrakcia
    * [ ] Objekt v sebe spája stav (atribúty) a správanie (metódy) do jedného celku
- [x] Zapuzdrenie (anglicky encapsulation) sa používa na skrytie interných detailov objektu a ochranu jeho dát pred neoprávneným prístupom alebo modifikáciou.
    * [ ] Základné zapuzdrenie je riešené pomocou obmedzenia prístupu k atribútom a metódam triedy
    * [ ] Na vytvorenie interných (protected) a súkromných atribútov a metód v Pythone používame podčiarníky v názve
- [x] Getter, Setter a Deleter
    * [ ] Je zvykom atribúty nezverejňovať, ale mať ich súkromné (private)
    * [ ] Na samotný prístup k hodnotám súkromných atribútov sa potom vytvárajú tvz. getter a setter metódy
    * [ ] Getter a setter metódy si vieme naimplementovať pomocou obyčajných inštančných metód
- [x] Property - elegantnejší spôsob definovania getter a setter metód
    * [ ] Navonok sa bude poskytovať akoby priamy prístup k atribútu, ale v skutočnosti sa budú volať getter a setter metódy
    * [ ] getter metóda - zavolá sa pri pokuse o čitanie hodnoty atribútu
    * [ ] setter metóda - zavolá sa pri pokuse o zápis hodnoty do atribútu
    * [ ] deleter metóda - zavolá sa pri pokuse o zmazanie atribútu pomocou del
    * [ ] Na definovanie property v Pythone používme funkciu property(fget, fset, fdel, doc)
- [x] Property dekorátor
    * [ ] `@property`, `@atribut.setter`, `@atribut.deleter`
    * [ ] Pri property metódach s dekorátormi musí byť v kóde prvá uvedená getter metóda


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    ZAPUZDRENIE

    Objekt v sebe spája stav (atribúty) a správanie (metódy) do jedného celku

    Zapuzdrenie sa používa na skrytie interných detailov objektu

    Interné (protected) a súkromné (private) atribúty a metódy majú podčiarníky v názve

    Getter, Setter a Deleter
    - Je zvykom atribúty nezverejňovať, ale mať ich súkromné (private)
    - Na prístup k súkromným atribútom sa vytvárajú getter a setter metódy
    - Vieme ich naimplementovať pomocou obyčajných inštančných metód
    
    Property - elegantnejší spôsob na getter a setter metódy
    - Akoby priamy prístup k atribútu, ale v skutočnosti sa budú volať getter a setter metódy
    - getter - čitanie hodnoty atribútu
    - setter - zápis hodnoty do atribútu
    - deleter - zmazanie atribútu pomocou del
    - používame atribut = property(fget, fset, fdel, doc)

    Property dekorátor
    - @property, @atribut.setter, @atribut.deleter
    - V kóde musí byť prvá uvedená getter metóda!!!
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je zapuzdrenie, čo je jeho cieľom
    - Modifikátory prístupu v Pythone
    - Getter a Setter metódy
    - Property funkcionalita
    - Property dekorátor
