# Pokročílí 17: Zvuk a hudba v Pygame

Na tomto cvičení pokračujete v práci na polročnom projekte. Okrem toho si ukážeme v Pygame prácu so zvukmi a hudbou.

Pygame podporuje prehrávanie zvukov vo formátoch .wav, .ogg a mp3

## Zvuky

Pygame nám umožňuje prehrávať zvuk a hudbu. Na prehrávanie zvuku nám slúži modul `pygame.mixer.Sound`. Vlastnosti:

- Vhodné pre krátke zvukové efekty (výstrel, náraz, skok, výbuch, ...)
- Pygame načíta celý zvuk do pamäti programu
- Vieme prehrávať viacero zvukov súčasne, resp. zvuky sa môžu prelínať

Inicializácia prehrávania zvuku pozostáva z nasledovných častí:

- Nastavenie počtu kanálov (koľko zvukov sa môže prehrať súčasne) pomocou `pygame.mixer.set_num_channels(32)`
- Načítanie zvuku zo súboru pomocou `jump_sound = pygame.mixer.Sound("jump.wav")`
- Nastaviť hlasitosť zvuku pomocou `jump_sound.set_volume(0.7)` (defaulne je 1.0)

Samotné prehrávanie zvuku počas hry sa vykoná nasledovne:

- `jump_sound.play()` - prehrá ihneď daný zvuk
- `jump_sound.play(loops=2)` - prehrá zvuk 2x
- `jump_sound.play(0, 1200)` - prehrá iba prvých 1200ms zvuku

## Hudba

Okrem zvukov Pygame umožňuje aj prehrávanie hudby. Slúži na to modul `pygame.mixer.music`. Vlastnosti:

- Dlhá hudba ako pozadie do hry
- Nenačíta celý súbor naraz, ale postupne "streamuje" dáta
- Vie naraz prehrať iba 1 hudbu
- Podporuje nekonečné prehrávanie (looping)

Prehrávanie hudby funguje nasledovne:

- Otvorenie súboru s hudbou pomocou `pygame.mixer.music.load("hudba.mp3")`
- Nastavenie hlasitosti pomocou `pygame.mixer.music.set_volume(0.5)` (odporúča sa hudbu púšťať tichšie ako zvuky)
- Spustenie hudby pomocou `pygame.mixer.music.play(-1)` - prehrá hudbu s nekonečnou slučkou

Okrem týchto funkcií pygame podporuje aj nasledovné:

- `pygame.mixer.music.pause()` - Pauza
- `pygame.mixer.music.unpause()` - Odpauzovanie
- `pygame.mixer.music.stop()` - Zastavenie prehrávania
- `pygame.mixer.music.fadeout(2000)` - Pomalé postupné zastavenie
- `pygame.mixer.music.queue("next_track.ogg")` - Nastavenie ďalšej skladby

## Pong s hudbou

Ako príklad použitia zvukov a hudby si môžete vyskúšať upravenú verziu hry Pong z minulých cvičení. Na adrese `https://github.com/wagjo/pong` má te repozitár shrou Pong, do ktorej boli pridané zvuky a hudba.

## Databáza zvukov

Ak do vášho projektu potrebujete nejaké zvuky alebo hudbu, existuje veľké množstvo voľne dostupných zdrojov.

=== "Dostupné zdroje na zvuky do hier (SFX)"

| **Zdroj**                                     | **Popis**                                                                | **Licencia / použitie**                                                                | **Formáty / množstvo obsahu**       | **Odkaz**                                                                                                                          |
| --------------------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Freesound**                                 | Komunitou spravovaný repozitár zvukov.                                   | Rozličné **Creative Commons licence** (CC0 aj s atribúciou) | 500 000+ zvukov                     | [https://freesound.org](https://freesound.org)
| **Zapsplat**                                  | Veľká knižnica profesionálnych SFX + hudby pre rôzne žánre.              | Royalty-free (komerčné aj nekomerčné), **atribúcia pri free účte**                     | ~160 000+ SFX a 800+ hudobných stôp | [https://www.zapsplat.com](https://www.zapsplat.com)                                 |
| **Mixkit (Envato)**                           | Kvalitné zvuky bez registrácie vhodné pre hry, videá a UI.               | Royalty-free, bez atribúcie                                                            | Stovky základných SFX               | [https://mixkit.co/free-sound-effects/](https://mixkit.co/free-sound-effects/)                   | 
| **sfxr.me** (generátor zvukov) | Webový nástroj na generovanie retro SFX zvukov | Neobmedzené komerčné použitie | WAV | [https://sfxr.me/](https://sfxr.me/)

Podobne existuje aj databáza hudby, ktorú môžete použiť ako pozadie do vašej hry. Niektore z vyššie uvedených stránok ponúkajú aj hudbu. Okrem toho sú aj ďalšie zdroje:

=== "Dostupné zdroje pre hudbu"

| **Zdroj**                              | **Popis + čo ponúka**                                                                                                                                                                    | **Licencia / použitie**                                                                                                      | **Odkaz**                                                                                                            |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Pixabay – hudba**                    | Veľká knižnica royalty-free hudby pre komerčné aj nekomerčné použitie. Zahŕňa aj hry-orientované skladby (napr. „games“, chiptune, ambient).                           | Royalty-free, **bez atribúcie** pre komerčné a nekomerčné projekty (podmienky Pixabay Content License).    | [https://pixabay.com/sk/music/](https://pixabay.com/sk/music/)                                                       |
| **Uppbeat – free game music**          | Kategória hudby špeciálne pre hry – epické, retro, akčné, chill hudba a ďalšie.                                                                                     | Royalty-free pre komerčné a nekomerčné použitie podľa podmienok služby.                                    | [https://uppbeat.io/music/category/game](https://uppbeat.io/music/category/game)                                     |
| **No Copyright Music (Liborio Conti)** | Kompozície bez autorských poplatkov, ktoré môžeš použiť **komerčne aj nekomerčne**.                                          | Royalty-free   | [https://www.no-copyright-music.com/](https://www.no-copyright-music.com/)                                           |
| **Bensound**       | Vysoko kvalitná hudba od jedného zdroja, jednoduché licenčné podmienky, vhodné pre herné soundtracky a BGM.  | Free skladby často pod **Creative Commons s atribúciou** (uvedenie autora), **Pro licencia** bez atribúcie a s rozšírenými právami (platená)                              |  [https://www.bensound.com/](https://www.bensound.com/) |
| **OpenGameArt**    | Veľmi široká zbierka hudby priamo pre herné projekty. **Každá skladba má vlastnú licenciu**, treba ju skontrolovať pri stiahnutí.                       | Rôzne **slobodne licencované hudobné súbory**: CC0 (public domain), **CC-BY 3.0 / CC-BY-SA**, GPL/LGPL; **komerčné použitie** je možné podľa licencie každej skladby  | [https://opengameart.org/](https://opengameart.org/)   |
| **Soundimage.org** | Veľká zbierka originálnych hudobných tém a skladieb rozdelených do žánrov (fantasy, sci-fi, ambient atď.). Atraktívne pre indie a herné projekty.             | Royalty-free **povolené komerčné použitie** s **povinnou atribúciou autora (Eric Matyas)** – pozri atribučné info na webe                                       |  [https://soundimage.org/](https://soundimage.org/)     |

