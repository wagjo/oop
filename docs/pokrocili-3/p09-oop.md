# Pokročílí 9: Triedy a metódy

Dnešné cvičenie je venované triedam a objektom v Pythone. Python podporuje paradigmy objektovo orientovaného programovania, a umožňuje nám navrhnúť logiku programu pomocou tried a objektov.


## Trieda

Trieda sa v Pythone definuje kľúčovým slovom `class`. Pre názvy tried sa používa konvencia CamelCase, teda začínajú sa veľkým písmenom.

=== "Definícia triedy v Pythone"

    ```python
    class Obdlznik:
        pass
    ```

Do vnútra triedy píšeme jej atribúty a metódy.

## Konštruktor `__init__`

Konštruktor triedy má názov `__init__` a jeho prvý argument je referencia na vytváraný objekt. Je to podobné ako `this` v Jave, ale v Pythone sa to dáva ako prvý argument a jeho názov zvykne byť `self`.

=== "Trieda s konštruktorom"

    ```python
    class Obdlznik:
        def __init__(self, x, y):
            self.x = x
            self.y = y

    maly_obdlznik = Obdlznik(2, 3)
    ```

Novú inštanciu triedy (nový objekt) vytvoríme volaním triedy ako metódy.

## Inštančné metódy a atribúty

Klasické inštančné atribúty vytvoríme jednoducho tak, že im priradíme hodnotu. Na rozdiel od Javy nie je potrebná ich deklarácia vopred.

Inštančné metódy vytvoríme ako funkcie vnútri triedy, pričom prvý argument musí byť referencia na aktuálny objekt, podobne ako pri konštruktore.

=== "Inštančné metódy a atribúty"

    ```python
    class Obdlznik:
        def __init__(self, x, y):
            self.x = x
            self.y = y

        def obvod(self):
            return 2 * (self.x + self.y)


    maly_obdlznik = Obdlznik(2, 3)

    print(f"Obvod obdlznika: {maly_obdlznik.obvod()}")
    ```

## Dunder inštančná metóda `__str__`

V Pythone majú triedy veľké množstvo špeciálnych inštančných metód, ktoré upravujú chovanie triedy resp. jej objektov. Jednou z týchto metód je `__str__`, ktorá slúži na user-friendy (užívateľsky prívetivý) výpis objektu vo forme reťazca. Nasledujúci príklad demonštruje jej definíciu a použitie.

=== "Dunder inštančná metóda `__str__` "

    ```python
    class Obdlznik:
        def __init__(self, x, y):
            self.x = x
            self.y = y

        def obvod(self):
            return 2 * (self.x + self.y)

        def __str__(self):
            return f"{self.x}x{self.y}"


    maly_obdlznik = Obdlznik(2, 3)

    print(f"Obvod obdlznika {maly_obdlznik}: {maly_obdlznik.obvod()}")
    ```

## Triedne atribúty a metódy

Inštančné atribúty sú jedinečné pre každý objekt. Podobne ako v Jave, trieda môže obsahovať aj atribúty, ktoré patria triede a sú zdieľané všetkými objektami danej triedy. Takého atribúty sa v Pythone nazývajú **triedne atribúty** (anglicky class attributes).

Python poskytuje taktiež možnosť definovať aj **triedne metódy**, čo sú metódy, ktoré majú prístup k triede ale nie ku konkrétnemu objektu. Tieto metódy začínajú anotáciou `@classmethod` a ako prvý argument majú referenciu na triedu, ktorá sa zvykne volať `cls`.

Triedne metódy sa používajú napr. na tvorbu továrenských metód. K triednym atribútom a metódam sa dá pristupovať cez názov triedy ale aj cez názov objektu.

=== "Triedne atribúty a metódy"

    ```python
    class Teplomer:
        jednotka = "C"

        def __init__(self, teplota):
            self.teplota = teplota

        @classmethod
        def zmen_jednotku(cls, nova):
            cls.jednotka = nova

    moj_teplomer = Teplomer(37)
    print(Teplomer.jednotka)
    print(moj_teplomer.jednotka)
    ```

## Statické metódy

Python podporuje aj tzv. **statické metódy**, čo sú funkcie definované v triede, ktoré nemajú žiaden prístup ani k objektu ani ku triede. Pri ich definícii sa používa anotácia `@staticmethod`. Používajú sa na rôzne utilitky.

Názov statická metóda je síce podobný statickej metóde v Jave, avšak statické metódy a atribúty v Jave sa v Pythone volajú triedne metódy a atribúty.

=== "Statické metódy v Pythone"

    ```python
    class Matematika:
        @staticmethod
        def sucet(a, b):
            return a + b

        @staticmethod
        def mocnina(x):
            return x * x
    ```

## Úlohy na hodine

