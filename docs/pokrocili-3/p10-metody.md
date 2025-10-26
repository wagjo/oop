# Pokročílí 9: Triedy a objekty

Dnešné cvičenie je venované triedam a objektom v Pythone. Python podporuje paradigmy objektovo orientovaného programovania, a umožňuje nám navrhnúť logiku programu pomocou tried a objektov.

- trieda
- triedne metódy, statické metódy
- triedne atribúty
- inštancia: vytváranie, mazanie, inštančné atribúty
- _protected, __private

## Trieda

Trieda sa v Pythone definuje kľúčovým slovom `class`. Pre názvy tried sa používa konvencia PascalCase, teda začínajú sa veľkým písmenom.

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

Triedne metódy sa používajú napr. na tvorbu továrenských metód.

=== "Triedne atribúty a metódy"

    ```python
    class Teplomer:
        jednotka = "C"

        def __init__(self, teplota):
            self.teplota = teplota

        @classmethod
        def zmen_jednotku(cls, nova):
            cls.jednotka = nova
    ```

## Statické metódy

Python podporuje aj tzv. **statické metódy**, čo sú funkcie definované v triede, ktoré nemajú žiaden prístup ani k objektu ani ku triede. Pri ich definícii sa používa anotácia `@staticmethod`. Používajú sa na rôzne utilitky.

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


## Zhrnutie cvičenia

![git architecure](../assets/git-flow.png){.on-glb}
/// caption
Štruktúra `git` repozitára a základné príkazy
///

- [x] Vzdialený repozitár
    * [ ] Pripojíme pomocou `git remote add <remote_nick> <remote_url>`
- [x] Odoslanie zmien 
    * [ ] Prvý krát pomocou `git push -u <remote_nick> <branch_name>`
    * [ ] Následné posielania už iba pomocou `git push`
- [x] Prijímanie zmien
    * [ ] Pomocou `git pull`
- [x] GitHub Issues
    * [ ] Slúžia na správu bugov, úloh a pod.
    * [ ] Každý issue má svoje číslo, v komentároch na neho odkazujeme pomocou mriežky, napr. #2
- [x] GitHub projects 
    * [ ] Umožňuje vytvoriť tabuľku s rôznymi stavmi issues, napr. štýlu Kanban
- [x] Pull requests
    * [ ] Nástroj na schvaľovanie zmien, ktoré sú v samostatných vetvách
    * [ ] Pull request vytvoríme vybraním vetvy, ktorá obsahuje zmeny, ktoré chceme dať schváliť
    * [ ] Človek, zodpovedný za schválenie vie pull request zlúčiť do hlavnej vetvy, alebo vrátiť na prepracovanie


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    VZDIALENÝ REPOZITÁR

    Pripojíme pomocou git remote add <remote_nick> <remote_url>

    Odoslanie zmien 
    Prvý krát pomocou git push -u <remote_nick> <branch_name>
    Následné posielania pomocou git push
 
    Prijímanie zmien - git pull

    GitHub Issues - správa bugov, úloh a pod.
    Každý issue má svoje číslo, odkazujeme na neho pomocou mriežky, napr. #2

    GitHub projects - tabuľky s rôznymi stavmi issues, napr. štýlu Kanban
    
    Pull requests - schvaľovanie zmien, ktoré sú v samostatných vetvách
    Po kontrole vieme pull request zlúčiť do hlavnej vetvy, alebo vrátiť na prepracovanie
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Ako sa pripojí vzdialený repozitár
    - Posielanie commitov do vzdialeného repozitára
    - Prijímanie zmien so vzdialeného repozitára
    - Na čo slúži GitHub issues
    - Na čo slúži GitHub projects
    - Na čo slúži GitHub pull requests
