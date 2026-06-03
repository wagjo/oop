# Pokročílí 27: Web Scraping

Web scraping je názov pre programy, ktoré systematicky prehľadávajú webovú lokalitu a sťahujú z nej dáta. Viac o problematike web scrapovania si povieme nabudúce, dnes si vyskúšame web scraper naprogramovať.

## Princíp fungovania scrapera

Scraper začína na štartovacej URL adrese:

1. Pošle HTTP požiadavku a stiahne HTML súbor so stránkou.
1. Zparsuje a zanalyzuje HTML kód stránky a nájde URL odkazy na ďalšie stránky alebo súbory
1. Stiahne súbory, o ktoré máme záujem
1. Z nájdených URL odkazov vyberie tie, o ktoré máme záujem a pokračuje bodom 1

Aby scraper nepokračoval do nekonečna, musí si:

- pamätať už navštívené stránky
- ukončiť scrapovanie po určitom počte stránok, resp. súborov
- ukončiť po určitom čase

Pri pokročilom scrapovaní nestačí iba stiahnuť HTML súbor, ale scraper musí vykonať aj Javascript kód na stránke a tváriť sa, že je regulérny prehliadač.

## Nástroje pre Web Scraping

Pre tvorbu web scraperov sa vo veľkej miere používa Python a Javascript. Samotní boti sú často stredne veľké skripty v týchto jazykoch, ktoré necháte bežať buď lokálne na vašom počítači, alebo na prenajatom serveri.

Na uľahčenie tvorby web scraperov existuje veľké množstvo nástrojov a knižníc. Medzi najpopulárnejšie patria:

Python:

- Requests — Jednoduchá a najpoužívanejšia knižnica na odosielanie HTTP požiadaviek.
- Crawlee - Open-source crawling framework (JavaScript + Python) - moderný nástroj na tvorbu robustných scraprov
- BeautifulSoup — Knižnica na parsovanie HTML a XML a jednoduchú extrakciu dát.
- Scrapy — Plnohodnotný framework na veľké a škálovateľné crawling projekty.
- Playwright — Moderná knižnica na automatizáciu prehliadača pri dynamických stránkach.
- Selenium — Klasická knižnica na ovládanie reálneho prehliadača (stále používaná).

Javascript (node.js):

- Axios — Ľahká a populárna knižnica na vykonávanie HTTP požiadaviek.
- Cheerio — Rýchly HTML parser s jQuery-like API pre Node.js.
- Playwright — Najmodernejšia knižnica na scrapovanie dynamických a JavaScript-heavy stránok.
- Puppeteer — Veľmi populárna knižnica od Google na ovládanie Chrome prehliadača.


Etika a právne aspekty:

- Zbierajte **len verejné dáta**
- Rešpektujte **Terms of Service** stránok
- Pri osobných údajoch dodržiavajte **GDPR**
- Nevytvárajte DDoS útoky – scraping nie je útok
- Buďte transparentní (v User-Agente môžete uviesť, že ste scraper)