!!! example "Úloha 9.1: Trieda `BankovyUcet`"

    Do samostatného modulu vytvorte triedu `BankovyUcet`, ktorá bude mať atribúty `majitel`, `zostatok` a statické atribúty `poplatok` a `urok`.

    Poplatok nastavte na sumu `5.0` a úrok na `0.01`.

    Vytvorte konštruktor, ktorý bude prijímať argumenty majitel a zostatok.

    Vytvorte dunder metódu `__str__`.

    Vo funkcii main v moduli Main vytvorte inštancie tejto triedy a vypíšte ich do konzoly.

!!! example "Úloha 9.2: Vklady a výbery"

    Do triedy `BankovyUcet` pridajte inštančné metódy `vklad` a `vyber`, ktoré budu mať ako argument sumu peňazí. Kód vhodne ošetrite voči rôznym prípadom, napr. negatívne sumy, nedostatok prostriedkov na účte atď.

    Vytvorte vhodné príklady použitia týchto metód

!!! example "Úloha 9.3: Úroky a poplatky"

    Do triedy `BankovyUcet` pridajte inštančné metódy `pripocitaj_urok` a `odpocitaj_poplatok` a naimplementujte ich.

!!! example "Úloha 9.4: Trieda `Banka`"

    Do samostatného modulu vytvorte triedu `Banka`, ktorá bude mať inštančné atribúty `nazov` a `ucty`. Konštruktor triedy prijíma iba názov banky.

    Vytvorte inštančné metódy `otvor_ucet`, `najdi_ucet` a `vypis_vsetky_ucty`.

    Vytvorte dunder metódu `__str__` ktorá vypíše názov banky a počet účtov.

!!! example "Úloha 9.5: Uzávierka"

    V triede `Banka` vytvorte metódu `mesacna_uzavierka`, ktorá pre každý účet vykoná pripočítanie úroku a odpočítanie poplatku.

    Vytvorte vhodné príklady použitia triedy `Banka`

## Úlohy na precvičenie

!!! example "Úloha 9.6: Imanie"

    V triede `Banka` sledujte imanie banky. Upravte konštruktor tak, aby prijímal počiatočné imanie.
    Pri mesačnej uzávierke pripočítajte poplatky do imania a odpočítajte z neho úroky vyplatené účtom.

!!! example "Úloha 9.7: História tranzakcií"

    Pre každý účet zapisujte históriu všetkých tranzakcií do inštančného atribútu `historia`, ktorý bude zoznam reťazcov.

    Vytvorte metódu `zobraz_historiu`. Do histórie ukladajte aj neúspešné pokusz o výber z účtu, poplatky aj úrok

!!! example "Úloha 9.8: Reporty"

    V trieda `Banka` vytvorte metódy na zistenie a výpis celkového počtu účtov, celkového majetku, ktorý spravuje banka a maximálnej sumy na účte.

!!! example "Úloha 9.9: Povolené prečerpanie"

    Pre účty implementujte povolené prečerpanie. Vytvorte atribút `povolene_precerpanie`, ktora signalizuje, či má daný účet povolené prečerpanie a tiež atribút `limit_precerpania`, ktorý definuje max limit prečerpania. Upravte existujúce metódy tak, aby vedeli pracovať s prečerpaním. V triede `Banka` vytvorte metódy na aktiváciu a nastavenie prečerpania.

!!! example "Úloha 9.10: Dátumy tranzakcií"

    Do histórie si ukladajte čas tranzakcií.

    Vytvorte metódu na výpis tranzakcií z daného časového obdobia.

## Zhrnutie cvičenia

- [x] Triedy - class
    * [ ] Konštruktor pomocou metódy `__init__`, prvý argument je `self`
    * [ ] Inštancie sa vytváraju volaním triedy ako funkcia
- [x] Inštančné metódy a atribúty
    * [ ] Atribúty nie je potrebné deklarovať vopred
    * [ ] Metódy majú prvý argument `self`
- [x] Triedne metódy a atribúty
    * [ ] Triedne metódy majú anotáciu `@classmethod` a metódy majú prvý argument `cls`
- [x] Statické metódy
    * [ ] Majú anotáciu `@staticmethod` a nemajú žiaden prístup k triede alebo objektu
- [x] Dunder metódy
    * [ ] `__str__` - user friendly výpis triedy do reťazca


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Triedy - class
    - Konštruktor pomocou metódy __init__, prvý argument je self
    - Inštancie sa vytváraju volaním triedy ako funkcia

    Inštančné metódy a atribúty
    - Atribúty nie je potrebné deklarovať vopred
    - Metódy majú prvý argument `self`

    Triedne metódy a atribúty
    - Triedne metódy majú anotáciu @classmethod a metódy majú prvý argument cls

    Statické metódy
    - Majú anotáciu @staticmethod a nemajú žiaden prístup k triede alebo objektu

    Dunder metódy
    - __str__ - user friendly výpis triedy do reťazca
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Ako sa vytvára trieda 
    - Konštruktor `__init__`
    - Inštančné metódy a atribúty, `self`
    - Triedne metódy a atribúty, `cls`
    - Statické metódy
    - Dunder inštančná metódy `__str__`
