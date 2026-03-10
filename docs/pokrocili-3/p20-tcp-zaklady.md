# Pokročílí 20: TCP protokol

V téme sieťovej komunikácie pokračujeme ďalším populárnym protokolom - TCP. Dnes si predstavíme jeho vlastnosti a ukážeme si základy práce s TCP v Pythone.

## Transmission Control Protocol

TCP (Transmission Control Protocol) je populárny protokol na 4. vrstve OSI modelu (transportná vrstva). 

Tento protokol je vo viacerých veciach iný ako UDP. Jedným z hlavných rozdielov je, že TCP je stream-oriented (nie message-oriented). Nepoužíva „správy“ (datagramy), ale súvislý prúd bajtov - **stream**.

Kľúčové vlastnosti TCP:

- Spoľahlivosť - stratené segmenty sa automaticky znovu pošlú
- Poradie - dáta prídu presne v tom poradí, v akom boli poslané
- Obojsmerný stream - obe strany môžu posielať a prijímať súčasne

TCP je connection oriented, to znamená, že sender a receiver (vysielateľ a prijímateľ, client/server) musia najprv nadviazať spojenie a až potom si začnú posielať údaje.

Podobne ako pri UDP aj pre prácu s TCP v Pythone používame štandardný modul `socket`.

Nevýhody TCP oproti UDP:

- vyššie oneskorenie a overhead
- nevhodné pre realtime komunikáciu a online hry
- vyššie nároky na server
- nepodporuje broadcast/multicast
- pomalší štart

Aj napriek týmto nevýhodam je však TCP veľmi používaný, a to hlavne vďaka jeho spoľahlivosti. 

## Fázy spojenia

TCP pracuje v troch fázach spojenia.

1. Nadviazanie spojenia - viackroková operácia za účelom nadviazania spojenia. V Pythone na to používame funkcie `listen()`, `connect()` a `accept()`
1. Prenos dát - obojsmerný prenos údajov. V Pythone používame `sendall()` a `recv()`
1. Ukončenie spojenia - riadené zatvorenie spojenia a uvoľnenie zdrojov. V Pythone používame `shutdown()` a `close()`

Dnes si bližšie vysvetlíme prvú fázu - nadviazanie spojenia

## Nadviazanie spojenia

Pre nadviazanie spojenia je potrebné, aby jeden z účastníkov počúval (*listen*) na svojom porte a zachytil (*accept*) prichádzajúce žiadosti o spojenie. Tohto účastníka nazývame aj server. Druhý účastník - client - žiada server o nadviazanie spojenia (*connect*). 

Mechanizmus nadviazania spojenia v TCP sa volá **Three-way handshake**

![Three way handshake](../assets/three-way-handshake.jpg){align=right .on-glb width=400}

Obidve strany spojenia si potrebujú overiť, či ten druhý počúva a či je pripravený prijímať dáta. Taktiež si potrebujú dohodnúť niektoré parametre spojenia. Pri three-way handshake sa posielajú tri správy, tzv. TCP segmenty.

1. Klient pošle `SYN` segment
2. Server odpovie segmentom `SYN+ACK`
1. Klient nakoniec odošle `ACK` segment

Prvý `SYN` TCP segment (správa) od klienta otvára spojenie a žiada server o pripojenie. Server odpovie správou `SYN+ACK`. Tým sa klient dozvie, že server počúva a je pripravený vytvoriť spojenie. Server tým zároveň žiada klienta o finálne potvrdenie, že spojenie akceptuje. Klient teda nakoniec odpovie TCP segmentom `ACK` a spojenie je pripravené na prenos dát. 

Vnútri úvodných `SYN` segmentov si jednotlivé strany taktiež vymenia rôzne nastavenia spojenia, aby spolu vedeli efektívne komunikovať.

## Prenos údajov

