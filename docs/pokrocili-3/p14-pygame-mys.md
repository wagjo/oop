# Pokročílí 14: Pygame 2

Pokračujeme v PyGame, dnes si vyskúšame ovládanie myšou a vykresľovanie obrázkov

## Spracovanie myši

Podobne ako pri klávesnici, vstupy z myši máme buď jednorázové - **eventy**, alebo máme **stavy**, v ktorých sa myš nachádza (tlačítko je stlačené, myš sa nachádza na pozícii X, Y, ...)

Udalosti si vieme odchytiť pomocou `pygame.event.get()`, tak ako pri klávesnici.

Stav myši vieme zistiť pomocou `pygame.mouse.get_pressed()` alebo `pygame.mouse.get_pos()`.

Nasledujúci príklad ukazuje príklady použitia:

=== "Spracovanie myši"

    ```python
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            # === UDALOSTI MYŠI ===
            if event.type == pygame.MOUSEBUTTONDOWN:    # stlačenie tlačidla
                print("Myš stlačená!", event.button, "na pozícii", event.pos)

            if event.type == pygame.MOUSEBUTTONUP:      # pustenie tlačidla
                print("Myš pustená!", event.button)

            if event.type == pygame.MOUSEWHEEL:         # koliesko
                print("Koliesko otočené:", event.y)     # +1 hore, -1 dole

        # Aktuálna pozícia myši (každý frame)
        mys_x, mys_y = pygame.mouse.get_pos()

        # Ktoré tlačidlá sú práve stlačené (ľavé, stredné, pravé)
        tlacidla = pygame.mouse.get_pressed()

        # tlacidla je tuple: (ľavé, stredné, pravé)
        if tlacidla[0]:    # ľavé tlačidlo držané
            print(f"Držím ľavé tlačidlo na [{mys_x}, {mys_y}]")

        clock.tick(60)
    ```

## Tlačítka

V Pygame vieme použiť `Rect` ako jednoduché tlačítka - buttons, na ovládanie aplikácie. Postup je nasledovný

- Vykreslíme Rect na pozícii, kde sa má tlačítko nachádzať
- Vypíšeme label tlačítka
- V hernej slučke budeme sledovať kliknutia myšou. Ak bolo kliknuté na pozícii tlačítka, vykonáme akciu

=== "Tlačítka v pythone"

    ```python
    button_rect = pygame.Rect(POZICIA)
    button_label = "..."

    pygame.draw.rect(screen, FARBA_POZADIA, button_rect)
    btn_text = font.render(button_label, True, FARBA_TEXTU)
    screen.blit(btn_text, POZICIA)

    ...

    if event.type == pygame.MOUSEBUTTONDOWN:
    if event.button == 1:  # Left click
        if button_rect.collidepoint(event.pos):
            button_action()
    ```

Ak máme tlačítiek viacero, je vhodné vytvoriť samostatnú triedu pre tlačítko.

## Vykresľovanie obrázku

Prácu s obrázkami má na starosti modul `pygame.image`. Pygame podporuje všetky štandardné formáty ako GIF, JPEG, PNG a WEBP.

Obrázok si viem načítať pomocou metódy `image = pygame.image.load("filename")`

Po načítaní vieme zmeniť veľkosť obrázka pomocou `image = pygame.transform.scale(image, (sirka, vyska))`

Samotné vykreslenie obrázku na obrazovku sa realizuje pomocou `screen.blit(image, rect)`

Nie je vhodné obrázok načítavať každý frame. Obrázok je potrebné načítať raz a pamätať si načítané dáta. Vo frame sa už obrázok iba vykreslí.


## Úlohy na hodine

Dnes si pre našu adventúru vytvoríme grafické rozhranie. Postup je nasledovný

- Do assets pridáme novú mapu a sadu obrázkov miestností
- Upravíme triedu Room o nový atribút s obrázkom
- Upravíme akciu use, aby sa v miestnosti mohol meniť obrázok
- Vytvoríme novú triedu PGEngine s grafickým enginom

Ak nemáte predchádzajúcu verziu hry adventura, môžete si ju stiahnuť pomocou `git clone https://github.com/wagjo/adventura-text.git`

