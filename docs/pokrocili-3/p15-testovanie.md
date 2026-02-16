# Pokročílí 15: Testovanie

Dnes si priblížime spôsoby a techniky testovania softvéru a vyskúšame si vytvoriť testy pomocou nástroja **pytest**.

## Testovanie softvéru

Testovanie je dôležitá súčasť vývoja softvéru. Jeho cieľom je overiť, že kód funguje správne, odhaliť chyby čo najskôr a zabezpečiť, že budúce zmeny v kóde nič nepokazia. Testovanie má nasledovný význam:

- Chyby sa nájdu skoro - oprava chyby v rannej fáze je lacnejšia a rýchlejšia.
- Zabraňuje regresii - po zmene kódu existujúca funkcionalita ostáva funčná.
- Slúži ako dokumentácia - dobre napísané testy ukazujú, ako sa má kód používať.
- Zvyšuje sebavedomie - ľahko vieme zistiť, či všetko stále funguje.

| Typ testu | Popis |
|------|-------|
 | **Unit testy**  | Testujú najmenšiu možnú časť kódu (funkciu, metódu, triedu) izolovane |
 | **Integračné testy**  | Overujú spoluprácu viacerých častí systému (moduly, DB, API) |
 | System / End-to-end  | Testujú celú aplikáciu ako čiernu skrinku (UI, API, DB spolu) |
 | Acceptačné testy  | Overujú, či aplikácia spĺňa biznis požiadavky (často s klientom) |
 | Performance testy  | Meranie rýchlosti, škálovateľnosti |
 | Security testy  | Hľadanie bezpečnostných dier |

Základné princípy dobrých testov (*FIRST*):

- **Fast** - rýchle (unit testy < 0.1 s)
- **Isolated** - nezávislé na iných testoch
- **Repeatable** - vždy rovnaký výsledok
- **Self-validating** - zreteľný výsledok PASS/FAIL
- **Timely** - píšu sa súbežne s kódom

## Pytest 