Po nadviazaní spojenia si môžu klient a server medzi sebou začať posielať dáta. Na rozdiel od UDP si v TCP jednotlivé strany neposielajú správy, ale dáta sú posielané v súvislom toku, tzv. streame.

Bližšie o tom, ako TCP prenáša údaje, si povieme nabudúce, dnes si iba ukážeme, ako sa v Pythone dajú dáta dosielať a prijímať.

Začneme kódom pre klienta. Jeho úlohou je požiadať o spojenie, poslať a prijať dáta a na konci spojenie ukončiť.

```python
import socket

# Pre TCP použijeme typ socket.SOCK_STREAM
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM);

# 1. Nadviazanie spojenia - CONNECT (THREE-WAY HANDSHAKE)
sock.connect(('127.0.0.1', 65432))

# 2. Pošleme správu serveru
message = "Ahoj server! Toto je test spojenia."
data = message.encode('utf-8')
sock.sendall(data)

# 3. Prijmeme odpoveď
response = sock.recv(1024)
if response:
    message = response.decode('utf-8')
    print(f"Odpoveď od servera: {message}")

# 4. Shutdown - oznámime serveru, že už nebudeme posielať
sock.shutdown(socket.SHUT_WR)

# 5. Ešte raz recv() - počkáme, kým server všetko spracuje
final_data = sock.recv(1024)
if final_data:
    message = final_data.decode('utf-8')
    print(f"Zostávajúca odpoveď: {message}")

# 6. Zatvorenie spojenia
sock.close()
```

Server to má trošku zložitejšie. Server musí počúvať na porte a vytvoriť samostatné vlákno pre každé nové spojenie. Na server sa totižto zvykne pripojiť viacero klientov súčasne. V tomto príklade však budeme čakať iba na prvého klienta, prijmeme od neho správu a pošleme mu ju naspať. Potom program ukončíme.

```python
import socket

HOST = '127.0.0.1'  # localhost (pre testovanie)
PORT = 65432  # ľubovoľný port > 1024

# Pre TCP použijeme typ socket.SOCK_STREAM
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 1. Viažeme socket na IP + port
sock.bind((HOST, PORT))

# 2. Začneme počúvať (5 je max počet klientov čakajúcich na pripojenie)
sock.listen(5)

# 3. Prijatie spojenia (THREE WAY HANDSHAKE)
conn, addr = sock.accept()

# Teraz máme aktívne TCP spojenie (conn)
# Môžeme posielať a prijímať dáta...

# 4. Server čaká na správu od klienta
data = conn.recv(1024)
if data:
    message = data.decode('utf-8')
    print(f"Prijaté od klienta: {message}")
    # 5. Poslanie správy naspať klientovi
    conn.sendall(data)

# 6. Zatvorenie serveru
conn.close()
sock.close()
```

## Úlohy

!!! example "Úloha 20.1: TCP klient a server"

    Vyskúšajte lokálne spustiť príklady z tohto cvičenia


!!! example "Úloha 20.2: Interaktívny klient"

    Upravte klienta tak, aby načítaval správy z klávesnice a posielal ich serveru. Zároveň v samostanom vlákne prijímajte odpovede zo servera a vypisujte ich do konzoly.

    Ošetrite, aby sa program dal korektne ukončiť klávesou CTRL-C


!!! example "Úloha 20.3: Lepší server"

    Upravte server tak, aby v nekonečnej slučke prijímal nové spojenia a obsluhoval ich na samostatných vláknach.

    V rámci jedného obsluhovania klienta server nech prijíma správy a preposiela ich naspať klientovi.

    Využite settimeout pri sockete aj pri connection tak, aby sa dal server vypnúť klávesou CTRL-C


## Zhrnutie cvičenia

- [x] TCP (Transmission Control Protocol) - protokol na 4. vrstve OSI modelu (transportná vrstva)
    * [ ] Stream-oriented, nie message-oriented. Nepoužíva „správy“ (datagramy), ale súvislý prúd bajtov - stream.
    * [ ] TCP je connection oriented, to znamená, že client a server musia najprv nadviazať spojenie a až potom si začnú posielať údaje.
    * [ ] Pre prácu s TCP v Pythone používame štandardný modul `socket`
