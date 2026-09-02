# Záloha školského roku 2025/2026

Snímka materiálov predmetu OOP pred prípravou na **2026/2027**.
Zdroj: vetva `main`, commit `2794115b28f3a3d5ce1e7a6bf5b871a181f407a2`.

Táto zložka ostáva v repozitári, ale **nie je v živej navigácii** webu
(`exclude_docs: backup/**` v `mkdocs.yml`).

## Stav presunu živých hodín (rozpracované)

Cieľ: kompletný markdown t01–t28, c01–c20, p01–p28 pod `teoria-3/`, `cvicenie-3/`, `pokrocili-3/` v tejto zálohe; živá stránka len prvé hodiny.

Už skopírované byte-identicky (zhoda blob SHA so živým súborom):

- `teoria-3/t01-uvod.md` (originál 2025 z main)
- `cvicenie-3/c10-dedicnost.md`, `c16-projekt2.md`, `c17-javafx-property.md`
- `pokrocili-3/p16-projekt.md`, `p26-projekt2.md`

`teoria-3/t02-typy.md`, `t03-objekt.md`, `t04-trieda.md` existujú, ale **nie sú byte-identické** (po 1 preklepe z MCP prepisu). Treba ich prepísať zo živých `docs/teoria-3/`.

Ostatné t02–t28 / c02–c20 / p02–p28 ešte ostávajú na živých cestách. Živé t01/c01/p01 sa nemažú. `prehlad.md` / `prehlad-private.md` ešte nie sú v zálohe.

Slim nav súbory (len 1. hodina) sú pripravené v `.github/tmp-nav/` (`mkdocs.yml`, `prehlad.md`, `prehlad-private.md`).

## Čo už bolo v zálohe predtým

- `index.md`, `osnovy.md`, `osnovy-new.md`, `mkdocs.yml`
- `MANIFEST.txt` — blob SHA z `main` (t01–t28, c01–c20, p01–p28)

## Prezentácie (pptx)

Binárne súbory z `prezentacie/` sem neboli kopírované. Prvé prezentácie ostávajú v `prezentacie/` **bez úpravy**. Celú sadu pptx nájdete v histórii `main` na commite `2794115b`.