V Pythone je na písanie unit a integračných testov najpopulárnejší framework [pytest](https://pytest.org/).

Inštalácia je jednoduchá pomocou nástroja `pip`:

```shell
pip install pytest
```

Okrem samotného frameworku budeme potrebovať aj niektoré ďalšie rozšírenia, nainštalujeme ich taktiež pomocou `pip`:

```shell
pip install pytest-cov pytest-mock
```

Po nainštalovaní vieme spustiť testy v danom projekte jednoduchým zavolaním nástroja `pytest`. Ten nájde všetky testy v projekte a vykoná ich.

### Štruktúra testov

Súbor s testami musí mať názov vo formáte `test_*.py` alebo `*_test.py` a samotné testy musia byť funkcie začínajúce slovom `test_`. 

Tieto testy môžu byť buď súčasťou zdrojového kódu programu, alebo môžu byť v samostatnom adresári. V tomto prípade je potrebné projekt správne nakonfigurovať.

=== "Príklad testu"

    ```python
    def faktorial(n):
        if n == 0: return 1
        return n * faktorial(n - 1)

    def test_faktorial():
        assert faktorial(0) == 1
        assert faktorial(1) == 1
        assert faktorial(2) == 2
        assert faktorial(3) == 6
        assert faktorial(4) == 24
    ```

### assert, pytest.raises

V samotnom teste sa na overenie správnosti hodnoty používa výraz `assert`. Okrem neho sa ešte často používa `pytest.raises`, ktorý overí, či dané volanie vyhodilo výnimku.

=== "Príklad overenia vyhodenia výnimky"

    ```python
    def test_delenie():
        assert 2 / 2 == 1
        with pytest.raises(ZeroDivisionError):
            2 / 0
    ```

### pytest.approx

Pri testovaní čísel s pohyblivou desatinnou čiarkou je nutné testovať približnú hodnotu, nie presnú. V pyteste sa to dá pomocou funkcie `pytest.approx`.

=== "Približné porovnávanie hodnôt"

    ```python
    def test_priemer():
        hodnoty = [0.1, 0.2, 0.3]
        ocakavany = 0.2
        vysledok = priemer(hodnoty)

        # assert vysledok == ocakavany  # ERROR
        assert vysledok == pytest.approx(ocakavany) # OK
    ```

### parametrizácia testov

Často chceme otestovať viacero vstupov. Namiesto viacnásobných `assert` príkazov vieme test parametrizovať a testované hodnoty posielať ako argumenty do testov. Na to nám v pyteste slúži anotácia `@pytest.mark.parametrize`.

=== "Parametrizovanie testu"

    ```python
    @pytest.mark.parametrize("hodnoty, ocakavany", [
        ([1, 2, 3], 2.0),
        ([1.0, 2.0, 3.0], 2.0),
        ([0.1, 0.2, 0.3], 0.2),
        ([0.1 + 0.2, 0.3], 0.3),
        ([1e10 + 1, 1e10], 1e10 + 0.5),
        ([], 0.0),
    ])
    def test_priemer(hodnoty, ocakavany):
        vysledok = priemer(hodnoty)
        assert vysledok == pytest.approx(ocakavany) # OK
    ```

## Fixtures

Netriviálne testy často potrebujú nastaviť určitý počiatočný stav aplikácie, aby sa dala daná funkcionalita správne otestovať. Tento počiatočný stav je často rovnaký pre viacero testov a pre správne fungovanie testov musí byť nastavený pred každým jedným volaním testu.

V pyteste nám na nastavenie počiatočného stavu slúži tzv. fixture. Je to špeciálne oanotovaná funkcia pomocou `@pytest.fixture`, ktorá vráti počiatočný stav. Tento stav sa potom posiela ako argument do samotných testov. 

=== "Použitie fixture"

    ```python
    @pytest.fixture
    def user():
        # set up logika
        return {"first_name": "Fero",
                "last_name": "Horvát",
                "address": "Poštová 10",
                "city": "Prešov"}

    def test_full_name(user):
        assert full_name(user) == "Fero Horvát"

    def test_full_address(user):
        assert full_address(user) == "Poštová 10, Prešov"
    ```

## Mocking

Integračné testy často precujú s databázov alebo externými službami. Pri testovaní to predstavuje problém, pretože nemáme kontrolu nad tým, aké dáta nám externá služba alebo databáza poskytne, teda nevieme zaručiť reprodukovateľnosť testov.

Ďalšou prekážkou je, že správne nastaviť databázy a externé služby pre testovanie je časovo aj technicky náročné. Riešením je tzv. mocking, kedy počas testovania nahradíme volania k databáze alebo externým službám fiktívnymi volaniami, ktoré namiesto skutočných dát vrátia nami definované testovacie hodnoty. Okrem kontroly nad dátami je takýto mocking aj oveľa rýchlejší ako volanie skutočných služieb.

V pytest sa mocking robí pomocou špeciálneho fixture `mocker`, ktorý nám umožňuje vytvárať mock objekty. Tento mocking je súčasťou rozšírenia pytestu s názvom `pytest-mock` a je ho potrebné samostatne nainštalovať pomocou `pip`.

Pomocou `mocker` objektu vieme v testovanom module nahradiť funkcie alebo triedy. Nasledujúci príklad nahradí volanie HTTP služby fiktívnou funkciou, ktorá vráti testovanie dáta.

=== "Mocking pomocou pytest-mock"

    ```python
    def test_load_json(mocker):
        fake_data = b'{"foo": "bar"}'
        mocker.patch("urllib.request.urlopen", mocker.mock_open(read_data=fake_data))

        x = load_json("http://example.com")
        assert x == {"foo": "bar"}
    ```

## Generativne testovanie

Ak naša funkcia prijíma na vstupe čísla, reťazce alebo iné typy dát, ako si vieme byť istý, že funguje správne? V unit testoch vieme otestovať zopár vstupných hodnôt, niektoré krajné situácie, ale všetky možné kombinácie to testu často nevieme napísať. 

Populárna knižnica `hypothesis` nám umožňuje vytvoriť testy, ktoré otestujú všetky kobinácie vstupov a hraničných hodnôt. Tento spôsob testovania sa používa hlavne, keď si chceme otestovať, že daná funkcia nebude vyhadzovať výnimku alebo ak vieme jednoducho vypočítať očakávaný výstup pre rôzne vstupy. 

Knižnica hypothesis (`pip install hypothesis`) nám vie vygenerovať kombinácie čísel (`strategies.integers()`, `strategies.floats()`), reťazcov (`strategies.text()`), alebo aj iné zložitejšie dáta.

=== "Generatívne testovanie pomocou hypothesis"

    ```python
    from hypothesis import given, strategies as st

    # obyčajný test
    def test_obvod_stvorca():
        assert obvod_stvorca(1) == 4

    # generatívny test
    @given(st.integers())
    def test_obvod_stvorca_all(x):
        assert obvod_stvorca(x) == obvod_obdlznika(x, x)
    ```

## Coverage

Pri väčších projektoch môžeme stácať prehľad, ktoré funkcionality majú testy a ktoré nie. Na uľahčenie tohto procesu slúži tzv. coverage, ktorý nám vie poskytnúť prehľadný report o tom, ktoré časti nášho programu sú 'pokryté' testami.

Tento nástroj je rozšírenie pytestu a inštaluje sa pomocou `pip install pytest-cov`. 

Generovanie reportu o pokrytí testov spustíme pomocou `pytest --cov=src`. Príklad výstupu:

```shell
==================================== tests coverage =====================================
____________________ coverage: platform linux, python 3.13.7-final-0 ____________________

Name                   Stmts   Miss  Cover
------------------------------------------
src/spse/__init__.py       0      0   100%
src/spse/json.py           8      0   100%
src/spse/tvary.py          8      2    75%
src/spse/user.py           4      0   100%
src/spse/util.py          21      5    76%
------------------------------------------
TOTAL                     41      7    83%
================================== 15 passed in 0.21s ===================================
```

## Úlohy na hodine

!!! example "Úloha 15.1: Projekt s utilitkami"

    Vytvorte si nový projekt a do adresára `src` umiestnite balík `spse` s nasledovnými modulmi:

    ```python title="src/spse/json.py"
    import json
    import urllib.request

    def load_json(url):
        req = urllib.request.Request(url)
        with urllib.request.urlopen(url) as response:
            s = response.read().decode('utf-8')
            parsed = json.loads(s)
            return parsed
    ```

    ```python title="src/spse/tvary.py"
    def obvod_obdlznika(a, b):
        return 2 * a + 2 * b

    def obsah_obdlznika(a, b):
        return a * b

    def obvod_stvorca(a):
        return obvod_obdlznika(a, a)

    def obsah_stvorca(a):
        return obsah_obdlznika(a, a)
    ```

    ```python title="src/spse/user.py"
    def full_name(data):
        return data["first_name"] + " " + data["last_name"]

    def full_address(data):
        return data["address"] + ", " + data["city"]
    ```

    ```python title="src/spse/util.py"
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

    def priemer(hodnoty):
        if not hodnoty:
            return 0.0
        return sum(hodnoty) / len(hodnoty)
    ```

    1. V projekte vytvorte súbor `pyproject.toml` s nasledovným obsahom:

        ```python title="pyproject.toml"
        [project]
        name = "opgp-test"
        version = "0.0.1"
        dependencies = [
            "pytest",
            "pytest-cov",
            "pytest-mock",
            "hypothesis"]
        description = "OPGP test"

        [tool.pytest]
        pythonpath = ["src"]
        testpaths = ["tests"]
        addopts = ["--import-mode=importlib"]
        ```

    1. Vytvorte adresár `tests` a v ňom súbor `__init__.py`

    1. Adresár src označkujte ako zdrojový a adresár tests označte ako testovací

    1. Nainštalujte závislosti projektu pomocou `pip install -e .`


!!! example "Úloha 15.2: Základné testy"

    V adresári `tests` vytvorte súbor `test_util.py` a v ňom vytvorte nasledovné testy, ktoré budú testovať modul `spse.util`

    1. `test_faktorial` - funkčnosť overte pomocou `assert`

    1. `test_fib` - funkčnosť overte pomocou `assert`

    1. `test_obrat` - funkčnosť overte pomocou `assert`

    1. `test_delenie` - funkčnosť overte pomocou `assert` a tiež pomocou `pytest.raises(ZeroDivisionError)` pri denelí nulou

    Spustite testy pomocou `pytest` a skontrolujte či testy prejdú.

    Vygenerujte report o pokrytí pomocou `pytest --cov=src` a skontrolujte


!!! example "Úloha 15.3: Približné hodnoty a parametrizácia"

    V súbore `test_util.py` pridajte test na `test_priemer`

    Otestujte hodnoty pomocou `pytest.approx`

    Vstupné hodnoty do testu vložte pomocou `@pytest.mark.parametrize`. Zadefinujte aspoň 7 rôznych vstupov a výstupov.

    Spustite testy a vygenerujte report pomocou `pytest --cov=src`


!!! example "Úloha 15.4: Fixtures"

    V adresári `tests` vytvorte súbor `test_user` a v ňom vytvorte testy `test_full_name` a `test_full_address`, ktoré budú testovať modul `spse.user`.

    Ako vstup do testov použite fixture `user` pomocou `@pytest.fixture`, ktorý vráti dictionary objekt s testovacími dátami.

    Spustite testy a vygenerujte report pomocou `pytest --cov=src`

!!! example "Úloha 15.5: Mocking"

    V adresári `tests` vytvorte súbor `test_json` a v ňom vytvorte test `test_load_json`, ktorý bude testovať modul `spse.json`.

    V teste vytvorte mock volania `urllib.request.urlopen` tak, aby volanie funkcie `spse.json.load_json` vrátilo testovací JSON, ktorý potom viete overiť pomocou `assert`.

    Spustite testy a vygenerujte report pomocou `pytest --cov=src`

!!! example "Úloha 15.6: Generatívne testovanie"

    V súbore `test_util` vytvorte nový test `test_obrat_gen`, ktorý bude testovať funkciu obrat pre všetky kombinácie vstupov pomocou generatívneho testovania.

    Použite anotáciu `@given(st.text())` a otestujte, že dvojnásobne volanie funkcie `obrat` vracia pôvodný text.

    Vytvorte nový súbor `test_tvary.py` a v ňom vytvorte `test_obvod_stvorca`, ktorý bude testovať funkciu z modulu `spse.tvary`. Použite `assert`

    Potom vytvorte test `test_obdod_stvorca_all` a použite generatívne testovanie na overenie všetkých kombinácii vstupov. Otestujte, že obdov štvorca sa rovná výsledku volania `obvod_obdlznika` s tými istými stranami. Použite `@given(st.integers())`


## Zhrnutie cvičenia

- [x] Testovanie softvéru - cieľom je overiť, že kód funguje správne a odhaliť chyby čo najskôr
    * [ ] Chyby sa nájdu skoro - oprava chyby v rannej fáze je lacnejšia a rýchlejšia.
    * [ ] Zabraňuje regresii - po zmene kódu existujúca funkcionalita ostáva funčná.
    * [ ] Slúži ako dokumentácia - dobre napísané testy ukazujú, ako sa má kód používať.
    * [ ] Zvyšuje sebavedomie - ľahko vieme zistiť, či všetko stále funguje.
- [x] Typy testov, ktoré vieme pomocou `pytest` robiť
    * [ ] Unit testy - Testujú najmenšiu možnú časť kódu (funkciu, metódu, triedu) izolovane
    * [ ] Integračné testy - Overujú spoluprácu viacerých častí systému (moduly, DB, API)
- [x] Základné princípy dobrých testov (FIRST):
    * [ ] Fast - rýchle (unit testy < 0.1 s)
    * [ ] Isolated - nezávislé na iných testoch
    * [ ] Repeatable - vždy rovnaký výsledok
    * [ ] Self-validating - zreteľný výsledok PASS/FAIL
    * [ ] Timely - píšu sa súbežne s kódom
- [x] Pytest - `pip install pytest`
    * [ ] Súbor s testami musí mať názov vo formáte `test_*.py` alebo `*_test.py` a samotné testy musia byť funkcie začínajúce slovom `test_`. 
    * [ ] V samotnom teste sa na overenie správnosti hodnoty používa výraz `assert`. 
    * [ ] `pytest.raises` overí, či dané volanie vyhodilo výnimku.
    * [ ] `pytest.approx` testuje približnú hodnotu, nie presnú
    * [ ] Parametrizovanie testov pomocou `@pytest.mark.parametrize`
    * [ ] Fixture, `@pytest.fixture` slúži na nastavenie počiatočného stavu alebo prostredia, zdieľaného medzi testami
- [x] Mocking - `pip install pytest-mock`
    * [ ] Počas testovania nahradíme volania k databáze alebo externým službám fiktívnymi volaniami, ktoré namiesto skutočných dát vrátia nami definované testovacie hodnoty.
    * [ ] V pytest sa mocking robí pomocou špeciálneho fixture mocker, ktorý nám umožňuje vytvárať mock objekty. 
    * [ ] Pomocou mocker objektu vieme v testovanom module nahradiť funkcie alebo triedy
- [x] Generatívne testovanie - `pip install hypothesis`
    * [ ] Umožňuje vytvoriť testy, ktoré otestujú všetky kombinácie vstupov a hraničných hodnôt
    * [ ] Používa sa hlavne, keď si chceme otestovať, že daná funkcia nebude vyhadzovať výnimku alebo ak vieme jednoducho vypočítať očakávaný výstup pre rôzne vstupy. 
    * [ ] Hypothesis vie vygenerovať kombinácie čísel (`strategies.integers()`, `strategies.floats()`), reťazcov (`strategies.text()`), alebo aj iné zložitejšie dáta.
- [x] Coverage - `pip install pytest-cov`
    * [ ] Coverage nám vie poskytnúť prehľadný report o tom, ktoré časti nášho programu sú 'pokryté' testami
    * [ ] Generovanie reportu o pokrytí testov spustíme pomocou `pytest --cov=src`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    TESTOVANIE

    Cieľom testovanie je overiť, že kód funguje správne a odhaliť chyby čo najskôr. Vlastnosti:
    - Chyby sa nájdu skoro
    - Zabraňuje regresii
    - Slúži ako dokumentácia
    - Zvyšuje sebavedomie

    Typy testov podporovaných pytestom
    - Unit testy - Testujú najmenšiu možnú časť kódu
    - Integračné testy - Overujú spoluprácu viacerých častí systému

    Základné princípy dobrých testov (FIRST):
    - Fast
    - Isolated
    - Repeatable
    - Self-validating
    - Timely

    PYTEST - pip install pytest

    Súbor s testami test_*.py alebo *_test.py a testy sa začínajú s test_. 
    assert - overenie správnosti hodnoty
    pytest.raises - overenie vyhodenia výnimky
    pytest.approx - testuje približnú hodnotu, nie presnú
    @pytest.mark.parametrize - parametrizovanie testov
    @pytest.fixture - fixture, slúži na nastavenie počiatočného stavu alebo prostredia 
    zdieľaného medzi testami

    Mocking - pip install pytest-mock
    Nahradíme volania k databáze alebo externým službám fiktívnymi volaniami
    Špeciálny fixture mocker, ktorý nám umožňuje vytvárať mock objekty. 
    Vieme v testovanom module nahradiť funkcie alebo triedy

    Generatívne testovanie - pip install hypothesis
    Testy, ktoré otestujú všetky kombinácie vstupov a hraničných hodnôt
    Vieme vygenerovať kombinácie čísel (strategies.integers(), strategies.floats()), 
    reťazcov (strategies.text()), alebo aj iné zložitejšie dáta.

    Coverage - pip install pytest-cov
    Prehľadný report o tom, ktoré časti nášho programu sú 'pokryté' testami
    Generovanie reportu pomocou pytest --cov=src
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Vlastnosti testovania
    - Typy testov
    - FIRST princípy dobrých testov
    - Základné funkcie knižnice pytest
    - Čo je fixture a mocking
    - Generatívne testovanie - na čo slúži
    - Coverage - na čo slúži