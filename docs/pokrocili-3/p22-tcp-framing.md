# Pokročílí 22: Rámcovanie správ nad TCP

Minule sme si vysvetlili, ako TCP pracuje s prúdom údajov - streamom. V praxi si však medzi zariadeniami potrebujeme posielať správy. Dnes si ukážeme, ako vieme nad TCP protokolom posielať rôzne typy správ.

TCP je spoľahlivý prúd bajtov. Keď pošleme `send("Ahoj")` a potom `send(" svet")`, na druhej strane to môže prísť ako:

- "Ahoj svet" naraz,
- "Ahoj" + " svet",
- "Aho" + "j svet",
- čokoľvek iné.

TCP nevie, kde končí jedna správa. Preto musíme na aplikačnej vrstve (nad TCP) definovať vlastný protokol – pravidlá, podľa ktorých klient a server vedia, kde začína a končí jedna správa.

## Rámcovanie správ

Rozdelenie toku bajtov - streamu - do jednotlivých správ sa volá **rámcovanie**, anglicky message framing. Rámcovanie správ nemá na starosti protokol TCP, ale je to úlohou aplikačnej vrstvy.

**Rámcovanie je spôsob, ako rozdelíme nekonečný TCP stream na jednotlivé správy**

Existujú tri základné techniky rámcovania správ:

- Rámcovanie s pevnou dĺžkou správy - fixed size
- Rámcovanie s oddeľovacím znakom - delimiter
- Rámcovanie s dĺžkou správy v hlavičke - length prefix

## Pevná dĺžka správy

Tento typ rámcovania je jednoduchý. Dáta sa posielajú vo forme správ, ktoré majú vopred dohodnutú pevnú dĺžku. Prijímateľ tak presne vie, koľko bajtov má načítať.

Tento typ rámcovania má niekoľko nevýhod:

- Väčšie správy sa musia rozdeliť medzi viacero správ, čo komplikuje protokol
- Malé správy mrhajú priestorom, keďže v správe ostanú nevyužité bajty

Protokoly s pevnou dĺžkou správy (fixed size framing) sú používané iba zriedka.

=== "Rámcovanie fixnou dĺžkou správy"

    ```python
    FIXED_SIZE = 64  # môžeš zmeniť na 32, 128, 256...

    # správa nemôže končiť znakom \0, ten sa používa na padding
    def send_fixed(sock: socket.socket, message: bytes):
        """Pošle správu s pevnou dĺžkou – doplní nulami ak je kratšia"""
        if len(message) > FIXED_SIZE:
            raise ValueError(f"Správa je dlhšia ako {FIXED_SIZE} bajtov!")
        padded = message.ljust(FIXED_SIZE, b'\0')   # doplní nulami
        sock.sendall(padded)                        


    def recv_fixed(sock: socket.socket) -> bytes | None:
        """Prijme presne FIXED_SIZE bajtov (ošetruje čiastočné recv)"""
        data = b""
        while len(data) < FIXED_SIZE:
            chunk = sock.recv(FIXED_SIZE - len(data))
            if not chunk:                           # spojenie zatvorené
                return None
            data += chunk
        # odstránime padding nuly
        return data.rstrip(b'\0')
    ```

## Oddeľovací znak - delimiter

Medzi populárnejšie metódy rámcovania patrí použitie oddeľovacieho znaku. Odosielateľ každú správu zakončí vopred dohodnutým oddeľovacím znakom, ktorý môže byť iba 1 bajt napr. 0x00, alebo '\n' (nový riadok). Oddeľovací znak sa však môže skladať aj z viacerých bajtov. Ide iba o to, ako sa dohodnú.

Prijímateľ dáta číta, až kým mu nepríde oddeľovací znak, potom načítané bajty spracuje do finálnej správy.

Použitie oddeľovacieho znaku šetrí miesto v porovnaní s pevnou dĺžkou správ. Medzi nevýhody tohto typu rámcovania patrí:

- Prijímateľ dopredu nevie, aká veľká bude správa - musí alokovať pamäť na správu dynamicky
- V tele správy sa nesmie vyskytovať oddeľovací znak - v takom prípade sa musí tzv. escapnuť, teda nahradiť iným znakom, aby si prijímateľ nemyslel, že nastal koniec správy

Ako príklad protokolu, ktorý využíva oddeľovací znak si môžeme uviesť protokol HTTP, kde sa správy zakončujú znakom `CRLF`, teda bajtami `"\r\n"`.

Tento typ rámcovania sa tiež používa, keď ani samotný odosielateľ dopredu nevie, kedy bude správa končiť, hlavne napr. pri nejakom streamovaní dát.

