# Použitie LLM v aplikácii

Vyskúšame si vytvoriť aplikáciu, ktorá bude využívať LLM.

Cieľom je urobiť program, ktorý na základe zadaného príbehu vráti zoznam top 5 filmov, ktoré majú daný príbeh. Keďže sme ešte nebrali ako sa robí grafické rozhranie, naša aplikácia bude konzolová.

## API prístup ku LLM

Na volanie LLM z počítačového programu potrebujeme použiť tzv. API daného LLM. Existuje veľké množstvo služieb, ktoré nám poskytujú prístup k LLM API, my si dnes vyskúšame službu Groq.

!!! example "Úloha 1:"

    * Zaregistrujte sa na stránke ([https://console.groq.com](https://console.groq.com/home))
    * Prihláste sa do služby Groq a choďte na stránku s API kľúčmi [https://console.groq.com/keys](https://console.groq.com/keys)
    * Vytvorte nový kľúč s názvom filmy a s expiráciou 30 dní
    * skopírujte si zadaný kľúč. Pozor, ak si ho teraz neskopírujete, už ho viac nezískate! Klúč vyzerá napr. takto: `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`


### Projekt Filmárik

Vytvoríme si nový projekt

!!! example "Úloha 2:"

    Vytvorte nový projekt s názvom "**filmarik**"

    Vytvorte v ňom súbor `pyproject.toml` s týmto obsahom:

    ```toml
    [project]
    name = "filmarik"
    version = "0.0.1"
    dependencies = [
        "groq",
    ]
    ```

    Vytvorte adresár src a v ňom balík `filmarik`. V balíku vytvorte súbory `__init__.py` a `__main__.py`. Potom v balíku filmarik ešte vytvorte súbor `llm.py`. Výsledná štruktúra by mala vyzerať nasledovne:

    ```
    filmarik
     ┃
     ┠── src
     ┃    ┖── filmarik
     ┃         ┠── __init__.py
     ┃         ┠── __main__.py
     ┃         ┖── llm.py
     ┖── pyproject.toml
    ```

!!! example "Úloha 3:"

    Do súboru `llm.py` vložte nasledovný kód:

    ```python
    from groq import Groq

    def vyber_model(client):
        models = sorted(m.id for m in client.models.list().data)

        print("Dostupné modely:")
        for i, model_id in enumerate(models, start=1):
            print(f"  {i:2}. {model_id}")

        raw = input("Číslo modelu: ").strip()
        return models[int(raw) - 1]

    def zavolaj_llm(client, model, system, user):
        completion = client.chat.completions.create(
            model=model,
            temperature=0.3,
            max_completion_tokens=8000,
            messages=[
                {"role": "system", "content": system},
                {"role": "user", "content": user},
            ],
        )
        return (completion.choices[0].message.content or "").strip()
    ```

!!! example "Úloha 4:"

    Do súboru `__main__.py` vložte nasledovný kód:

    ```python
    from groq import Groq
    from . import llm

    KEY = " ... tu vložte svoj GROQ API kľúč ... "
    prompt = """Si filmový expert. Používateľ opíše DEJ filmu (nie názov).
    Nájdi 5 filmov, ktorých dej je tomuto opisu najbližší.
    Odpovedz PO SLOVENSKY v tomto formáte:

    1. Názov (rok) — prečo sedí
    2. ...
    3. ...
    4. ...
    5. ...

    Len týchto 5 bodov. Žiadny úvod, žiadny záver.
    Ak opis sedí na konkrétny slávny film, daj ho na 1. miesto
    a ďalšie 4 nech sú podobné (žáner, motív, twist).
    """

    def napis_dej():
        print()
        print("Napíš dej filmu (viac riadkov). Prázdny riadok = odoslať.")
        print("-------------------------------------------------------")

        lines = []
        while True:
            line = input()
            if line == "":
                break
            lines.append(line)

        dej = "\n".join(lines).strip()
        if not dej:
            print("Nič si nenapísal.")
        return dej

    def hadaj_film(client, model):
        dej = napis_dej()
        if not dej:
            return False
        print("\nHľadám 5 najbližších filmov...\n")
        vysledok = llm.zavolaj_llm(client, model, prompt, dej)
        print(vysledok)
        return True

    def main():
        client = Groq(api_key=KEY)
        model = llm.vyber_model(client)
        print(f"Vybral si si {model}")
        while hadaj_film(client, model):
            pass

    if __name__ == "__main__":
        main()
    ```

    * Do súboru na vyznačené miesto vložte svoj GROK API kľúč
    * Spustite terminál a nainštalujte závislosti pomocou `pip install -e .`
    * Program spustite pomocou `python -m filmarik`

### Príklady

Príklady dejov, ktoré môžete vyskúšať:

* Muz sa zobudi a zisti, ze ostal sam na svete. Okrem neho su na svete iba zombici. Postupne odhali, co sa stalo a zachrani svet.
* Hlavny hrdina cestuje naspat v case, aby zachranil svet
* Najhorsi film na svete
* Hrozi zanik zeme dopadom asteroidu. Odvazni astronauti sa vyberu na nebezpecnu cestu, aby tomu zabranili
* Film ktory sa paci zenam a na ktory by ste zobrali dievca na prvom rande
* Film ktory sa paci chlapcom a na ktory by ste zobrali chlapca na prvom rande
* Stredoveki dobrodruhovia sa plavia na lodi a zazivaju vela dobrodruzstiev a stretnu aj piratov
* Film, ktory vyzera ze dopadne dobre, ale nakoniec skonci velmi smutne a zle
