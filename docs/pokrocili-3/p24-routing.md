# Pokročílí 24: URL a Routing

HTTP nám poskytuje štandardizovaný prístup k zdrojom na internete. HTML súbory, dokumenty, obrázky, to všetko môžeme požiadať a stiahnuť zo serveru pomocou HTTP protokolu. 

Keďže na serveri sa nachádza viacero zdrojov, musíme medzi nimi vedieť rozlíšiť a v HTTP request správe jednoznačne pomenovať resp. identifikovať ten zdroj, o ktorý máme záujem. Na to nám slúži formát URL a technológia routovania, ktoré si dnes predstavíme.

## Uniform Resource Locator

URL (Uniform Resource Locator) je webová adresa, ktorú píšeme do prehliadača. Má presne definovanú štruktúru podľa štandardu [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986.html).

URL v sebe obsahuje všetko potrebné, aby sa jednoznačne dal na internete nájsť tzv. zdroj, ako napríklad HTML súbor, obrázok, video, dokument, atď. 

URL určuje:

- **kde** sa zdroj nachádza
- **ako** sa k nemu dostať

Príklady reálnych URL:

- `https://www.example.com/blog/2025/04/moj-prispevok?id=15&preview=true#uvod`
- `http://localhost:8080/api/users?limit=10&offset=20`
- `https://user:tajne@mojserver.sk:8443/admin`
- `http://fero:heslo123@www.example.com:8080/blog/prispevok-1?id=42&lang=sk&sort=date#komentare`


URL má svoju presnä štruktúru a skladá sa z viacerých častí. Kompletný tvar URL je nasledovný:

```
schéma://používateľ:heslo@host:port/cesta?query=parametre#fragment
```

Jednotlivé časti URL majú nasledovný význam:


| Časť | Príklad | Význam |
|----|----|----|
| Schéma | `http` | Protokol (`http`, `https`, `ftp`, `file`, ...)|
| Používateľ + heslo | `fero:heslo123` | používateľské meno a heslo, tzv. Authority, obe voliteľné |
| Host | `www.example.com` | Doména alebo IP adresa |
| Port | `8080` | Port (voliteľný) |
| Path | `/blog/prispevok-1` | Cesta k zdroju
| Query string | `id=42&lang=sk&sort=date` | Parametre požiadavky (key=value páry oddelené znakom `&`) |
| Fragment | `komentare` | Umiestnenie (anchor) na stránke (spracováva sa v prehliadači, server ho zvyčajne nedostane) | 

!!! info "Bezpečnosť a súkromie v URL"

    URL sa často uchováva v logoch a iných záznamoch a je taktiež voľne viditeľný v prehliadači. Je preto dobrým zvykom nedávať do URL žiadne citlivé informácie, ako napr. heslo, prístupový kľúč, alebo iné súkromné dáta. Ak je potrebné tieto dáta odoslať serveru, je oveľa lepšie ich umiestniť do tela request správy a použiť HTTP metódu POST namiesto GET.

### Dovolené znaky

Keďže niektoré znaky majú v URL špeciálny význam, nie všetky znaky sú v názvoch a jednotlivých častiach povolené.

Ak chceme v niektorej časti URL uviesť znak, ktorý nie je povolený, musíme ho zakódovať pomocou znaku percenta (tzv. percent encoding). 

V percent encodinu zakódujeme znak pomocou reťazca `%XX`, kde `XX` je hexadecimálna hodnota znaku v kódovaní UTF-8. V niektorých prípadoch je znak reprezentovaný viacerými bajtami a teda aj výsledne kódovanie pomocou percenta bude dlhšia.

Príklady percent kódovania:

- medzera - `%20`
- lomítko `/` - `%2F`
- písmeno s diakritikou `é` - `%C3%A9`
- percento `%` - `%25`

Pri spracovaní a generovaní URL nesmieme na toto kódovanie zabudnúť, hlavne pri query parametroch a ceste. V Pythone sa na to použíajú funkcie `quote()` a `unquote()` z modulu `urllib.parse`. Príklad:

- `quote('/El Niño/')` vráti reťazec `/El%20Ni%C3%B1o/`

### Parsovanie URL v Pythone

Python má na prácu s URL vstavaný modul `urllib.parse`. Medzi najdôležitejšie funkcie v tomto module patria:

- `urlparse()` - rozdelí URL do jednotlivých častí
- `parse_qs()` - vytvorí z query stringu slovník s hodnotami
- `quote()`/`unquote()` - kódovanie znakov

=== "Príklad parsovania URL reťazca"

    ```python
    from urllib.parse import urlparse, parse_qs, unquote

    url = "https://www.example.com/blog/prispevok?id=42&meno=Peter%20Novak#uvod"

    parsed = urlparse(url)

    print("Scheme:     ", parsed.scheme)      # https
    print("Netloc:     ", parsed.netloc)      # www.example.com
    print("Hostname:   ", parsed.hostname)    # www.example.com
    print("Port:       ", parsed.port)        # None
    print("Path:       ", parsed.path)        # /blog/prispevok
    print("Query:      ", parsed.query)       # id=42&meno=Peter%20Novak
    print("Fragment:   ", parsed.fragment)    # uvod

    # Parsovanie query parametrov
    params = parse_qs(parsed.query)
    print("Params:     ", params)             
    # {'id': ['42'], 'meno': ['Peter Novak']}

    print("Dekódované meno:", unquote(params['meno'][0]))  # Peter Novak
    ```

## Routovanie

Routovanie predstavuje proces mapovania HTTP požiadavky na konkrétny zdroj resp. funkcionalitu, prístupnú pomocou HTTP protokolu.

Na identifikáciu cieľa routovania (zdroja) sa používajú údaje z request správy, najmä z hlavičky:

- cesta z URL (anglicky URL path)
- HTTP metóda (GET, POST)
- Názov hostiteľa, hostname (www.example.com)
- Dodatočné hodnoty z hlavička (Content-Type, SessionId, ...)

Webový server po prečítaní request hlavičky zistí cestu k zdroju a HTTP metódu a na základe toho sa rozhodne, akú odpoveď pošle naspäť klientovi.

Príklady:

- `GET /` - hlavná stránka
- `GET /blog/prispevok-1` - konkrétny článok
- `POST /api/login` - prihlásenie
- `GET /images/logo.png` - statický súbor

Druhy routovania:

- statické, URL cesta musí presne zodpovedať vzoru. `/about` -> about_handler()
- dynamické, niektoré časti URL cesty sú parametrizované a handler tak vie obslúžiť celú množinu ciest. `/users/<id>` -> user_detail(id)

Process routovania:

1. príde HTTP request
1. server rozparsuje URL
1. prejde zoznam "routov"
1. nájde vhodný match
1. zavolá handler

Handler je funkcia, ktorá je pridelená k danému "routu", a ktorá vykoná príslušnú logiku podľa dohody (vráti súbor, prihlási užívateľa, ...)
Okrem parametrizovaných častí cesty pri dynamickom routovaní má handler k dispozícii aj query parametre (pozri URL kapitolu), ktoré boli zaslané v danom URL.

### Routovanie v Pythone

Proces routovania by sa dal naimplementovať pomocou klasického Pythonu napr. takto:

```python
def router(method, path):
    if method == "GET" and path == "/users":
        return get_users()
    elif method == "GET" and path.startswith("/users/"):
        user_id = path.split("/")[2]
        return get_user(user_id)
```

Tento prístup má však mnoho nedostatkov. Napríkladie je modulárny a pri veľkom počte zdrojov bude značne neprehľadný. Preto sa pre routovanie používajú na to určené knižnice.

## Flask

