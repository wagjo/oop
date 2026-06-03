# Pokročílí 28: Web Scraping - ochrana

Internet nám umožňuje zdieľať informácie. Prístup k dátam a ich sťahovanie je technicky riešené pomocou HTTP protokolu, ktorý sme preberali na minulých hodinách.

Každý internetový zdroj je identifikovateľný pomocou URL adresy. Ak chce užívateľ daný zdroj (webovú stránku, súbor, obrázok, video) stiahnuť, zašle HTTP požiadavku. Server, ktorý hostuje daný zdroj, mu dáta v HTTP odpovedi (HTTP response) vráti. Toto sťahovanie sa väčšinou vykonáva pomocou webového prehliadača.

## Informácie ako konkurenčná výhoda

Pre väčšinu firiem a organizácií sú dáta jednou z jej najväčších konkurenčných výhod. Zoznam klientov alebo dodávateľov, dáta užívateľov, dokumenty alebo iné informácie sú veľmi cenené. A tak na jednej strane stojí firma, ktorá si chce svoje dáta strážiť a na druhej strane je klient alebo konkurenčná firma, ktorá sa chce k dátam dostať.

V dnešnej dobe je už väčšina dát na webových stránkach prístupná iba po prihlásení, alebo za poplatok. Facebook, LinkedIn, Twitter, všetky tieto stránky boli kedysi otvorené a umožňovali prístup aj neprihláseným užívateľom. Dnešný internet smeruje čoraz viac k uzavretým ekosystémom, tzv. walled garden. Tento proces sa s nástupom AI iba urýchľuje.

Na ochranu a speňaženie dát sa používajú nasledovné prístupy:

- **Právne prístupy** (Ochrana autorských práv, GDPR, DMCA). Tieto prístupy sa zaoberajú hlavne ochranou osobných údajov a nedovoleným zdieľaním cudzích dát.
- Prístup k dátam po zaplatení, tzv. **Paywall**. Túto techniku využívajú napr. vydavateľstvá odborných článkov (Elsevier), alebo internetové denníky (New York Times, HNonline, ...)
- **Zobrazovanie reklamy**. Stránka k dátam síce prístup nezakáže, ale do obsahu pridá reklamy.
- Povolenie prístupu **iba zaregistrovaným a prihláseným** - Pre akýkoľvek prístup k dátam musíte byť prihlásený
- **Časové a objemové limity** - Klienti majú limitovaný počet alebo množstvo dát, ktoré môžu za istý čas stiahnuť alebo zobraziť.
- **Obmedzenie dosahu** (reach) užívateľa - Prístup je obmedzený iba pre určitú podmnožinu dát. To sa využíva hlavne v sociálnych sieťach. Facebook alebo Instagram napr. umožňuje prístup iba ku contentu vašich priateľov (pokiaľ ho sami nedajú zverejniť pre všetkých).

## Automatizované získavanie dát

Na opačnej strane máme ľudí a firmy, ktoré chcú získať čo najviac dát. Niekedy sú dôvody opodstatnené a legálne, niekedy je to šedá zóna, kedy sa síce porušujú pravidlá danej stránky alebo služby, ale trestné to nie je. Niekedy však ide o ilegálnu činnosť, ako napr. firemná spionáž, alebo snaha získať osobné údaje užívateľov.

Ako najvýraznejší príklad legálnosti automatizovaného získavania dát sú vyhľadávače ako napr. Google. Bez toho, aby Google automaticky nesťahoval verejne dostupné stránky a dokumenty, by vyhľadávač nefungoval a o mnohých stránkach by sme nevedeli.

Automatizované získavanie dát z internetu sa nazýva **Web Scraping**. Ide o program - bot - ktorý prechádza web stránky (*web crawling*), spracováva ich a získava URL adresy ďalších stránok alebo súborov. Takto systematicky prejde celú webovú lokalitu, a získa a stiahne veľké množstvo dát, o ktoré ma jeho tvorca záujem.

