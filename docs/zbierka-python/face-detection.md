# Rozpoznanie tváre pomocou AI v Pythone

Vyskúšame si aplikáciu, ktorá pomocou AI rozpozná tvár a aj tzv. sentiment (náladu).

Budeme používať nástroj [Face Landmarker](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker) ktorý je súčasťou projektu [Media Pipe](https://developers.google.com/edge/mediapipe/solutions) spoločnosti Google.


!!! example "Úloha 1:"

    * Naklonujte si projekt z adresy [https://github.com/wagjo/smajl.git](https://github.com/wagjo/smajl.git)
    * Nainštalujte závislosti pomopcou `pip install -e .`
    * Spustite program pomocou `python -m smajl`

Kód je v súbore smajl.py. prezrite si ho.

!!! example "Úloha 2:"

    * Program rozpoznáva stavy happy, normal a sad. Klasifikácia stavu je vo funkcii `classify_emotion`.
    * Skúste program upraviť tak, aby sa rozpoznala aj emócia prekvapenia, alebo nejaká iná

!!! example "Úloha 3:"

    Projekt obsahuje aj detekciu ruky a 'giest'. Spustite to pomocou `python -m ruka`.

    Vyskúšajte detegovať všetky podporované gestá.

!!! example "Úloha 4:"

    * Skúste pridať do programu nový druh gesta, napr. "High five", "OK Sign", "Shoot", počítanie prstami alebo iné.

    * Skúste pomocou LLM pridať do programu podporu druhej ruky a zobrazte gestá oboch rúk.

    * Pridajte podporu pre gestá z oboma rukami, napr. "Srdce" alebo "V".
    