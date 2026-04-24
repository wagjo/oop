# Pokročílí 25: Webové API

Softvérová aplikácia poskytuje svoje funkcionality rôznymi spôsobmi. Najčastejšie má aplikácia nejaké užívateľské rozhranie (UI - User Interface), pomocou ktorého vie človek - užívateľ, používať danú aplikáciu.

Často chceme, aby náš softvér mohli využívať nie len ľudia, ale aj iné počítačové programy. Rozhranie, ktoré umožňuje jednej aplikácii využívať funkcie alebo dáta inej aplikácie, sa nazýva API - Aplication Programming Interface.

**API (Application Programming Interface – aplikačné programovacie rozhranie) je súbor pravidiel, protokolov a nástrojov, ktoré umožňujú dvom rôznym softvérovým aplikáciám spolu komunikovať a vymieňať si dáta.**

Poznáme viacero druhov API:

- *knižničné API* - súbor funkcií a dátových štruktúr, ktorými používame softvérovú knižnicu v našom programe
- *sieťové API* - sada definovaných pravidiel a protokolov pre komunikáciu medzi aplikáciami po sieti
- *webové API* - typ sieťového API, ktoré je postavené nad webovými protokolmi, hlavne nad protokolom HTTP

## Webové API

Webové API (Application Programming Interface) je rozhranie, ktoré umožňuje rôznym aplikáciám a systémom komunikovať cez internet pomocou protokolov ako HTTP. Slúži ako most, ktorý dovoľuje jednej aplikácii vyžiadať si dáta alebo funkcie od inej (napr. mobilná aplikácia získavajúca počasie), pričom sa používajú formáty ako JSON či XML. 

Príklady použitia:

- Integrácia služieb: Prihlásenie cez Google/Facebook, platobné brány (PayPal, Stripe) alebo integrácia máp (Google Maps) do vlastného webu
- Automatizácia a dáta: Automatické nahrávanie fotiek do cloudu, získavanie predpovede počasia alebo aktualizácia skladových zásob.
- Komunikácia systémov: Mobilné aplikácie alebo webové stránky si sťahujú dáta zo servera pomocou AJAX dotazov.

Výhodou webových API je, že vývojári nemusia poznať internú štruktúru cudzieho systému, stačí im poznať pravidlá komunikácie cez API.

Webové API zvyknú používať protokol HTTP a komunikáciu vo forme request-response. API sú zverejnené na presne definovanej URL webovej adrese, ktorá sa volá **endpoint**. Jednotlivé funkcionality sú potom dostupné na rôznych cestách na tejto URL adrese. Dáta, ktoré sú prenášané medzi aplikáciami sú zakódované väčšinou pomocou štandardizovaných formátov ako napr. JSON, XML alebo CSV.

Webové API vieme ďalej rozdeliť do rôznych typov. V súčastnosti najpoužívanejší typ je REST API.

## REST API

REST je populárny druh webového API, ktoré má jednoduchú štruktúru, založenú na prístupe k dátovým zdrojom (web resource). 

Používa HTTP metódy GET, POST, PUT a DELETE, a jednotlivé URL webové adresy sú orientované na zdroje a dáta, s ktorými chceme pracovať. REST API je podobne ako HTTP bezstavové a ako dátový formát používa väčšinou JSON.

URL endpoint k REST API je väčšinou verzionovaný, aby sa zaručila spätná kompatibilita. (napr. `https://api.example.com/v1/`)

Príklad REST API:

- `GET /users` - vráti zoznam všetkých užívateľov
- `GET /users/1` - vráti informácie o užívateľovi s ID 1
- `POST /users/1` - zmení vlastnosti užívateľa s ID 1

REST API sa dá jednoducho vytvoriť aj v bežnom webovom frameworku pomocou routovania a JSON knižnice. Ako ukážku si uvedieme API v Pythone pomocou Flask:

=== "Web API vo Flasku"

    ```python
    from flask import Flask, jsonify, request

    app = Flask(__name__)

    # Fake databáza (v reálnom projekte by bola SQL/NoSQL)
    books = [
        {"id": 1, "title": "1984", "author": "George Orwell"},
        {"id": 2, "title": "Duna", "author": "Frank Herbert"}
    ]

    # 1. GET – vráti všetky knihy
    @app.route('/api/books', methods=['GET'])
    def get_books():
        return jsonify(books), 200

    # 2. GET – vráti jednu knihu podľa ID
    @app.route('/api/books/<int:book_id>', methods=['GET'])
    def get_book(book_id):
        book = next((b for b in books if b["id"] == book_id), None)
        if book:
            return jsonify(book), 200
        return jsonify({"error": "Kniha nenájdená"}), 404

    # 3. POST – pridá novú knihu
    @app.route('/api/books', methods=['POST'])
    def add_book():
        data = request.get_json()
        if not data or not data.get("title") or not data.get("author"):
            return jsonify({"error": "Chýbajú title alebo author"}), 400
        
        new_book = {
            "id": max(b["id"] for b in books) + 1,
            "title": data["title"],
            "author": data["author"]
        }
        books.append(new_book)
        return jsonify(new_book), 201

    # 4. DELETE – vymaže knihu
    @app.route('/api/books/<int:book_id>', methods=['DELETE'])
    def delete_book(book_id):
        global books
        books = [b for b in books if b["id"] != book_id]
        return jsonify({"message": "Kniha bola vymazaná"}), 200

    if __name__ == '__main__':
        app.run(debug=True, port=5000)
    ```

## Prístup k webovému API 

Na rozdiel od webových stránok si webové API nevieme jednoducho "otvoriť" alebo prezrieť v prehliadači. Našťastie však máme množstvo nástrojov, pomocou ktorých si vieme API vyskúšať. 

!!! info "Web API dokumentácia"

    Tak ako aj softvérové knižnice aj webové API by mali mať kvalitnú dokumentáciu. Tá je zvyčajne umiestnená na samostatnej webovej adrese. 
    
    Pre tvorbu webových API existuje veľké množstvo nástrojov na tvorbu dokumentácie ako napr. Swagger, Scalar alebo Stoplight. Väčšinou však ide o komerčné nástroje. 