Takýto web scraping sa zvykne robiť **jednorázovo**, alebo aj **opakovane**, ak chceme získať nové alebo aktualizované dáta.

### Ochrana pred Web Scrapingom

Keďže web scraping vie veľmi rýchlo vygenerovať veľké množstvo požiadaviek, jeho použitie predstavuje pre server značnú záťaž. Zle napísaný web scraping môže niekedy byť na nerozoznanie od cieleného útoku na server. Preto sa servery väčšinou web scrapingu bránia, snažia sa ho rozpoznať a zablokovať. 

Tých 'slušnejších' botov, napr. web crawlerov Google, pustia, a niekedy im poskytnú pravidlá správania v tzv. `robots.txt` súbore. V ňom uvedú zoznam URL ciest, ktoré sú pre prehľadávať zaujímavé a zoznam ciest, ktoré stránka nechce, aby bot prechádzal. Je už na slušnosti daného bota, či to dodrží, alebo nie.

Spôsoby detekcie webového bota:

- Vysoká frekvencia HTTP requestov z jednej IP adresy
- Systematické zasielanie HTTP requestov v poradí, napr. `/page1`, `/page2`, `/page3`, atď.
- Neštandardná HTTP hlavička, nepodobajúca sa na prehliadač
- Podozrivý browser fingerprinting. Nepodporovanie Javascriptu.
- Nachytanie na tzv. Honeypot adresách, na ktoré bežný užívateľ nepríde

Ako najefektívnejšia ochrana proti web scrapingu je často poskytnutie rozhrania API. Pomocou neho si užívatelia budú môcť jednoducho stiahnuť dáta bez potreby scrapovania HTML stránok, a ušetria vám tak serverové zdroje. Tieto API môžete spoplatniť a budete mať tak ďalší zdroj príjmu. (Samozrejme za predpokladu, že takéto sprístupnenie neohrozí váš hlavný biznis.)

Hlavne štátne a verejné inštitúcie, ktoré z princípu musia dáta poskytovať verejne, by mali tieto svoje (naše) dáta poskytovať otvoreným spôsobom, pomocou tzv. konceptu Open Data - otvorených dát.

!!! warning "Dôležité"

    Web scraping je legálny nástroj, ale jeho zneužitie môže mať právne následky. Vždy postupujte eticky a zodpovedne.