- [x] Vlastnosti TCP
    * [ ] Spoľahlivosť - stratené segmenty sa automaticky znovu pošlú
    * [ ] Poradie - dáta prídu presne v tom poradí, v akom boli poslané
    * [ ] Obojsmerný stream - obe strany môžu posielať a prijímať súčasne
- [x] Nevýhody TCP oproti UDP
    * [ ] vyššie oneskorenie a overhead
    * [ ] nevhodné pre realtime komunikáciu a online hry
    * [ ] vyššie nároky na server, pomalší štart
    * [ ] nepodporuje broadcast/multicast
- [x] Fázy spojenia
    * [ ] Nadviazanie spojenia - viackroková operácia za účelom nadviazania spojenia. V Pythone na to používame funkcie `listen()`, `connect()` a `accept()`
    * [ ] Prenos dát - obojsmerný prenos údajov. V Pythone používame `sendall()` a `recv()`
    * [ ] Ukončenie spojenia - riadené zatvorenie spojenia a uvoľnenie zdrojov. V Pythone používame `shutdown()` a `close()`
- [x] Nadviazanie spojenia
    * [ ] Pre nadviazanie spojenia je potrebné, aby jeden z účastníkov počúval (listen) na svojom porte a zachytil (accept) prichádzajúce žiadosti o spojenie. Tohto účastníka nazývame aj server. Druhý účastník - client - žiada server o nadviazanie spojenia (connect).
    * [ ] Mechanizmus nadviazania spojenia v TCP sa volá Three-way handshake
- [x] Three-way handshake
    * [ ] 1. Klient pošle SYN segment
    * [ ] 2. Server odpovie segmentom SYN+ACK
    * [ ] 3. Klient nakoniec odošle ACK segment
    * [ ] Vnútri úvodných SYN segmentov si jednotlivé strany taktiež vymenia rôzne nastavenia spojenia, aby spolu vedeli efektívne komunikovať.


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    TCP - protokol na 4. vrstve OSI
    
    Stream-oriented, používa súvislý prúd bajtov - stream.
    Connection oriented - client a server musia najprv nadviazať spojenie
    Používame štandardný Python modul socket

    Vlastnosti TCP
    - Spoľahlivosť - stratené segmenty sa automaticky znovu pošlú
    - Poradie - dáta prídu presne v tom poradí, v akom boli poslané
    - Obojsmerný stream - obe strany môžu posielať a prijímať súčasne

    Nevýhody TCP oproti UDP
    - vyššie oneskorenie a overhead
    - nevhodné pre realtime komunikáciu a online hry
    - vyššie nároky na server, pomalší štart
    - nepodporuje broadcast/multicast

    Fázy spojenia
    1. Nadviazanie spojenia - používame funkcie `listen()`, `connect()` a `accept()`
    2. Prenos dát - používame sendall() a recv()
    3. Ukončenie spojenia - používame shutdown() a close()

    Nadviazanie spojenia
    
    Server počúva (listen) na svojom porte a zachytí (accept) žiadosti o spojenie. 
    Client žiada server o nadviazanie spojenia (connect).
    Mechanizmus nadviazania spojenia sa volá Three-way handshake

    Three-way handshake:
    1. Klient pošle SYN segment
    2. Server odpovie segmentom SYN+ACK
    3. Klient nakoniec odošle ACK segment
    
    SYN segmenty obsahujú aj rôzne nastavenia spojenia
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je TCP, jeho vlastnosti
    - Výhody a nevýhody TCP
    - 3 fázy spojenia
    - Nadviazanie spojenia, three way handshake
    - Kód v Pythone na nadviazanie spojenia a posielanie/prijímanie