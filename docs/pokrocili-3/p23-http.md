# Pokročílí 23: HTTP

Minule sme si ukazovali, ako rôzne vieme nad protokolom TCP vytvoriť vlastný aplikačný protokol, ktorý rozdelí TCP stream na správy. Dnes si predstavíme najpoužívanejší webový protokol, HTTP.

!!! warning

    Protokol HTTP prešiel dlhou históriou a obsahuje v sebe veľa vylepšení, ktoré ho robia rýchlym a efektívnym. My si dnes pre jednoduchosť predstavíme iba jeho základnú verziu.

## HTTP

HTTP (HyperText Transfer Protocol) je protokol na aplikačnej vrstve. Primárnym **cieľom HTTP protokolu je umožnenie prenosu tzv. hypertextových dokumentov** a iných súborov cez internet. Tento protokol tvorí základ pre dnešný web a nad ním postavené technológie.

Ak si v prehliadači otvoríme nejakú webovú stránku, prehliadač sa správa ako HTTP client, a cez TCP spojenie posiela požiadavky (requests) na daný HTTP server, ktorý mu odpovedá a posiela späť HTML dokumenty, obrázky alebo iné súbory.

Vlastnosti HTTP:

- **textový protokol**: údaje posielané po TCP sú zakódované vo forme reťazcov, preto je jednoducho čitateľný aj ľuďom
- **request-response** model: funguje na základe princípu pošli požiadavku (request) a dostaneš odpoveď (response)
- **client-server** komunikácia: spojenie 1-1, kde klient pošle žiadosť a server odpovedá
- **bezstavový (stateless)**: medzi dvoma požiadavkami (requestami) neexistuje spojitosť. Každý request je samostatný a nezávislý

!!! info

    HTTP neposkytuje žiadnu ochranu proti 'špehovaniu', preto nie je vhodný pre súkromné informácie ako heslá, bankové údaje a pod. Väčšina dnešného webu je dnes už šifrovaná pomocou HTTPS (HTTP + TLS/SSL) - prehliadače označujú obyčajné HTTP ako „Not secure“.

Predvolený port pre HTTP je `80` a pre HTTPS `443`

## História HTTP

![OG web developer](../assets/tim.jpg){align=right width=400}

HTTP vznikol v roku 1991. Webové technológie vrátane HTTP vymyslel **Tim Berners-Lee** v roku 1989 v CERN-e (pôvodne na zdieľanie vedeckých dokumentov). 

Okrem HTTP Berners-Lee vymyslel aj HTML (Hypertext Transfer Protocol) a URL (Uniform Resource Locator).

Počas jeho histórie bolo vydaných niekoľko verzií HTTP protokolu:

- HTTP/0.9 (1991) mal iba jeden riadok: GET /index.html a server poslal HTML (žiadne hlavičky).
- HTTP/1.0 (1996) viac funkcionalít, každý request mal svoje vlastné TCP spojenie
- **HTTP/1.1** (1997) prvý HTTP štandard, tak ako ho poznáme dnes
- **HTTP/2** (2015) je binárny (nie textový) - rýchlejší, podporuje multiplexovanie (viac requestov naraz na jednom TCP spojeni).
- **HTTP/3** (2022) už nepoužíva TCP, ale QUIC (cez UDP) - rieši problém „head-of-line blocking“.

Dnes sa používajú iba posledné 3 verzie (1.1, 2, 3). My si budeme vysvetľovať základné funkcionality verzie 1.1.

## Štruktúra HTTP komunikácie

HTTP komunikuje na základe správ, ktoré sú buď request (požiadavka od klienta) alebo response (odpoveď servera). 

Na oddelenie jednotlivých častí v správe sa používa reťazec `CRLF` (Carriage Return + Line Feed), v Pythone `"\r\n"`. 

Request správa sa skladá z 3 častí:

1. **Request riadok** - obsahuje **HTTP metódu**, cestu a HTTP verziu
1. **Hlavička** - nastavenia a parametre, viacero `attribút: hodnota` párov, oddelených `CRLF`
1. **Telo požiadavky** - len niekedy

=== "Príklad HTTP Request správy"

    ```http
    GET / HTTP/1.1
    Host: www.example.com
    User-Agent: curl/8.7.1
    Connection: close
    Accept: */*
    ```

Response správa sa skladá tiež z 3 častí:

1. **Statusový riadok** - HTTP verzia, **HTTP kód** a textový popis kódu
1. **Hlavička** - podobne ako v requeste, obsahuje parametre a nastavenia
1. **Telo požiadavky** - len niekedy, obsahuje samotný dokument alebo iné dáta