!!! example "Úloha 28.1: Ochrana pred scrapingom"

    Úlohy:

    1. Skúste nájsť súbory `robots.txt` pre populárne stránky. Porovnajte ich obsah
    1. Zistiti si váš browser fingerprint na stránke [https://amiunique.org/](https://amiunique.org/) a na stránke [https://coveryourtracks.eff.org/](https://coveryourtracks.eff.org/). Ste jedinečný?
    1. Zistite, aké zákony a predpisy ohľadom Web Scrapingu platia na Slovensku a v EÚ

!!! example "Úloha 28.2: Ochrana pred AI"

    Umelá inteligencia pre svoju prácu potrebuje prehľadávať veľké množstvo stránok. Ide tak isto o automatizované prehľadávanie a získavanie dát, 
    pričom reklamy sa míňaju účinku. Narastajú tak poplatky za hostingové služby a príjmy majiteľov stránok sa nezvyšujú.

    1. Zistite, aké typy ochrany pred AI existujú
    1. Skúste zistiť, čo je Dataset poisoning a aké nástroje existujú
    1. Prezrite si nástroj [https://anubis.techaro.lol/](https://anubis.techaro.lol/) a skúste nájsť informácie o tom, ako funguje.

!!! example "Úloha 28.3: Lokálny scraper"

    Vytvorte scraper, ktorý bude bežať z vášho PC a stiahne aktuality zo školskej stránky, podobne ako sme to robili na minulom cvičení.

    Stiahnite 50 noviniek a vypíšte ich titulky do konzoly.

    - Použite nástroj Crawlee - [https://crawlee.dev/python](https://crawlee.dev/python)
    - Ak to bude potrebné, môžete použiť tiež knižnice Requests a BeautifulSoup

    
!!! example "Úloha 28.4: Scraper pre stránku https://toscrape.com/"

    Vytvorte scraper, ktorý bude sťahovať dáta zo stránky [https://toscrape.com/](https://toscrape.com/)


## Zhrnutie cvičenia

- [x] Informácie ako konkurenčná výhoda
    * [ ] Pre väčšinu firiem a organizácií sú dáta jednou z jej najväčších konkurenčných výhod. 
    * [ ] Zoznam klientov alebo dodávateľov, dáta užívateľov, dokumenty alebo iné informácie sú veľmi cenené.
    * [ ] V dnešnej dobe je už väčšina dát na webových stránkach prístupná iba po prihlásení, alebo za poplatok. 
    * [ ] Dnešný internet smeruje čoraz viac k uzavretým ekosystémom, tzv. walled garden. Tento proces sa s nástupom AI iba urýchľuje.
- [x] Na ochranu a speňaženie dát sa používajú nasledovné prístupy:
    * [ ] Právne prístupy (Ochrana autorských práv, GDPR, DMCA). Tieto prístupy sa zaoberajú hlavne ochranou osobných údajov a nedovoleným zdieľaním cudzích dát.
    * [ ] Prístup k dátam po zaplatení, tzv. Paywall. 
    * [ ] Zobrazovanie reklamy
    * [ ] Povolenie prístupu iba zaregistrovaným a prihláseným
    * [ ] Časové a objemové limity - Klienti majú limitovaný počet alebo množstvo dát, ktoré môžu za istý čas stiahnuť alebo zobraziť.
    * [ ] Obmedzenie dosahu (reach) užívateľa - Prístup je obmedzený iba pre určitú podmnožinu dát. 
- [x] Automatizované získavanie dát
    * [ ] Dôvody na získanie dát sú niekedy opodstatnené a legálne, inokedy je to šedá zóna, kedy sa síce porušujú pravidlá danej stránky alebo služby, ale trestné to nie je
    * [ ] Niekedy však ide o ilegálnu činnosť, ako napr. firemná spionáž, alebo snaha získať osobné údaje užívateľov.
    * [ ] Web crawler - program, ktorý prechádza web stránky a získava URL adresy
    * [ ] Web scraping - Systematicky prechádza celú webovú lokalitu a získa a stiahne veľké množstvo dát, o ktoré ma jeho tvorca záujem.
    * [ ] Zvykne sa robiť jednorázovo, alebo aj opakovane, ak chceme získať nové alebo aktualizované dáta.
- [x] Ochrana pred Web Scrapingom
    * [ ] Použitie scrapera predstavuje pre server značnú záťaž. 
    * [ ] Zle napísaný web scraping môže niekedy byť na nerozoznanie od cieleného útoku na server. 
    * [ ] Aj preto sa servery väčšinou web scrapingu bránia, snažia sa ho rozpoznať a zablokovať.
    * [ ] Tých 'slušnejších' botov, napr. web crawlerov Google, pustia, a niekedy im poskytnú pravidlá správania v tzv. robots.txt súbore. 
    * [ ] V ňom uvedú zoznam URL ciest, ktoré sú pre prehľadávať zaujímavé a zoznam ciest, ktoré stránka nechce, aby bot prechádzal.
- [x] Spôsoby detekcie webového bota
    * [ ] Vysoká frekvencia HTTP requestov z jednej IP adresy
    * [ ] Systematické zasielanie HTTP requestov v poradí, napr. `/page1`, `/page2`, `/page3`, atď.
    * [ ] Neštandardná HTTP hlavička, nepodobajúca sa na prehliadač
    * [ ] Podozrivý browser fingerprinting. Nepodporovanie Javascriptu.
    * [ ] Nachytanie na tzv. Honeypot adresách, na ktoré bežný užívateľ nepríde