Na jednoduché otestovanie a vyskúšanie webových API budeme používať open source nástroj [https://hoppscotch.io/](https://hoppscotch.io/)

V praxi ku webovému API pristupuje iný program a nie človek. Nakoľko ide o webový protokol, na komunikáciu vieme použiť bežného HTTP klienta. Uvedieme si jednoduchú ukážku použitia API vytvoreného v predchádzajúcom príklade.

=== "Volanie webového API"

    ```python
    import requests

    BASE_URL = "http://127.0.0.1:5000/api"

    def main():
        # GET všetky
        r = requests.get(f"{BASE_URL}/books")
        print("Všetky knihy:", r.json())

        # POST nová
        r = requests.post(f"{BASE_URL}/books", json={
            "title": "Pán prsteňov",
            "author": "J.R.R. Tolkien"
        })
        print("Vytvorená:", r.json())

        # DELETE
        r = requests.delete(f"{BASE_URL}/books/1")
        print("Delete status:", r.status_code)

    if __name__ == "__main__":
        main()
    ```

Na vytvorenie a odoslanie HTTP requestov vieme využiť tiež nástroj `curl`. Ide o populárny a veľmi silný nástroj, pomocou ktorého vieme v príkazovom riadku vytvoriť a odoslať akýkoľvek HTTP request.

Príklad vytvorenia a odoslania HTTP requestov: 

```curl
curl -X GET http://127.0.0.1:5000/api/books/1 | jq

curl -X POST http://127.0.0.1:5000/api/books \
     -H "Content-Type: application/json" \
     -d '{
           "title": "Hobit",
           "author": "J.R.R. Tolkien"
         }'

curl -X DELETE http://127.0.0.1:5000/api/books/1
```

## Voľne dostupné API

Tak ako máme obrovské množstvo voľne dostupných webových stránok, tak existuje aj veľa voľne dostupných webových API, ktoré môžeme využívať vo svojich programoch.

Celkom dobrý zoznam takýchto API nájdete na stránkach [free-apis.github.io](https://free-apis.github.io/#/), [www.freepublicapis.com](https://www.freepublicapis.com/) alebo [publicapis.dev](https://publicapis.dev/)

Aby sa predišlo príliš častému volaniu API, pri väčšine z nich sa musíte zaregistrovať a pri volaní používať autentifikačný token. Dnes si však ukážeme niekoľko verejných API, ktoré sú k dispozícii aj bez registrácie.

### Kurz mien

Pre zistenie aktuálneho alebo historického kurzu svetových mien vieme použiť api na stránke [https://frankfurter.dev/](https://frankfurter.dev/)

- Aktuálny kurz EUR/USD a EUR/GBP: [`https://api.frankfurter.dev/v2/rates?base=EUR&quotes=USD,GBP`](https://api.frankfurter.dev/v2/rates?base=EUR&quotes=USD,GBP)
- Historický kurz EUR/USD od začiatku roka 2026: [`https://api.frankfurter.dev/v2/rates?from=2026-01-01&base=EUR&quotes=USD`](https://api.frankfurter.dev/v2/rates?from=2026-01-01&base=EUR&quotes=USD)

### Predpoveď počasia

Stránka [open-meteo.com](https://open-meteo.com) ponúka API na získanie predpovede počasia pre akékoľvek miesto na Zemi.

- Získanie polohy podľa názvu mesta: [https://open-meteo.com/en/docs/geocoding-api](https://open-meteo.com/en/docs/geocoding-api) - napr. [`https://geocoding-api.open-meteo.com/v1/search?name=Berlin&count=10&language=en&format=json`](https://geocoding-api.open-meteo.com/v1/search?name=Berlin&count=10&language=en&format=json)
- Získanie predpovede počasie pre danú polohu: [https://open-meteo.com/en/docs](https://open-meteo.com/en/docs) - napr. [`https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m&past_days=0&forecast_days=7`](https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m&past_days=0&forecast_days=7)

### Klinické štúdie

Americká vláda povinne zverejňuje dáta o všetkých medicínskych štúdiách v USA na stránke [ClinicalTrials.gov](https://clinicaltrials.gov/). K týmto štúdiám je možné pristupovať aj pomocou webového API. Dokumentácia je k dispozícii na stránke [https://clinicaltrials.gov/data-api/api#extapi](https://clinicaltrials.gov/data-api/api#extapi)

- Nájdenie ID klinických štúdií, ktoré sa venujú určitej chorobe alebo syndrómu: [`https://clinicaltrials.gov/api/v2/studies?query.cond=lung+cancer&sort=%40relevance`](https://clinicaltrials.gov/api/v2/studies?query.cond=lung+cancer&sort=%40relevance)
- Získanie detailov o konkrétnej štúdii na základe jej ID: [`https://clinicaltrials.gov/api/v2/studies/NCT05817110`](https://clinicaltrials.gov/api/v2/studies/NCT05817110)

## Úlohy

!!! example "Úloha 25.1: Voľne dostupné API"

    Prejdite si voľne 3 dostupné API uvedené v predchádzajúcej kapitole.

    - Prehliadnite si dokumentácie k jednotlivým API
    - Vyskúšajte vzorové requesty v prehliadači
    - Vyskúšajte vzorové requesty v nástroji [https://hoppscotch.io/](https://hoppscotch.io/)
    - Vytvorte vlastné requesty podľa dokumentácie

!!! example "Úloha 25.2: Webový server pomocou Flask"

    Vytvorte v Pythone webový server pomocou frameworku Flask, z nasledovnými routes a API endpointami:

    - `GET /` - Hlavná HTML stránka, ktorá vypíše zoznam všetkých uložených kníh
    - `GET /api/books` - Vráti JSON obsah so zoznamom všetkých uložených kníh
    - `POST /api/books/<id>` - Vloží do zoznamu novú knihu alebo upraví hodnoty existujúcej knihy
    - `DELETE /api/books/<id>` - Vymaže zo zoznamu knihu s daným ID
    
    Pri vytváraní novej knihy posielajte JSON objekt s parametrami `title`, `author` a `year`

    Vyskúšajte vaše endpointy pomocou webového prehliadača a pomocou nástroja [https://hoppscotch.io/](https://hoppscotch.io/)

!!! example "Úloha 25.3: Webová stránka s kurzom mien"

    Vytvorte v Pythone webovú aplikáciu pomocou frameworku Flask, ktorá bude ponúkať webovú stránku s kurzami svetových mien.

    Vaša webová aplikácia nech ponúkne html stránku, na ktorej sa bude zobrazovať 5 vybraných kurzov mien. Užívateľ bude mať možnosť zmeniť hlavnú menu, voči ktorej sa budú počítať kurzy.

    Na zistenie výmenného kurzu použite API služby [https://frankfurter.dev/](https://frankfurter.dev/)

    

## Zhrnutie cvičenia

- [x] API (Application Programming Interface) je súbor pravidiel, protokolov a nástrojov, ktoré umožňujú dvom rôznym softvérovým aplikáciám spolu komunikovať a vymieňať si dáta.
    * [ ] knižničné API - súbor funkcií a dátových štruktúr, ktorými používame softvérovú knižnicu v našom programe
    * [ ] sieťové API - sada definovaných pravidiel a protokolov pre komunikáciu medzi aplikáciami po sieti
    * [ ] webové API - typ sieťového API, ktoré je postavené nad webovými protokolmi, hlavne nad protokolom HTTP
- [x] Webové API
    * [ ] Umožňuje rôznym aplikáciám a systémom komunikovať cez internet pomocou protokolov ako HTTP
    * [ ] API sú zverejnené na presne definovanej URL webovej adrese, ktorá sa volá endpoint. 
    * [ ] Jednotlivé funkcionality sú potom dostupné na rôznych cestách na tejto URL adrese. 
    * [ ] Dáta, ktoré sú prenášané medzi aplikáciami sú zakódované väčšinou pomocou štandardizovaných formátov ako napr. JSON, XML alebo CSV.
- [x] REST API
    * [ ] REST je populárny druh webového API, ktoré má jednoduchú štruktúru, založenú na prístupe k dátovým zdrojom (web resource).
    * [ ] Používa HTTP metódy GET, POST, PUT a DELETE
    * [ ] Jednotlivé URL webové adresy sú orientované na zdroje a dáta, s ktorými chceme pracovať. 
    * [ ] REST API je podobne ako HTTP bezstavové a ako dátový formát používa väčšinou JSON.
    * [ ] URL endpoint k REST API je väčšinou verzionovaný, aby sa zaručila spätná kompatibilita. (napr. https://api.example.com/v1/)
- [x] Prístup k webovému API
    * [ ] Pomocou webového prehliadača
    * [ ] CLI nástroj curl
    * [ ] Open source nástroj https://hoppscotch.io/
- [x] Voľne dostupné API
    * [ ] free-apis.github.io, www.freepublicapis.com, publicapis.dev
    * [ ] Pre zistenie aktuálneho alebo historického kurzu svetových mien vieme použiť api na stránke https://frankfurter.dev/
    * [ ] Stránka open-meteo.com ponúka API na získanie predpovede počasia pre akékoľvek miesto na Zemi.
    * [ ] Informácie o klinických štúdiách získame pomocou API na stránke https://clinicaltrials.gov/data-api/api#extapi

!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    API - súbor pravidiel, protokolov a nástrojov, aby mohli aplikácie navzájom komunikovať
    
    - knižničné API - softvérové knižnice
    - sieťové API - komunikácia medzi aplikáciami po sieti
    - webové API - sieťové API postavené nad webovými protokolmi

    REST API
    
    Má jednoduchú štruktúru, založenú na prístupe k dátovým zdrojom (web resource).
    HTTP metódy GET, POST, PUT a DELETE
    URL webové adresy sú orientované na zdroje a dáta, s ktorými chceme pracovať. 
    URL endpoint je väčšinou verzionovaný, napr. https://api.example.com/v1/

    Prístup k webovému API

    - Pomocou webového prehliadača
    - CLI nástroj curl
    - Open source nástroj https://hoppscotch.io/

    Voľne dostupné API - free-apis.github.io, www.freepublicapis.com, publicapis.dev
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je a na čo slúži API
    - Aké druhy API poznáme
    - Vlastnosti webových API a REST API
    - Ako vieme k webovým API pristupovať