=== "Rámcovanie oddeľovacím znakom"

    ```python
    DELIMITER = b'\n'

    # Ak správa obsahuje \n vo vnútri, protokol sa pokazí
    def send_delimiter(sock: socket.socket, message: bytes):
        """Pošle správu ukončenú delimiterom"""
        if DELIMITER in message:
            raise ValueError("Správa nesmie obsahovať delimiter!")
        sock.sendall(message + DELIMITER)


    def recv_delimiter(sock: socket.socket, buffer: bytearray) -> bytes | None:
        """
        Prijme správu ukončenú DELIMITER.
        Používaš buffer (bytearray), ktorý si držíš mimo funkciu!
        """
        while True:
            # nájdeme delimiter v aktuálnom bufferi
            idx = buffer.find(DELIMITER)
            if idx != -1:
                message = bytes(buffer[:idx])      # vrátime správu bez delimiteru
                del buffer[:idx + len(DELIMITER)]  # odstránime spracovanú časť
                return message

            # delimiter ešte nie je → načítame ďalší chunk
            chunk = sock.recv(4096)
            if not chunk:
                if buffer:                         # zostali neúplné dáta
                    message = bytes(buffer)
                    buffer.clear()
                    return message
                return None                        # spojenie zatvorené

            buffer.extend(chunk)                   # pridáme do bufferu

    # na začiatku spojenia
    recv_buffer = bytearray()

    while True:
        msg = recv_delimiter(client, recv_buffer)
        if msg is None:
            break
        print("Prijaté:", msg.decode("utf-8"))
    ```

## Dĺžka správy v hlavičke

Tento asi najpopulárnejší spôsob rámcovania používa správy s pevnou dĺžkou hlavičky, v ktorej je uložená informácia o dĺžke celej správy. Prijímateľ tak po načítaní hlavičky vie, koľko bajtov musí ešte načítať, aby získal celú správu.

Tento spôsob je veľmi bezpečný a flexibilný a je aj najčastejšie využívaný. Nevýhody:

- komplikovanejší spôsob - prijímateľ najprv číta a spracuje hlavičku a až potom samotný obsah správy
- neefektívne pri prenose extrémne malých správ - hlavička môže byť oveľa väčšia ako samotná správa
- správy majú maximálnu veľkosť - správy väčšie ako maximálne veľkosť hlavičky sú ťažko prenesiteľné (v praxi napr. 4 GB)

=== "Rámcovanie hlavičkou s dĺžkou správy"

    ```python
    def send_length(sock: socket.socket, message: bytes):
        """Pošle 4-bajtovú dĺžku + samotnú správu"""
        length = len(message)
        header = struct.pack("!I", length)   # !I = big-endian unsigned 32-bit
        sock.sendall(header + message)


    def recv_length(sock: socket.socket) -> bytes | None:
        """Prijme správu s length prefix (ošetruje čiastočné recv)"""
        # 1. načítame 4 bajty hlavičky
        header = b""
        while len(header) < 4:
            chunk = sock.recv(4 - len(header))
            if not chunk:
                return None
            header += chunk

        length = struct.unpack("!I", header)[0]

        # 2. načítame presne 'length' bajtov dát
        data = b""
        while len(data) < length:
            chunk = sock.recv(length - len(data))
            if not chunk:
                return None
            data += chunk

        return data
    ```

## Úlohy

!!! example "Úloha 22.1: Klient pre hru bojové lode"

    Máme hru bojové lode, kde hráč sa snaží zničiť lode, ukryté na mape.

    Vytvorte TCP klienta, ktorý bude prijímať a posielať správy s hlavičkou, obsahujúcou dĺžku správy. Samotný obsah správy je JSON objekt. Meno hráča a súradnice zadávajte z klávesnice.

    Hlavička bude mať dĺžku 4 bajty. Na vytvorenie hlavičky použite funkciu `struct.pack("!I", dlzka)`

    Konverziu JSON z/na pole bajtov realizujte pomocou `json.dumps(msg).encode("utf-8")` a `json.loads(data.decode("utf-8"))`

    Príklady správy:

    ```json
    {"type": "guess",
     "x": 1,
     "y": 3,
     "player": "Fero"}
    ```

    Príklad odpovede, ktorú pošle server:
    ```json
    {"type": "error", "msg": "Neplatné súradnice!"}

    {"type": "result", "hit": False, "msg": "Už si to skúšal!"}

    {
        "type": "result",
        "hit": True,
        "sunk": True,
        "x": 1,
        "y": 2,
        "score": 3
    }
    ```

## Zhrnutie cvičenia