=== "Príklad HTTP Response správy obsahujúcej telo"

    ```http
    HTTP/1.1 200 OK
    Date: Thu, 02 Apr 2026 12:23:14 GMT
    Content-Type: text/html
    Connection: close
    Server: cloudflare
    Last-Modified: Tue, 24 Mar 2026 22:06:31 GMT
    Allow: GET, HEAD
    Accept-Ranges: bytes
    Age: 7106
    cf-cache-status: HIT
    CF-RAY: 9e5fcdba6e7984ce-VIE

    <!doctype html><html><head><title>Example</title></head><body>...</body></html>
    ```

### HTTP Metódy

Klient vie poslať rôzne typy požiadaviek, podľa toho, čo potrebuje. Tieto rôzne druhy správ sa nazývajú **HTTP metódy**. Názov metódy sa musí vždy poslať v prvom riadku požiadavky. Medzi základné HTTP metódy patria:

- **GET**: Klient chce len čítať data zo serveru, nič nezapisuje
- **POST**: Klient chce poslať dáta serveru, napr. formulár s údajmi
- **PUT**: Klient chce odoslať nový alebo aktualizovaný súbor alebo dáta
- **DELETE**: Klient požaduje odstrániť dáta alebo súbor na serveri

Okrem týchto metód existujú aj ďalšie (napr. `HEAD`, `OPTIONS`, `PATCH`) pre pokročilejšiu prácu so serverom.

### HTTP Statusové Kódy

Server na základe požiadavky klienta odošle naspať odpoveď - response. Nie vždy sa podarí splniť požiadavku klienta, preto má **odpoveď v sebe tzv. statusový kód**, ktorý informuje klienta o tom, čo server vykonal. 

HTTP statusové kódy sa skladajú z troch číslic a delíme ich do nasledujúcich skupín:

- **1xx Informovanie** - bežne nepoužívané
- **2xx Úspech** - napr. `200 OK`, najbežnejší typ odpovede
- **3xx Presmerovanie** - napr. `301 Moved Permanently` pri zmene adresy alebo názvu súboru
- **4xx Chyba klienta** - napr. `404 Not Found` ak sa dokument nenašiel, `403 Forbidden` ak klient nie je prihlásený, atď.
- **5xx Chyba servera** - napr. `503 Service Unavailable` pri poruche servera, `504 Gateway Timeout` pri preťažení, atď

## Úlohy

!!! example "Úloha 23.1: HTTP klient"

    Vytvorte jednoduchého HTTP klienta v Pythone, ktorý bude robiť nasledovné:

    - pripojí sa na server pomocou TCP
    - odošle HTTP request
    - načíta HTTP response
    - vypíše odpoveď do konzoly

    Pri poslaní v hlavičke nezabudnite uviesť `Connection: close`, aby po poslaní odpovede server zatvoril spojenie. Uľahčí to načítavanie odpovede.

    Pripojte sa na nasledovné servery:

    - www.example.com, cesta /, port 80
    - www.faqs.org, cesta /faqs/, port 80
    - www.faqs.org, cesta /, port 80

    Vyskúšajte iné servery (školskú stránku?) alebo cesty a analyzujte odpovede serverov
    ```

!!! example "Úloha 23.2: HTTP server"

    Vytvorte jednoduchý HTTP server v Pythone, ktorý bude robiť nasledovné :

    - bude akceptovať TCP spojenia
    - prijme HTTP request
    - vypíše request do konzoly
    - odošle HTTP response 200 s nejakým HTML obsahom
    - ukončí spojenie

    Po spustení servera sa na neho pripojte pomocou svojho prehliadača, napr. [localhost:8080](http://localhost:8080)

    Vyskúšajte iné druhy odpovede a tela odpovede, napr. 404, 403, 504, 303, atď. Čo urobí prehliadač?
    ```

## Zhrnutie cvičenia

- [x] HTTP (HyperText Transfer Protocol) - webový protokol na prenos dokumentov
    * [ ] textový protokol: údaje posielané po TCP sú zakódované vo forme reťazcov, preto je jednoducho čitateľný aj ľuďom
    * [ ] request-response model: funguje na základe princípu pošli požiadavku (request) a dostaneš odpoveď (response)
    * [ ] client-server komunikácia: spojenie 1-1, kde klient pošle žiadosť a server odpovedá
    * [ ] bezstavový (stateless): medzi dvoma požiadavkami (requestami) neexistuje spojitosť. Každý request je samostatný a nezávislý
- [x] História HTTP
    * [ ] Vytvoril ho v roku 1991 Tim Berners-Lee, OG Web Developer
    * [ ] HTTP/0.9 (1991) mal iba jeden riadok: GET /index.html a server poslal HTML (žiadne hlavičky).
    * [ ] HTTP/1.0 (1996) viac funkcionalít, každý request mal svoje vlastné TCP spojenie
    * [ ] HTTP/1.1 (1997) prvý HTTP štandard, tak ako ho poznáme dnes
    * [ ] HTTP/2 (2015) je binárny (nie textový) - rýchlejší, podporuje multiplexovanie (viac requestov naraz na jednom TCP spojeni).
    * [ ] HTTP/3 (2022) už nepoužíva TCP, ale QUIC (cez UDP) - rieši problém „head-of-line blocking“.