!!! example "Úloha 14.1: Nová mapa a obrázky"

    1. Do adresára `assets` si stiahnite novú verziu mapy [hra12.json](http://oop.wagjo.com/assets/hra14.json)

    1. Stiahnite si súbor [rooms.zip](http://oop.wagjo.com/assets/rooms.zip) a rozbaľte jeho obsah do adresára `assets`

    1. V súbore `__main__.py` zmeňte mapu z `hra12.json` na `hra14.json`

    1. Pozrite si obsah súboru s mapou a všimnite si nový atribút `image`

!!! example "Úloha 14.2: Pridanie atribútu s obrázkom"

    1. Do triedy `Room` pridajte nový atribút `_image`, v konštruktore pridajte nový parameter s defaultnou hodnotou `image=None`

    1. Vytvorte getter metódu pre atribút `_image`.

    1. Vytvorte metódu `replace_image`, ktorá nahradí image novou hodnotou, podobne ako je to v metóde `replace_desc`

    1. V triede `Hra` upravte metódu `_replace`, aby podporovala aj nahradenie atribútu `image`, podobne ako to robí pre atribút `desc`


!!! example "Úloha 14.3: Trieda Button"

    Začneme pracovať na grafickom engine. Vytvoríme si triedu Button, ktorá bude reprezentovať tlačítko

    1. Vytvorte modul `pgengine.py` a v ňom vytvorte triedu `Button`

    1. Trieda Button nech má 3 atribúty: `_rect`, `_label` a `_command`. Všetky tieto atribúty bude trieda načítavať v konštruktore.

    1. Vytvorte getter metódy pre atribúty `_rect` a `_command`

    1. Do triedy vložte metódu `draw`:

        ```python
        def draw(self, screen, font, zoom):
            pygame.draw.rect(screen, (250, 250, 200), self._rect)
            btn_text = font.render(self._label, True, (0, 0, 0))
            screen.blit(btn_text, (self._rect.x + 8 * zoom, self._rect.y + 13 * zoom))
        ```

!!! example "Úloha 14.4: Trieda PGEngine"

    1. V module `pgengine` vytvorte triedu `PGEngine` s nasledovným kódom

        ```python
        class PGEngine:
            def __init__(self, hra, intro, outro):
                self._hra = hra
                self._intro = intro
                self._outro = outro
                self._message = intro

                self._zoom = 2
                self.WIDTH, self.HEIGHT = 1200 * self._zoom, 800 * self._zoom
                self._images = {}
                self.ASSETS_DIR = "assets"

                pygame.init()
                self._screen = pygame.display.set_mode((self.WIDTH, 560 * self._zoom))
                pygame.display.set_caption("Adventura")
                self._clock = pygame.time.Clock()
                self._font = pygame.font.SysFont(None, 36 * self._zoom)
                self._buttons = self.create_main_buttons()

            def get_image(self, filename):
                if self._images.get(filename):
                    return self._images[filename]
                else:
                    image = pygame.image.load(path.join(self.ASSETS_DIR,filename))
                    image = pygame.transform.scale(image, (self.WIDTH // 2, self.HEIGHT // 2))
                    self._images[filename] = image
                    return image

            def get_active_image(self):
                room = self._hra.active_room()
                image = room.image
                return self.get_image(image)

            def draw_row(self, text, color, pos):
                rendered = self._font.render(text, True, color)
                self._screen.blit(rendered, ( 20 * self._zoom, pos))

            def draw_image(self):
                self._screen.fill((30, 30, 20))
                image = self.get_active_image()
                self._screen.blit(image, (self.WIDTH // 2, 0))

            def draw_buttons(self):
                for btn in self._buttons:
                    btn.draw(self._screen, self._font, self._zoom)

            def draw_text(self):
                room = self._hra.active_room()
                player = self._hra.player

                # Header
                header = "Miesto: " + room.label
                offset = 20 * self._zoom
                self.draw_row(header, (50, 255, 50), offset)

                # Description
                offset += 60 * self._zoom
                desc = textwrap.wrap(room.desc, width=46)
                for s in desc:
                    self.draw_row(s, (255, 255, 255), offset)
                    offset += 30 * self._zoom
                offset += 30 * self._zoom

                # Footer
                footer = ["Môžeš ísť: " + ", ".join(room.exits),
                        "Na zemi sa nachádza: " + ", ".join(room.items),
                        "V inventári máš: " + ", ".join(player.inventory)]
                for s in footer:
                    self.draw_row(s, (100, 100, 255), offset)
                    offset += 30 * self._zoom

                # Message
                self.draw_row(self._message, (255, 50, 50), 430 * self._zoom)

            def create_buttons(self, labels, commands):
                buttons = []
                for i, label, command in zip(range(len(labels)), labels, commands):
                    rect = pygame.Rect((20 + 180 * i) * self._zoom, 480 * self._zoom,
                                    160 * self._zoom, 50 * self._zoom)
                    button = Button(rect, label, command)
                    buttons.append(button)
                return buttons

            def create_main_buttons(self):
                return self.create_buttons(["Choď...", "Vezmi...", "Použi..."],
                                        ["menu go", "menu take", "menu use"])

            def create_go_buttons(self):
                choices = self._hra.active_room().exits
                labels = choices + ["Menu..."]
                commands = ["go " + label for label in choices] + ["menu main"]
                return self.create_buttons(labels, commands)

            def create_end_buttons(self):
                return self.create_buttons(["GAME OVER"], ["menu end"])

            def switch_menu(self, menu):
                if menu == "main":
                    self._buttons = self.create_main_buttons()
                elif menu == "go":
                    self._buttons = self.create_go_buttons()
                elif menu == "end":
                    self._buttons = self.create_end_buttons()

            def action(self, command):
                verb, noun = command.split()
                desc = self._message
                if verb == "menu":
                    self.switch_menu(noun)
                    desc = "" if noun != "end" else desc
                elif verb == "go":
                    self.switch_menu("main")
                    self._hra.go(noun)
                    desc = "Išiel si smerom: " + noun
                elif verb == "take":
                    self.switch_menu("main")
                    self._hra.take_item(noun)
                    desc = "Zo zeme si zodvihol: " + noun
                elif verb == "use":
                    self.switch_menu("main")
                    try:
                        desc = self._hra.use_item(noun)
                    except Exception as ex:
                        desc = "Na tomto mieste túto vec neviem použiť"
                room = self._hra.active_room()
                if room.game_over:
                    self.switch_menu("end")
                    desc = room.game_over
                self._message = desc
        ```

    1. V triede `PGEngine` napíšte triedne metódy `from_dict` a `load_game` podobne ako to máme v triede `Engine`

    1. Vytvorte metódy `create_take_buttons` a `create_use_buttons` s podobnou logikou ako má metóda `create_go_buttons` len s tým rozdielom, že budú vytvárať tlačitká na základe hodnôt v atribútoch `active_room().items` resp. `player.inventory` a
    namiesto príkazov `go` budú vytvárať príkazy typu `take` a `use`.

    1. Doplňte metódu `switch_menu` o nové elif vetvy, ktoré budú mať na starosti `take` a `use` menu.

    1. Vytvorte metódu `play(self)` s hlavnou slučkou hry. V slučke vykonajte nasledovné veci:

        - Ošetrenie vstupov nasledovným kódom:

            ```python
            # Handle mouse clicks
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    sys.exit()
                if event.type == pygame.MOUSEBUTTONDOWN:
                    if event.button == 1:  # Left click
                        for btn in self._buttons:
                            if btn.rect.collidepoint(event.pos):
                                self.action(btn.command)
            ```
        - Vykreslenie obrázku pomocou `draw_image()`
        - Vypísanie textu pomodou `draw_text()`
        - Vykreslenie tlačidiel pomocou `draw_button()`
        - Vyrenderovanie na obrazovku pomocou `flip` a nastavenie FPS pomocou `tick` (podobne ako v hre Pong)

!!! example "Úloha 14.5: Spustenie hry"

    V module `__main__` upravíme spustenie hry tak, aby sa spúšťal `PGEngine` namiesto `Engine`

    Hra je hotová, veľa šťastia pri jej hraní


## Úlohy na precvičenie

!!! example "Úloha 14.6: Look a inventár"
    
    Ak ste v textovej verzii hry mali naimplementované aj príkazy look a inventory, skúste ich teraz naprogramovať do grafického enginu.

!!! example "Úloha 14.7: Prehľadnejšie UI"
    
    Ak hráč nemá nič v inventári alebo nie je v miestnosti žiaden predmet, nevypisujte zbytočne na obrazovku text "V miestnosti sa nachádza:" a pod.

!!! example "Úloha 14.8: Zreteľnejšie zmeny"
    
    Ak sa niečo v miestností zmení, text sa rýchlo upraví a nie je zrejmé, čo sa zmenilo. Podobne aj hláška červenou farbou sa niekedy zmení bez povšimnutia. 

    Naimplementujte prebliknutie, teda ak nastane znema, na pár desiatok frame-ov nechajte obrazovku čiernu a až potom ukážte nový text. Hráč si tak uvedomí, že sa niečo zmenilo.

    Podobne urobte aj pre text, ktorý je vypisovaný červeným písmom.

!!! example "Úloha 14.9: Reštart hry"
    
    Ak nastane koniec hry pridajte tlačítko "Nová hra", ktoré spustí hru znova

!!! example "Úloha 14.10: Save/Load"
    
    Naimplementujte Uloženie hry na disk a načítanie hry zo súboru. Môžete to urobiť pomocou naimplementovania metódy `__repr__` pre triedy `Player`, `Room` a `Hra`, alebo naimplementujte metódy `to_json` a hru uložte fo formáte `JSON`

## Zhrnutie cvičenia

- [x] Spracovanie myši
    * [ ] Podobne ako pri klávesnici, vstupy z myši máme buď jednorázové - eventy, alebo máme stavy, v ktorých sa myš nachádza (tlačítko je stlačené, mýš sa nachádza na pozícii X, Y, ...)
    * [ ] Udalosti si vieme odchytiť pomocou `pygame.event.get()`, tak ako pri klávesnici.
    * [ ] Stav myši vieme zistiť pomocou `pygame.mouse.get_pressed()` alebo `pygame.mouse.get_pos()`
- [x] Tlačítka
    * [ ] V Pygame vieme použiť Rect ako jednoduché tlačítka - buttons, na ovládanie aplikácie. Postup je nasledovný
    * [ ] 1. Vykreslíme Rect na pozícii, kde sa má tlačítko nachádzať
    * [ ] 2. Vypíšeme label tlačítka
    * [ ] 3. V hernej slučke budeme sledovať kliknutia myšou. Ak bolo kliknuté na pozícii tlačítka, vykonáme akciu
- [x] Vykresľovanie obrázku
    * [ ] Prácu s obrázkami má na starosti modul `pygame.image`. Pygame podporuje všetky štandardné formáty ako GIF, JPEG, PNG a WEBP.
    * [ ] Obrázok si viem načítať pomocou metódy `image = pygame.image.load("filename")`
    * [ ] Po načítaní vieme zmeniť veľkosť obrázka pomocou `image = pygame.transform.scale(image, (sirka, vyska))`
    * [ ] Samotné vykreslenie obrázku na obrazovku sa realizuje pomocou `screen.blit(image, rect)`
    * [ ] Nie je vhodné obrázok načítavať každý frame. Obrázok je potrebné načítať raz a pamätať si načítané dáta. Vo frame sa už obrázok iba vykreslí.


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    PYGAME

    Spracovanie myši podobne ako pri klávesnici, vstupy máme buď jednorázové - eventy, 
    alebo máme stavy (poloha, ktprá tlačítka sú práve stlačené, ...)

    Stav myši vieme zistiť pomocou pygame.mouse.get_pressed() alebo pygame.mouse.get_pos()

    V Pygame vieme použiť Rect ako jednoduché tlačítka - buttons, na ovládanie aplikácie

    Vykresľovanie obrázku v 3 krokoch: pygame.image.load, pygame.transform.scale, screen.blit

    Je vhodné obrázok načítať raz a potom vo frame už iba vykresľovať naťtítaný obrázok
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Okruhy otázok na test:

    - Spracovanie vstupov z myši
    - Tlačítko - button
    - Vykresľovanie obrázkov