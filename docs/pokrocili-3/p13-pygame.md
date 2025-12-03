# Pokročílí 13: Pygame

Dnes si vysvetlíme základy knižnice Pygame a vytvoríme si jednoduchú 2D hru

## Pygame

Pygame je knižnica pre tvorbu 2D hier v Pythone. Stránka tohto projektu je [https://www.pygame.org](https://www.pygame.org). Obsahuje dokumentáciu a množstvo návodov.

Knižnicu Pygame vieme nainštalovať klasicky pomocou `pip install pygame`

Funkcionality tejto knižnice sú prístupné v module `pygame`. Pred prvým použitím knižnice je potrebné spustiť inicializáciu pomocou `pygame.init()`


=== "Inicializácia Pygame"

    ```python
    import pygame

    pygame.init()
    ```

## Vytvorenie hlavného okna

V Pygame pracujeme s oknom, do ktorého budeme vykresľovať grafiku hry. Pri vytvorení okna je potrebné zadať jeho veľkosť. Premennú s objektom okna je potrebné si zapamätať, pretože funkcie na vykresľovanie vyžadujú okno ako prvý argument.

- okno vytvoríme pomocou `screen = pygame.display.set_mode((SIRKA, VYSKA))`
- titulok okna zadefinujeme pomocou `pygame.display.set_caption("Titulok")`

=== "Vytvorenie okna"

    ```python
    import pygame

    SIRKA = 800
    VYSKA = 600

    pygame.init()
    screen = pygame.display.set_mode((SIRKA, VYSKA))
    pygame.display.set_caption("Hra")
    ```

## Farby

Farby sú reprezentované ako `tuple` s tromi alebo štyrmi hodnotami (RGB alebo RGBA). Hodnoty sú v rozsahu 0 až 255, pričom `(0, 0, 0)` predstavuje čiernu farbu a `(255, 255, 255)` predstavuje bielu farbu.

Prvý prvok je červená zložka, druhý zložka zelená a tretí zložka modrá. (Štvrty prvok je alfa zložka, ktorá určuje priehľadnosť farby)

## Trieda Rect

Väčšina vecí v 2D hrách funguje na princípe obdĺžnikov. Či už ide o vykresľovanie, hitbox alebo písanie textu, väčšinou je k tomu potrebné mať obdĺžnik, ktorý určuje miesto na obrazovke.

V Pygame na reprezentáciu obdĺžnikov máme triedu `Rect`.

Vytvorenie nového rect objektu robíme pomocou konštruktora `pygame.Rect(x, y, width, height)`. Táto trieda nám poskytuje rôzne atribúty, pomocou ktorých môžeme zistiť ale aj meniť jeho vlastnosti:

| Atribút                      | Typ    | Popis                                        |
| ---------------------------- | ------ | -------------------------------------------- |
| `x`                        | int    | X-ová súradnica ľavého okraja.               |
| `y`                        | int    | Y-ová súradnica horného okraja.              |
| `width (w)`                | int    | Šírka obdĺžnika.                             |
| `height (h)`               | int    | Výška obdĺžnika.                             |
| `topleft`                  | (x, y) | Horný-ľavý roh.                              |
| `topright`                 | (x, y) | Horný-pravý roh.                             |
| `bottomleft`               | (x, y) | Dolný-ľavý roh.                              |
| `bottomright`              | (x, y) | Dolný-pravý roh.                             |
| `center`                   | (x, y) | Stred obdĺžnika.                             |
| `centerx`                  | int    | X-ová súradnica stredu.                      |
| `centery`                  | int    | Y-ová súradnica stredu.                      |
| `top`                      | int    | Y-ová súradnica hornej hrany.                |
| `bottom`                   | int    | Y-ová súradnica spodnej hrany.               |
| `left`                     | int    | X-ová súradnica ľavej hrany.                 |
| `right`                    | int    | X-ová súradnica pravej hrany.                |

### Detekcia kolízie

Trieda `Rect` nám poskytuje metódy na detekciu kolízie dvoch obdĺžnikov.

- `rect.colliderect(other_rect)` vráti true, ak sa obdĺžniky prekrývajú
- `rect.contains(other_rect)` vráti true, ak je obdĺžnik `other_rect` celý vo vnútri obdĺžnika
- `rect.collidepoint(x, y)` vráti true, ak je bod `(x, y)` vo vnútri obdĺžnika

## Vykresľovanie

### Vykresľovanie tvarov

Vykresľovanie rôznych tvarov sa robí pomocou modulu `pygame.draw`

- `pygame.draw.rect(screen, color, rect)` - vykreslí obdĺžnik
- `pygame.draw.ellipse(screen, color, rect)` - vykreslí elipsu
- `pygame.draw.line(screen, color, start_pos, end_pos, width = 1)` - vykreslí čiaru

### Vykresľovanie textu

Vykresľovanie textu sa robí pomocou modulu `pygame.font`. Najpr je potrebné mať vytvorený font.

- `font = pygame.font.SysFont(None, size)` vráti defaultný systémový font
- `font = pygame.font.SysFont(nazov, size)` vráti systémový font s daným názvom, napr. "Times New Roman" alebo "Arial"

Ak máme objekt s fontom, vieme vykresliť text pomocou `font.render` a potom je potrebné vykreslený text umiestniť na obrazovku pomocou `screen.blit`:

=== "Vykreslenie textu"

    ```python
    text = font.render("Hello World!", True, color)
    screen.blit(text, (X, Y))
    ```

## Udalosti

Počas hry môžu nastať rôzne udalosti, napríklad môže byť stlačená klávesa alebo tlačítko myšky. Takéto jednorázové udalosti je možné zachytiť pomocou modulu `pygame.event`.

=== "Spracovanie udalostí"
    ```python
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                # stlačenie ESC
    ```

### Držanie stlačenej klávesy

Okrem jednorázových udalostí je často potrebné vedieť, či je aktuálne nejaká klávesa stlačená alebo nie. To sa dá zistiť pomocou modulu `pygame.key`

=== "Spracovanie stavu klávesnice"
    ```python
    klavesy = pygame.key.get_pressed()
    if klavesy[pygame.K_LEFT]:
        hrac.x -= rychlost
    if klavesy[pygame.K_RIGHT]:
        hrac.x += rychlost
    ```

## Hlavná herná slučka

Hra zvyčajne beží v nekonečnom cykle. Jedna iterácia je jeden tick alebo frame - snímka. Aby sa herná slučka neopakovala veľmi rýchlo, je potrebné nastaviť počet snímkov za sekundu (FPS - frames per second) a použiť triedu `Clock`, ktorá nám spomalí hlavnú slučku na požadované FPS.

Úloha hlavnej slučky je zachytiť a spracovať udalosti, aktualizovať stav sveta hry a potom vykresliť hru na obrazovku.

Všetko ostatné je potrebné si pripraviť vopred. Je zbytočné vytvárať fonty a načítavať obrázky v každej iterácii slučky, v každom frame. Spomaľuje to vykresľovanie a môže to zaplniť pamäť alebo vnútorné úložisko zdrojov. 

=== "Hlavná herná slučka"

    ```python
    clock = pygame.time.Clock()

    while True:
        # 1. Spracovanie udalosti
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            ...

        # 2. Aktualizácia stavu hry, napr. pohyb objektov
        update()

        # 3. Vyčistiť obraz
        screen.fill((0, 0, 0))

        # 4. Vykresliť tvary, text a obrázky
        draw()

        # 5. Zobraziť na obrazovku
        pygame.display.flip()

        # 6. Spomaliť na dané FPS
        clock.tick(60)
    ```

## Úlohy na hodine

Dnes si vytvoríme hru Pong. Základné vlastnosti:

- hra pre dvoch hráčov
- každý hráč ovláda dosku na kraji obrazovky, ľavom a pravom
- hráč vie doskou pohybovať hore a dole
- po obrazovke lieta lopta a odráža sa od hornej a dolnej steny a od dosiek
- cieľom je dostať loptu za súperovú dosku, dať gól

!!! example "Úloha 13.1: Hra Pong"

    1. V novom alebo v existujúcom projekte si nainštalujeme pygame pomocou `pip install pygame`

    1. Vytvoríme si modul `pong.py`

    1. V module si importneme modul `pygame` a zavoláme metódu `pygame.init()`

    1. V moduli si zadefinujeme nasledovné konštanty:

        ```python
        WIDTH, HEIGHT = 800, 600
        FPS = 60
        WHITE = (255, 255, 255)
        BLACK = (0, 0, 0)
        ```

!!! example "Úloha 13.2: Trieda Paddle"

    Vytvoríme si triedu Paddle, ktorá bude reprezentovať dosku. Táto trieda bude mať metódy na vykreslenie a na aktualizáciu stavu - pohyb - podľa toho, ktorá klávesa je stlačená

    1. Vytvorte triedu `Paddle` a v nej konštruktor s parametrami `x`, `y`, `sirka`, `vyska`, `rychlost`. Niektorým parametrom dajte defaultné hodnoty: `sirka=15`, `vyska=100`, `rychlost=8`. Trieda nech má 2 atribúty: `rychlost` a obdĺžnik `rect`, ktorý vytvoríte zo vstupných argumentov konštruktora.

    1. Vytvorte metódu `draw(self, screen)`, ktorý vykreslí biely (WHITE) obdĺžnik `rect` na obrazovku pomocou `pygame.draw`.

    1. Vložte do triedy nasledovnú metódu na aktualizáciu stavu:

        ```python
        def move(self, up_key, down_key):
            keys = pygame.key.get_pressed()
            if keys[up_key] and self.rect.top > 0:
                self.rect.y -= self.rychlost
            if keys[down_key] and self.rect.bottom < HEIGHT:
                self.rect.y += self.rychlost
        ```

!!! example "Úloha 13.3: Trieda Ball"

    Vytvoríme si triedu Ball, ktorá bude reprezentovať loptu. Táto trieda bude mať metódy na vykreslenie a na aktualizáciu stavu - pohyb, a taktiež metódu na resetovanie lopty po góle

    1. Vytvorte triedu `Ball` a v nej konštruktor s parametrami `velkost` a `rychlost`. Parametrom dajte defaultné hodnoty: `velkost=25`, `rychlost=6`. Trieda nech má 2 atribúty: `rychlost` a štvorec `rect`, ktorý bude mať veľkosť podľa vstupného argumentu. Na konci konštruktora zavolajte metódu `reset`, ktorú si vložíme v ďalšom bode

    1. Importujte si modul `random` a vložte do triedy nasledovnú metódu na resetovanie stavu:

        ```python
        def reset(self):
            self.rect.center = (WIDTH // 2, HEIGHT // 2)
            self.speed_x = self.rychlost * random.choice((1, -1))
            self.speed_y = self.rychlost * random.choice((1, -1))
        ```

    1. Vytvorte metódu `draw(self, screen)`, ktorý vykreslí biely (WHITE) kruh na mieste štvorca `rect` na obrazovku pomocou `pygame.draw.ellipse`.

    1. Vytvorte metódu `move(self)`, ktorá pripočíta k atribútom `x` a `y` štvorce `rect` hodnoty `speed_x` a `speed_y

!!! example "Úloha 13.4: Trieda PongGame"

    1. Vytvorte triedu `PongGame` s nasledovným kódom:

        ```python
        class PongGame:
            def __init__(self):
                self.screen = pygame.display.set_mode((WIDTH, HEIGHT))
                pygame.display.set_caption("Pong - OPGP")
                self.clock = pygame.time.Clock()

                # Game objects
                paddle_y = HEIGHT // 2 - 50
                self.left_paddle = Paddle(30, paddle_y)
                self.right_paddle = Paddle(WIDTH - 45, paddle_y)
                self.ball = Ball()

            def handle_collision(self):
                # Top/Bottom walls
                if self.ball.rect.top <= 0 or self.ball.rect.bottom >= HEIGHT:
                    self.ball.speed_y *= -1

                # Paddle collisions
                if self.ball.rect.colliderect(self.left_paddle.rect) and self.ball.speed_x < 0:
                    self.ball.speed_x *= -1

                if self.ball.rect.colliderect(self.right_paddle.rect) and self.ball.speed_x > 0:
                    self.ball.speed_x *= -1

            def check_scoring(self):
                if self.ball.rect.left <= 0:
                    self.ball.reset()
                if self.ball.rect.right >= WIDTH:
                    self.ball.reset()

            def run(self):
                while True:
                    for event in pygame.event.get():
                        if event.type == pygame.QUIT:
                            pygame.quit()
                            sys.exit()

                    # Move
                    ## TODO

                    # Update
                    self.handle_collision()
                    self.check_scoring()

                    # Draw
                    ## TODO

                    pygame.display.flip()
                    self.clock.tick(FPS)
        ```

    1. V časti `# Move` je potrebné doplniť pohyb objektov nasledovne

        - zavolajte metódu pohybu ľavej dosky s klávesami `pygame.K_w` a `pygame.K_s` (pozrite si, ako je metóda na pohyb naimplementovaná)
        - zavolajte metódu pohybu pravej dosky s klávesami `pygame.K_UP`, `pygame.K_DOWN`
        - zavolajte metódu pohybu lopty

    1. V časti `# Draw` je potrebné doplniť vykresľovanie objektov nasledovne

        - vyčistite obrazovku pomocou `self.screen.fill`
        - vykreslite ľavú dosku
        - vykreslite pravú dosku
        - vykreslite loptu


!!! example "Úloha 13.5: Spustenie hry"

    Hru vieme spustiť tak, že vytvoríme objekt triedy `PongGame` a zavoláme jeho metódu `run`.

    Hru si môžete zahrať. Ak je príliš náročná alebo naopak ľahká, zmeňte rýchlosti a dĺžky dosiek. 

    Hra je funkčná, ale chýba jej veľa vecí. V ďalších úlohách ju vylepšíme.

## Úlohy na precvičenie

!!! example "Úloha 13.6: Pridanie stredovej čiary"
    
    1. V triede `PongGame` vytvorte metódu `draw_center_line` s nasledovným kódom:

        ```python
        for i in range(0, HEIGHT, 20):
            if i % 40 == 0:
                pygame.draw.line(self.screen, WHITE, (WIDTH // 2, i), (WIDTH // 2, i + 10), 3)
        ```

    1. Zavolajte túto metódu v hlavnej slučke pri vykresľovaní


!!! example "Úloha 13.7: Počítanie skóre"

    Vytvoríme triedu, ktorá bude uchovávať skóre a vykresľovať ho na obrazovku.

    1. Vytvorte triedu `Score` s atribútmi `left_score` a `right_score`. Obe inicializujte na 0. V konštruktore tiež vytvorte atribút `font`, ktorý inicializujte na `pygame.font.SysFont(None, 74)`

    1. Vytvorte metódy goal_left() a goal_right(), ktoré budú zvyšovať skóre odpovedajúcej strany.

    1. Vložte do triedy nasledovnú metódu na vykresľovanie:

        ```python
        def draw(self, screen):
            left_text = self.font.render(str(self.left_score), True, WHITE)
            right_text = self.font.render(str(self.right_score), True, WHITE)
            screen.blit(left_text, (WIDTH // 4, 20))
            screen.blit(right_text, (3 * WIDTH // 4, 20))
        ```
    
    1. V konštruktore triedy `PongGame` si vytvorte atribút `score` s objektom triedy `Score`. V hlavnej slučke zavolajte vykresľovanie skóre.

    1. V metóde `check_scoring` zavolajte metódu `goal_left()` alebo `goal_right()` podľa toho, kto vyhral

!!! example "Úloha 13.8: Zmena uhla lopty"

    Lopta letí stále pod rovnakým uhlom. Tento uhol teraz zmeníme tak, že uhol bude závisieť od toho, v akom bode dosky sa lopta odrazila. Ak sa odrazila bližšie pri strede, uhol bude menší. Ak pri okraji, uhol bude väčší.

    1. Pridajte do triedy `PongGame` nasledovnú metódu:

        ```python
        def apply_spin(self, paddle):
            # Add spin based on where the ball hits the paddle
            relative_intersect_y = (paddle.rect.centery - self.ball.rect.centery)
            normalized = relative_intersect_y / (paddle.rect.height / 2)
            self.ball.speed_y = -8 * normalized  # Max spin effect
        ```
    
    1. Zavolajte túto metódu v metóde `handle_collision` v situáciách, keď lopta narazila a otáča sa

!!! example "Úloha 13.9: Game Over"

    Hra nemá koniec. Upravte kód tak, aby sa hra skončila ak niektorý z hráčov dosiahne určité skóre. Na konci vypíšte "Game Over" a kto hru vyhral.

!!! example "Úloha 13.10: Hra pre troch hráčov"

    Upravte hru tak, aby bola pre troch hráčov. Tretí hráč bude mať dosku v dolnej časti obrazovky.


## Zhrnutie cvičenia

- [x] Pygame, knižnica na tvorbu 2D hier, https://www.pygame.org
    * [ ] Inštalácia pomocou `pip install pygame`
    * [ ] Modul `pygame`
    * [ ] Pred prvým použitím knižnice je potrebné spustiť inicializáciu pomocou `pygame.init()`
- [x] Hlavné okno
    * [ ] Okno vytvoríme pomocou `screen = pygame.display.set_mode((SIRKA, VYSKA))`
    * [ ] Titulok okna zadefinujeme pomocou `pygame.display.set_caption("Titulok")`
- [x] Farby
    * [ ] Farby sú reprezentované ako tuple s tromi alebo štyrmi hodnotami (RGB alebo RGBA)
    * [ ] Hodnoty sú v rozsahu 0 až 255, pričom (0, 0, 0) predstavuje čiernu farbu a (255, 255, 255) predstavuje bielu farbu.
    * [ ] Prvý prvok je červená zložka, druhý zložka zelená a tretí zložka modrá
- [x] Trieda Rect - obdĺžnik
    * [ ] Vytvorenie nového rect objektu robíme pomocou konštruktora `pygame.Rect(x, y, width, height)`
    * [ ] Trieda poskytuje rôzne atribúty ako napr. `x`, `y`, `width`, `height`, ...
    * [ ] Detekcia kolízie pomocou `rect.collidepoint(x, y)` a `rect.colliderect(other_rect)`
- [x] Vykresľovanie tvarov pomocou `pygame.draw`
    * [ ] `pygame.draw.rect(screen, color, rect)` - vykreslí obdĺžnik
    * [ ] `pygame.draw.ellipse(screen, color, rect)` - vykreslí elipsu
    * [ ] `pygame.draw.line(screen, color, start_pos, end_pos, width = 1)` - vykreslí čiaru
- [x] Vykresľovanie textu pomocou `pygame.font`
    * [ ] Najpr je potrebné mať vytvorený font, pomocou `font = pygame.font.SysFont(nazov, size)`
    * [ ] Vykresliť text vieme pomocou `font.render` a potom je potrebné vykreslený text umiestniť na obrazovku pomocou `screen.blit`
- [x] Udalosti - Jednorázové udalosti, napr. kliknutie myšou, stlačenie klávesy, ...
    * [ ] Udalosti je možné zachytiť pomocou modulu `pygame.event`
    * [ ] Udalosti získame pomocou `pygame.event.get()`
- [x] Držanie stlačenej klávesy - pretrvávajúce stavy
    * [ ] Stav stlačenia kláves zistíme pomocou `klavesy = pygame.key.get_pressed()`
- [x] Hlavná herná slučka - hra beží v nekonečnom cykle
    * [ ] Jedna iterácia je jeden tick alebo frame - snímka
    * [ ] Trieda `pygame.time.Clock` umožňuje spomaliť cyklus na požadované FPS
    * [ ] Úloha hlavnej slučky je zachytiť a spracovať udalosti, aktualizovať stav sveta hry a potom vykresliť hru na obrazovku.
    * [ ] Všetko ostatné je potrebné si pripraviť vopred. Je zbytočné vytvárať fonty a načítavať obrázky v každej iterácii slučky, v každom frame


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    PYGAME

    Knižnica na tvorbu 2D hier, https://www.pygame.org

    Inicializácia pomocou pygame.init()
    Okno pomocou pygame.display.set_mode((SIRKA, VYSKA))
    Farby sú reprezentované ako tuple (R, G, B) v intervaloch 0-255
    Trieda Rect - obdĺžnik, pygame.Rect(x, y, width, height)
    Detekcia kolízie pomocou rect.colliderect(other_rect)

    Vykresľovanie tvarov: pygame.draw.rect, pygame.draw.ellipse, pygame.draw.line
    Vykresľovanie textu v 3 krokoch: pygame.font.SysFont, font.render, screen.blit

    Jednorázové udalosti (klik myšou, stlačenie klávesy): pygame.event.get
    Stav klávesnice - pygame.key.get_pressed()

    Hlavná herná slučka - hra beží v nekonečnom cykle
    Jedna iterácia je jeden tick alebo frame - snímka
    pygame.time.Clock umožňuje spomaliť cyklus na požadované FPS
    Úloha hlavnej slučky je zachytiť a spracovať udalosti, 
    aktualizovať stav sveta hry a potom vykresliť hru na obrazovku.
    Všetko ostatné je potrebné si pripraviť vopred. 
    Je zbytočné vytvárať fonty a načítavať obrázky v každej iterácii slučky, v každom frame
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Čo je pygame a na čo slúži
    - Na čo je trieda Rect
    - Čo je hlavná slučka a aké má použitie
    - Čo sú udalosti a ako sa dajú spracovať