Jednou z populárnych knižníc pre webové servery, ktorá ponúka aj routovanie je [**Flask**](https://flask.palletsprojects.com/en/stable/). Táto knižnica ponúka tzv. mikro web framework, ktorý je vhodný pre jednoduché webové aplikácie.

Flask si nainštalujete pomocou `pip install flask`.

Jednotlivé "routes" si definujete pomocou anotácií. Nasledujúce príklady sú z repozitára [https://github.com/wagjo/opg-tcp](https://github.com/wagjo/opg-tcp)

=== "Príklad routovania pomocou Flask"

    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route("/")
    def index():
        return "Hello World!"

    @app.route("/users", methods=["GET"])
    def get_users():
        return "users"

    @app.route("/users/<int:user_id>")
    def get_user(user_id):
        return f"user {user_id}"

    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=8080, debug=True)
    ```

### Statické súbory

Statické súbory ako napr. štýly, javascript a obrázky Flask vie sprístupniť ak ich umiestnite do adresára `static`. Nie je potrebné písať pre nich samostatné routes.

### HTML stránky

HTML stránky je možné generovať pomocou tzv. templatovania. Flask podporuje templatovací nástroj Jinja2. Samotné šablóny HTML stránok sa umiestňujú do adresára `templates`

=== "Príklad HTML stránok pomocou templatovania"

    ```python
    from flask import Flask, request, render_template

    app = Flask(__name__)

    @app.route("/")
    def index():
        return render_template("index.html")

    @app.route("/about")
    def about():
        name = request.args.get("name", "Fero")
        return render_template("about.html", name=name)

    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=8080, debug=True)
    ```

### Statusové kódy

Pre vrátenie iných statusových kódov ako `200 OK` má flask funkciu `abort()`, v ktorej môžeme uviesť statusový kód chyby.

=== "Príklad statusových kódov"

    ```python
    from flask import Flask, abort

    app = Flask(__name__)

    @app.get("/login")
    def login():
        abort(401)

    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=8080, debug=True)
    ```

## Úlohy

!!! example "Úloha 24.1: Webový server pomocou Flask"

    Vytvorte v pythone webový server pomocou frameworku Flask, z nasledovnými "routes"

    - `/` - Vráti obsah súboru `index.html` - použite na to templates
    - `/hello` - Vráti obsah súboru `hello.html`
    - `/secret` - Vráti chybu 403
    
    Do HTML súboru vložte obrázok a HTML oštýlujte pomocou CSS. Všetky statické súbory umiestnite do adresára `static`.

!!! example "Úloha 24.2: Dynamické routovanie"

    Upravte váš server o nasledovné dynamické "routes"

    - `/search` - Načítajte query parameter `query` a zobrazte ho vo výsledon HTML súbore
    - `/trieda/<trieda_id>` - Vráti stránku s informáciami o danej triede
    

## Zhrnutie cvičenia

- [x] URL (Uniform Resource Locator)
    * [ ] webová adresa, ktorú píšeme do prehliadača. Má presne definovanú štruktúru podľa štandardu RFC 3986.
    * [ ] URL určuje kde sa zdroj nachádza a ako sa k nemu dostať
    * [ ] URL má svoju presnä štruktúru a skladá sa z viacerých častí
    * [ ] `schéma://používateľ:heslo@host:port/cesta?query=parametre#fragment`
- [x] Jednotlivé časti URL majú nasledovný význam:
    * [ ] Schéma - Protokol (http, https, ftp, file, ...)
    * [ ] Používateľ - používateľské meno a heslo, tzv. Authority, obe voliteľné
    * [ ] Host - Doména alebo IP adresa
    * [ ] Port - Voliteľný port
    * [ ] Path - Cesta k zdroju
    * [ ] Query string - Parametre požiadavky (key=value páry oddelené znakom &)
    * [ ] Fragment - Umiestnenie (anchor) na stránke (spracováva sa v prehliadači, server ho zvyčajne nedostane)