Pre uľahčenie programovania web scraperov existuje tiež stránka, na ktorej si môžete reálne vyskúšať rôzne typy scrapovania. Ide o stránku [https://toscrape.com/](https://toscrape.com/)

!!! warning "Dôležité"

    Web scraping je legálny nástroj, ale jeho zneužitie môže mať právne následky. Vždy postupujte eticky a zodpovedne. Viac o tom nabudúce.

## Platforma Apify

**[Apify](https://www.apify.com)** je full-stack cloudová platforma špecializovaná na **web scraping**, browser automation a extrakciu dát z webu.

Poskytuje všetko potrebné na profesionálny zber dát - od tisícov hotových nástrojov až po komplexnú infraštruktúru a nástroje na vývoj vlastných riešení.

Viete si v nej vytvoriť vlastný scraper, alebo vybrať z 34 000+ hotových *Actors* - hotové scrapery pre Instagram, TikTok, Google Maps, Amazon, LinkedIn a stovky ďalších stránok.

=== "Základné koncepty"

| Pojem              | Popis                                                                 | Praktické použitie                          |
|--------------------|-----------------------------------------------------------------------|---------------------------------------------|
| **Actor**          | Serverless skripty - boti na scraping alebo automatizáciu        | Instagram Scraper, vlastný e-shop crawler   |
| **Task**           | Uložená konfigurácia Actora s konkrétnymi vstupnými parametrami      | „Scrapuj denne produkty z Alza.sk“          |
| **Run**            | Jedno konkrétne spustenie Actora alebo Tasku                         | Spustenie Tasku každý deň o 8:00            |
| **Dataset**        | Hlavné úložisko výsledkov (štruktúrované dáta)                       | Export do CSV / JSON / Excel / Pandas       |
| **Key-value store**| Úložisko pre súbory, screenshoty, JSON objekty                       | Uloženie PDF faktúr alebo obrázkov          |
| **Proxy**          | Automatická rotácia IP adries (datacenter + residential)             | Zabrániť blokovaniu pri veľkom scrapingu    |
| **Schedule**       | Automatické spúšťanie Taskov podľa časového plánu (cron)             | Denný / hodinový zber dát                   |

### Ako začať

1. Zaregistrujte sa na **[apify.com](https://www.apify.com)** (free tier s $5 kreditmi mesačne)
1. Prejdite na [console.apify.com](https://console.apify.com)
1. Kliknite na **Store** v ľavom menu
1. Vyhľadajte Actor podľa stránky (napr. „Google Maps“, „Amazon“, „Website Content Crawler“)
1. Kliknite **Try for free** alebo **Run**
1. Nastavte vstupné parametre
1. Spustite a sledujte priebeh v sekcii **Runs**
1. Stiahnite dáta z **Dataset** (CSV, JSON, Excel...)

## Úlohy

!!! example "Úloha 27.1: Vytvorenie konta na Apify.com"

    Vytvorte si konto na Apify.com. Na prácu budete mať k dispozícii kredit vo veľkosti $5, 
    ktorý sa vám každý mesiac obnoví. Na učenie a základnú prácu to bohate stačí.

    - Oboznámte sa s platformou. Prezrite si Actors store, kde sú populárne skripty

!!! example "Úloha 27.2: Novinky našej školy"

    Vyskúšame si vytvoriť scraper, ktorý stiahne všetky aktuality zo stránky školy [https://www.spse-po.sk/aktuality](https://www.spse-po.sk/aktuality).

    Budeme mať záujem získať tieto informácie:

    - URL adresa, na ktorej sa nachádza aktualita (napr. [https://www.spse-po.sk/aktuality?page=6](https://www.spse-po.sk/aktuality?page=6))
    - URL adresa odkazujúca na aktualitu (napr. [https://www.spse-po.sk/aktuality/1382/valentinska-kvapka-krvi-spojila-ziakov-pre-dobru-vec](https://www.spse-po.sk/aktuality/1382/valentinska-kvapka-krvi-spojila-ziakov-pre-dobru-vec))
    - Titulok aktuality (napr. "Valentínska kvapka krvi spojila žiakov pre dobrú vec")
    - Textový popis (napr. "Dňa 13. februára 2026 sa na našej škole uskutočnilo podujatie Valentínska kvapka krvi, ktoré opäť ukázalo, že máme veľké srdcia a chuť pomáhať. Toto krásne podujatie sme zorganizovali... ")

    Nebudeme sťahovať samotnú stránku o danej aktualite, stačí nám iba titulok a popis. Preto nám bude stačiť prechádzať iba stránky začínajúce s cestou `https://www.spse-po.sk/aktuality?page=`

    Úlohy:

    - Oboznámte sa so štruktúrou stránky školy, a ako sú v nej organizované aktuality.
    - Pozrite si zdrojový kód stránky s aktualitami a identifikujte HTML elementy, ktoré obsahujú relevantné informácie.
    - Skúste navrhnúť, aké selektory sú potrebné na záskanie relevantných dát. Na to môžete použiť nástroj [https://scrapfly.io/web-scraping-tools/css-xpath-tester](https://scrapfly.io/web-scraping-tools/css-xpath-tester), do ktorého vložíte zdrojový kód stránky.
    

!!! example "Úloha 27.3: Apify actor"

    Vytvorte nový Apify actor skript, ktorý bude scrapovať aktuality našej stránky.

    - Použite šablónu "Python Crawlee with Parsel template"
    - Vo vstupných nastaveniach (Input data) nastavte štartovaciu stránku na [https://spse-po.sk/aktuality](https://spse-po.sk/aktuality)
    - Vyskúšajte spustiť Actor a prezrite si jeho výstupy

!!! example "Úloha 27.4: Úprava crawlera"

    - Upravte súbor `routes.py`, aby Apify crawloval iba stránky s aktualitami. Napr. takto:
        ```python
        await context.enqueue_links(
            include=[re.compile(r"^https://spse-po\.sk/aktuality\?.*")]
        ```
    - Upravte scrapovaciu logiku v súbore `routes.py`, aby scrapoval informácie o aktualitách. Napr. takto
        ```python
        data = []

        for div in context.selector.css('div.media-body'):
            a = div.css('h4 a')

            href = a.attrib.get('href') if a else None

            data.append({
                'url': url,
                'title': a.css('::text').get(default='').strip(),
                'article_url': href,
            })
        ```
    - Pridate do dát aj text popisu aktualít
    - Upravte súbor `dataset_schema.json` tak, aby rozumel vášmu formátu dát

## Zhrnutie cvičenia

- [x] Nástroje na web scraping pre Python
    * [ ] Requests — Jednoduchá a najpoužívanejšia knižnica na odosielanie HTTP požiadaviek.
    * [ ] Crawlee - Open-source crawling framework (JavaScript + Python) - moderný nástroj na tvorbu robustných scraprov
    * [ ] BeautifulSoup — Knižnica na parsovanie HTML a XML a jednoduchú extrakciu dát.
    * [ ] Scrapy — Plnohodnotný framework na veľké a škálovateľné crawling projekty.
    * [ ] Playwright — Moderná knižnica na automatizáciu prehliadača pri dynamických stránkach.
    * [ ] Selenium — Klasická knižnica na ovládanie reálneho prehliadača (stále používaná).
- [x] Nástroje na web scraping pre Javascript (node.js)
    * [ ] Axios — Ľahká a populárna knižnica na vykonávanie HTTP požiadaviek.
    * [ ] Cheerio — Rýchly HTML parser s jQuery-like API pre Node.js.
    * [ ] Playwright — Najmodernejšia knižnica na scrapovanie dynamických a JavaScript-heavy stránok.
    * [ ] Puppeteer — Veľmi populárna knižnica od Google na ovládanie Chrome prehliadača.
- [x] Etika a právne aspekty:
    * [ ] Zbierajte len verejné dáta
    * [ ] Rešpektujte Terms of Service stránok
    * [ ] Pri osobných údajoch dodržiavajte GDPR
    * [ ] Nevytvárajte DDoS útoky – scraping nie je útok
    * [ ] Buďte transparentní (v User-Agente môžete uviesť, že ste scraper)
