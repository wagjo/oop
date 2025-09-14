# Pokročílí 3: Rekurzia

Táto časť sa venuje rekurzii. Ak chcete viac informácií, nájdete ju na stránke o [rekurzii](https://oop.wagjo.com/pokrocili-3/p03-rekurzia/)

## Iterácia

Iteráciu v programovaní používame dennodenne. Iterácia je proces opakovania určitej operácie pomocou cyklov. Teda cykly `for` a `while` predstavujú iteráciu.

```python title="Výpočet faktoriálu iteratívnym spôsobom"
def faktorial(n):
    vysledok = 1
    for i in range(1, n + 1):
        vysledok *= i
    return vysledok
```

Existujú však typy úloh, kedy použitie iterácie je komplikované, neprehľadné alebo málo elegantné. V takých prípadoch vieme pri určitých typoch úloh použiť rekurziu.

## Rekurzia

Rekurzia je proces, pri ktorom funkcia volá samu seba, aby vyriešila problém rozdelením na menšie podproblémy rovnakého typu.

Rekurzívna funkcia má dve základné časti, prvá je **základný prípad** (base case) a druhá je **rekurzívny prípad** (recursive case). Základný prípad je stav, kedy sa rekurzia zastaví, a predstavuje nejaký konečný stav úlohy. Rekurzívny prípad je stav, kedy funkcia v určitom bode zavolá samú seba.

Ak by rekurzívna funkcia nemala základný - bázový prípad, pokračovala by donekonečna, resp. dovtedy, kým by nezaplnila pamäť počítača.

![](../assets/base_case_meme.jpg){width=500 .on-glb}
/// caption
Anakin programátor
///


```python title="Výpočet faktoriálu rekurzívnym spôsobom"
def faktorial(n):
    if n == 0:  # Bázový prípad
        return 1
    return n * faktorial(n - 1)  # Rekurzívny prípad
```

## Porovnanie rekurzia a iterácie

Každý algoritmus v rekurzívnom tvare sa dá prepísať na tvar iteratívny. Iteratívna verzia je obvykle efektívnejšia - je rýchlejšia a zaberá menej pamäti. Rekurziu používame hlavne kôli jej elegancii a jednoduchosti pri niektorých prípadoch.

| Vlastnosť | Rekurzia | Iterácia |
|-------------|-----------|-----------|
| Princíp | Funkcia volá samu seba | Opakovanie pomocou cyklov |
| Pamäť | Vyššia spotreba (zásobník) | Nižšia spotreba |
| Rýchlosť | Pomalšia (opakované volania) | Rýchlejšia |
| Čitateľnosť | Eleganantná pre zložité problémy |Jednoduchšia pre lineárne úlohy |
| Riziko | Pretečenie zásobníka pri hlbokej rekurzii | Žiadne také riziko |
| Použitie | Stromové štruktúry, divide-and-conquer | Lineárne opakovanie, jednoduché úlohy |

## Zásobník volaní

<div class="md-has-sidebar" markdown>
<main markdown>

Rekurzia, aj keď má bázový prípad, môže pri veľkom počte volaní - vnorení - zaplniť tzv. zásobník volaní bežiaceho programu.

Zásobník (stack) je dátová štruktúra typu LIFO *(Last In, First Out)*, kde sa prvky pridávajú *(push)* a odstraňujú *(pop)* z vrcholu zásobníka. Python interne spravuje volania funkcií pomocou zásobníka volaní *(call stack)*.

Keď sa zavolá funkcia:

- Python vytvorí nový rámec volania *(call frame)* na zásobníku, ktorý obsahuje lokálne premenné funkcie a parametre funkcie.
- Po dokončení funkcie sa jej rámec odstráni zo zásobníka *(pop)* a program pokračuje tam, kde skončil.

Rekurzia úzko súvisí so zásobníkom, pretože každé rekurzívne volanie funkcie vytvára nový rámec na zásobníku volaní. To umožňuje uchovať stav každého volania (lokálne premenné, parametre) až do dosiahnutia bázového prípadu.

No a pri veľkom počte rekurzívnych volaní (napr. `faktorial(10000)`) môže dôjsť k pretečeniu zásobníka (stack overflow), pretože každý rámec zaberá pamäť.
 </main>
  <aside markdown>
Spadnutie programu v dôsledku pretečenia zásobníka je veľmi častá chyba programu. Dokonca taká častá, že si ju za svoj názov zobrala asi najznámejšia stránka pre programátorov, [Stack Overflow](https://stackoverflow.com)  </aside>
</div>
Python má predvolený limit rekurzie *(zvyčajne 1000 volaní)*, ktorý sa dá upraviť pomocou funkcie `sys.setrecursionlimit()`.

```python title="Zmena limitu rekurzie"
import sys
print(sys.getrecursionlimit())  # štandardne 1000
sys.setrecursionlimit(2000)     # limit vieme zvýšiť, ale hrozí zaplnenie pamäti
```

!!! info "Tail Call Optimization"

    Veľa programovacích jazykov podporuje optimalizáciu rekurzie v prípadoch, kedy je rekurzívne volanie poslednou vecou vo funkcii. Pri takejto optimalizícii by sa do zásobníka neuložili nové hodnoty, ale by sa ibe prepísali tie existujúce. Jazyk Python však bohužiaľ túto optimalizáciu nepodporuje.


## Úlohy na precvičenie

Pri nasledujúcich úlohách si zmerajte rýchlosť rekurzívnej a iteratívnej verzie pomocou knižnice `timeit`.

```python title="Meranie rýchlosti funkcie pomocou timeit"
import timeit

def faktorial(n):
    if n <= 1:
        return 1
    return n * faktorial(n - 1)

opakovania = 1000

cas = timeit.timeit(
    stmt='faktorial(100)',  # Kód, ktorý meriame
    setup='from __main__ import faktorial',
    number=opakovania
)

print(f"Čas funkcie: {cas:.6f} sekúnd ({opakovania} opakovaní)")
```

!!! example "Úloha 3.1: Fibonacciho postupnosť"

    Vytvorte program na vypis n-tého prvku Fibonacciho postupnosti pomocou rekurzívnej a iteratívnej metódy

!!! example "Úloha 3.2: Mocnina"

    Napíš rekurzívnu funkciu, ktorá vypočíta a^n^ (mocninu čísla a na n). Ako by vyzerala iteratívna verzia?

!!! example "Úloha 3.3: Hanojské veže"

    Napíš rekurzívnu funkciu, ktorá vyrieši problém Hanojských veží pre n diskov. Funkcia vypíše kroky na presun diskov z jedného kolíka (A) na druhý (C) s použitím pomocného kolíka (B).

!!! example "Úloha 3.4: Obrátenie reťazca"

    Napíš rekurzívnu funkciu, ktorá obráti zadaný reťazec (napr. "ahoj" → "joha").

!!! example "Úloha 3.5: Kontrola palindrómu"

    Rekurzívne zisti, či je reťazec palindróm (čítaný spredu aj odzadu je rovnaký).


## Zhrnutie cvičenia

- [x] Iterácia je proces opakovania určitej operácie pomocou cyklov
- [x] Rekurzia je proces, pri ktorom funkcia volá samu seba, aby vyriešila problém rozdelením na menšie podproblémy rovnakého typu
- [x] Rekurzívna funkcia má dve základné časti, prvá je základný prípad a druhá je rekurzívny prípad
    * [ ] Základný prípad je stav, kedy sa rekurzia zastaví, a predstavuje nejaký konečný stav úlohy
    * [ ] Rekurzívny prípad je stav, kedy funkcia v určitom bode zavolá samú seba
- [x] Rekurzia môže byť elegantnejšia, iterácia je väčšinou efektívnejšia
- [x] Zásobník
    * [ ] Rekurzia pri veľkom počte vnorení zaplní zásobník
    * [ ] Python má predvolený limit rekurzie (zvyčajne 1000 volaní), ktorý sa dá upraviť pomocou funkcie `sys.setrecursionlimit()`


!!! note "Poznámky do zošita"
    V zošite je potrebné mať napísané aspoň tieto poznámky:

    ```
    Iterácia - používanie cyklov
    Rekurzia - volanie samého seba

    2 časti rekurzívnej funkcie
    - základný prípad - kedy sa rekurzia zastaví
    - rekurzívny prípad - keď volá samú seba

    Rekurzia môže byť elegantnejšia, iterácia je väčšinou efektívnejšia

    Rekurzia pri veľkom počte vnorení zaplní zásobník, nastane stack overflow

    Python má limit na rekurziu, mení sa pomocu sys.setrecursionlimit()
    ```

!!! warning "Skúšanie a kontrola vedomostí"

    Ústne skúšanie alebo krátka 5-minútovka:

    - Rozdiel medzi rekurziou a iteráciou
    - 2 časti rekurzívnej funkcie
    - Čo je zásobník volaní, ako súvisí s rekurziou