- [x] Percent kódovanie v URL
    * [ ] Ak chceme v niektorej časti URL uviesť znak, ktorý nie je povolený, musíme ho zakódovať pomocou znaku percenta (tzv. percent encoding). 
    * [ ] V percent encodinu zakódujeme znak pomocou reťazca %XX
    * [ ] Príklady percent kódovania: medzera - `%20`, lomítko `/` - `%2F`, písmeno s diakritikou `é` - `%C3%A9`, percento `%` - `%25`
    * [ ] V Pythone sa na to použíajú funkcie `quote()` a `unquote()` z modulu `urllib.parse`
    * [ ] Pri spracovaní a generovaní URL nesmieme na toto kódovanie zabudnúť, hlavne pri query parametroch a ceste
- [x] Parsovanie URL v Pythone
    * [ ] Python má na prácu s URL vstavaný modul `urllib.parse`
    * [ ] `urlparse()` - rozdelí URL do jednotlivých častí
    * [ ] `parse_qs()` - vytvorí z query stringu slovník s hodnotami
    * [ ] `quote()`/`unquote()` - kódovanie znakov
- [x] Routovanie - proces mapovania HTTP požiadavky na konkrétny zdroj resp. funkcionalitu, prístupnú pomocou HTTP protokolu.
    * [ ] Na identifikáciu cieľa routovania (zdroja) sa používajú údaje z request správy, najmä z hlavičky:
    * [ ] cesta z URL (anglicky URL path)
    * [ ] HTTP metóda (GET, POST)
    * [ ] Názov hostiteľa, hostname (www.example.com)
    * [ ] Dodatočné hodnoty z hlavička (Content-Type, SessionId, ...)
- [x] Druhy routovania
    * [ ] statické, URL cesta musí presne zodpovedať vzoru. `/about` -> `about_handler()`
    * [ ] dynamické, časti URL cesty sú parametrizované. `/users/<id>` -> `user_detail(id)`
- [x] Flask - mikro web framework, ktorý je vhodný pre jednoduché webové aplikácie
    * [ ] `pip install flask`
    * [ ] Jednotlivé "routes" si definujeme pomocou anotácií
    * [ ] Statické súbory ako napr. štýly, javascript a obrázky Flask vie sprístupniť ak ich umiestnite do adresára `static`.
    * [ ] HTML stránky je možné generovať pomocou tzv. templatovania. Samotné šablóny sa umiestňujú do adresára `templates`
    * [ ] Pre vrátenie iných statusových kódov ako 200 OK má flask funkciu `abort()`

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    URL (Uniform Resource Locator) - webová adresa
    
    schéma://používateľ:heslo@host:port/cesta?query=parametre#fragment

    Percent kódovanie v URL - pomocou reťazca %XX
   
    - medzera %20, lomítko / %2F, písmeno é %C3%A9, percento % %25
    - na kódovanie nesmieme zabudnúť hlavne pri query parametroch a ceste

    urllib.parse - parsovanie URL
    - urlparse() - rozdelí URL do jednotlivých častí
    - parse_qs() - vytvorí z query stringu slovník s hodnotami
    - quote()/unquote() - percent kódovanie znakov

    Routovanie - mapovanie požiadavky na konkrétny zdroj
    - cesta z URL (anglicky URL path)
    - HTTP metóda (GET, POST)
    - Názov hostiteľa, hostname (www.example.com)
    - Dodatočné hodnoty z hlavička (Content-Type, SessionId, ...)

    Druhy routovania
    - statické, URL zodpovedá vzoru. /about
    - dynamické, časti cesty sú parametrizované. /users/<id>

    Flask - mikro web framework - pip install flask
    - "routes" pomocou anotácií
    - Statické súbory do adresára static.
    - HTML stránky ako šablóny do adresára templates
    - Iné statusové kódy ako 200 OK pomocou abort()
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Štruktúra, vlastnosti a použitie URL
    - Čo je percent kódovanie, ako funguje
    - Čo je routovanie, ako funguje. Druhy routovania
    - Flask, na čo slúži, základné princípy