- [x] Rámcovanie správ nad TCP
    * [ ] TCP nevie, kde končí jedna správa. Preto musíme na aplikačnej vrstve (nad TCP) definovať vlastný protokol – pravidlá, podľa ktorých klient a server vedia, kde začína a končí jedna správa
    * [ ] Rozdelenie toku bajtov - streamu - do jednotlivých správ sa volá rámcovanie, anglicky message framing. 
    * [ ] Rámcovanie správ nemá na starosti protokol TCP, ale je to úlohou aplikačnej vrstvy.
    * [ ] Rámcovanie je spôsob, ako rozdelíme nekonečný TCP stream na jednotlivé správy
- [x] Pevná dĺžka správy - fixed size
    * [ ] Dáta sa posielajú vo forme správ, ktoré majú vopred dohodnutú pevnú dĺžku. Prijímateľ tak presne vie, koľko bajtov má načítať
    * [ ] Väčšie správy sa musia rozdeliť medzi viacero správ, čo komplikuje protokol
    * [ ] Malé správy mrhajú priestorom, keďže v správe ostanú nevyužité bajty
    * [ ] Protokoly s pevnou dĺžkou správy (fixed size framing) sú používané iba zriedka.
- [x] Oddeľovací znak - delimiter
    * [ ] Odosielateľ každú správu zakončí vopred dohodnutým oddeľovacím znakom, ktorý môže byť iba 1 bajt napr. 0x00, alebo '\n' (nový riadok). 
    * [ ] Oddeľovací znak sa však môže skladať aj z viacerých bajtov. Ide iba o to, ako sa dohodnú.
    * [ ] Prijímateľ dáta číta, až kým mu nepríde oddeľovací znak, potom načítané bajty spracuje do finálnej správy.
    * [ ] Prijímateľ dopredu nevie, aká veľká bude správa - musí alokovať pamäť na správu dynamicky
    * [ ] V tele správy sa nesmie vyskytovať oddeľovací znak - v takom prípade sa musí tzv. escapnuť, teda nahradiť iným znakom, aby si prijímateľ nemyslel, že nastal koniec správy
- [x] Dĺžka správy v hlavičke
    * [ ] Správy s pevnou dĺžkou hlavičky, v ktorej je uložená informácia o dĺžke celej správy. 
    * [ ] Prijímateľ tak po načítaní hlavičky vie, koľko bajtov musí ešte načítať, aby získal celú správu.
    * [ ] Tento spôsob je veľmi bezpečný a flexibilný a je aj najčastejšie využívaný
    * [ ] Komplikovanejší spôsob - prijímateľ najprv číta a spracuje hlavičku a až potom samotný obsah správy 
    * [ ] Neefektívne pri prenose extrémne malých správ - hlavička môže byť oveľa väčšia ako samotná správa
    * [ ] Správy majú maximálnu veľkosť - správy väčšie ako maximálne veľkosť hlavičky sú ťažko prenesiteľné (v praxi napr. 4 GB)


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Rámcovanie správ

    Spôsob, ako rozdelíme TCP stream na správy - anglicky message framing.
    Rámcovanie správ nemá na starosti protokol TCP, ale je to úlohou aplikačnej vrstvy.

    Existujú tri základné techniky rámcovania správ:

    - Pevnou dĺžkou správy - fixed size
    - Oddeľovacím znakom - delimiter
    - Dĺžkou správy v hlavičke - length prefix

    Pevná dĺžka správy - fixed size

    Správy majú vopred dohodnutú pevnú dĺžku. Prijímateľ presne vie, koľko bajtov má načítať
    Väčšie správy sa musia rozdeliť
    Malé správy mrhajú priestorom
    Je používaný iba zriedka.

    Oddeľovací znak - delimiter

    Odosielateľ správu zakončí oddeľovacím znakom, napr. 0x00, alebo '\n' (nový riadok).
    Oddeľovací znak sa môže skladať aj z viacerých bajtov
    Prijímateľ dáta číta, až kým mu nepríde oddeľovací znak
    Prijímateľ dopredu nevie, aká veľká bude správa
    V tele správy sa nesmie vyskytovať oddeľovací znak

    Dĺžka správy v hlavičke

    Správy s pevnou dĺžkou hlavičky, v ktorej je uložená informácia o dĺžke celej správy.
    Prijímateľ tak po načítaní hlavičky vie, koľko bajtov musí ešte načítať
    Veľmi bezpečné a flexibilné, najčastejšie využívaný spôsob
    Komplikovanejší spôsob - najprv číta hlavičku a až potom obsah správy
    Neefektívne pri prenose extrémne malých správ
    Správy majú maximálnu veľkosť (v praxi napr. 4 GB)
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Akým problém rieši rámcovanie
    - Aké metódy rámcovania správ poznáme
    - Vlastnosti a nevýhody rámcovania fixnou dĺžkou
    - Vlastnosti a nevýhody rámcovania oddeľovacím znakom
    - Vlastnosti a nevýhody rámcovania hlavičkou s dĺžkou správy