- [x] Štruktúra HTTP komunikácie
    * [ ] HTTP komunikuje na základe správ, ktoré sú buď request (požiadavka od klienta) alebo response (odpoveď servera).
    * [ ] Na oddelenie jednotlivých častí v správe sa používa reťazec CRLF (Carriage Return + Line Feed), v Pythone "\r\n". 
- [x] Request správa sa skladá z 3 častí:
    * [ ] Request riadok - obsahuje HTTP metódu, cestu a HTTP verziu
    * [ ] Hlavička - nastavenia a parametre, viacero attribút: hodnota párov, oddelených CRLF
    * [ ] Telo požiadavky - len niekedy
- [x] Response správa sa skladá tiež z 3 častí:
    * [ ] Statusový riadok - HTTP verzia, HTTP kód a textový popis kódu
    * [ ] Hlavička - podobne ako v requeste, obsahuje parametre a nastavenia
    * [ ] Telo požiadavky - len niekedy, obsahuje samotný dokument alebo iné dáta
- [x] HTTP Metódy
    * [ ] Názov metódy sa musí vždy poslať v prvom riadku požiadavky. Definuje typ požiadavky
    * [ ] GET: Klient chce len čítať data zo serveru, nič nezapisuje
    * [ ] POST: Klient chce poslať dáta serveru, napr. formulár s údajmi
    * [ ] PUT: Klient chce odoslať nový alebo aktualizovaný súbor alebo dáta
    * [ ] DELETE: Klient požaduje odstrániť dáta alebo súbor na serveri
- [x] HTTP Statusové Kódy
    * [ ] Odpoveď v sebe tzv. statusový kód, ktorý informuje klienta o tom, čo server vykonal. 
    * [ ] 1xx Informovanie - bežne nepoužívané
    * [ ] 2xx Úspech - napr. 200 OK, najbežnejší typ odpovede
    * [ ] 3xx Presmerovanie - napr. 301 Moved Permanently pri zmene adresy alebo názvu súboru
    * [ ] 4xx Chyba klienta - napr. 404 Not Found ak sa dokument nenašiel, 403 Forbidden ak klient nie je prihlásený, atď.
    * [ ] 5xx Chyba servera - napr. 503 Service Unavailable pri poruche servera, 504 Gateway Timeout pri preťažení, atď



!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    HTTP - HyperText Transfer Protocol
    - textový protokol: správy vo forme reťazcov, ľahko čitateľné
    - request-response model: pošli požiadavku a dostaneš odpoveď
    - client-server komunikácia: spojenie 1-1, klient žiada a server odpovedá
    - bezstavový (stateless): každý request je samostatný a nezávislý, neexistuje spojitosť
    
    Autor HTTP je Tim Berners-Lee, tvorca webu
    - HTTP/1.1 (1997) prvý HTTP štandard, stále používaný
    - HTTP/2 (2015) binárny (nie textový)
    - HTTP/3 (2022) používa QUIC (cez UDP), nie TCP

    Používa oddeľovací reťazec CRLF (Carriage Return + Line Feed), v Pythone "\r\n". 

    Request správa:
    - Request riadok - HTTP metóda, cesta a HTTP verzia
    - Hlavička - páry attribút: hodnota, oddelené pomocou CRLF
    - Telo požiadavky - len niekedy

    Response správa:
    - Statusový riadok - HTTP verzia, HTTP kód a textový popis kódu
    - Hlavička - podobne ako v requeste
    - Telo požiadavky - len niekedy, obsahuje samotné dáta

    HTTP Metóda - typ požiadavky
    - GET: len čítať data zo serveru
    - POST: poslať dáta serveru, napr. formulár s údajmi
    - PUT: odoslať nový alebo aktualizovaný súbor alebo dáta
    - DELETE: odstrániť dáta alebo súbor na serveri

    HTTP Statusové Kódy - 3 ciferné číslo, udáva čo server vykonal
    - 1xx Informovanie - bežne nepoužívané
    - 2xx Úspech - napr. 200 OK, najčastejší typ odpovede
    - 3xx Presmerovanie - napr. 301 Moved Permanently pri zmene adresy alebo názvu súboru
    - 4xx Chyba klienta - napr. 404 Not Found, 403 Forbidden
    - 5xx Chyba servera - napr. 503 Service Unavailable, 504 Gateway Timeout
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Na čo slúži protokol HTTP
    - Vlastnosti HTTP
    - Formát Request správy
    - Formát Response správy
    - HTTP Metódy - na čo slúžia, uveďte najpoužívanejšie
    - HTTP Status Kódy - na čo slúžia, uveďte najpoužívanejšie
