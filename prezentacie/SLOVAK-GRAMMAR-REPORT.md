# Slovak grammar survey — prezentacie/*.pptx (wagjo/oop main)

Scanned **63** pptx files under `/workspace/oop-slides/raw`.
Patterns aligned with `/workspace/oop-grammar/REPORT.md`.

## Summary

- **Files with issues:** 6
- **Total clear issues:** 7
- **Files with ZERO issues:** 57
- **Fixed files written to:** `/workspace/oop-slides/fixed/` (6)

## Files with issues

### `c08w.pptx` — 2 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 6 | `ináč` | **inak** | í ako 1 alebo ak sme vyhrali, ináč vráti false public int getOst |
| 6 | `ináč` | **inak** | o vráti, iba ak hra skončila, ináč vyhodí výnimku public String |

### `p09w.pptx` — 1 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 7 | `žiaden` | **žiadny** | nované v triede, ktoré nemajú žiaden prístup ani k objektu ani ku |

### `t03w.pptx` — 1 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 3 | `dátove` | **dátové** | , identita ostáva  Primitívne dátove typy v Jave majú hodnoty bez |

### `t05w.pptx` — 1 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 4 | `ináč` | **inak** | chyby, ktoré musíme ošetriť, ináč sa program ani neskompiluje a |

### `t08w.pptx` — 1 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 8 | `žiaden` | **žiadny** | argumentov Ak nemáme napísaný žiaden konštruktor, Java automaticky |

### `t27w.pptx` — 1 issue(s)

| Slide | Match | Fix | Context |
|-------|-------|-----|---------|
| 16 | `žiaden` | **žiadny** | lačná databáza. Nepotrebujeme žiaden samostatný server, ale databá |

## Fix verification

| File | Before | Fix ops | Remaining | Opens OK |
|------|--------|---------|-----------|----------|
| `c08w.pptx` | 2 | 2 | 0 | True |
| `p09w.pptx` | 1 | 1 | 0 | True |
| `t03w.pptx` | 1 | 1 | 0 | True |
| `t05w.pptx` | 1 | 1 | 0 | True |
| `t08w.pptx` | 1 | 1 | 0 | True |
| `t27w.pptx` | 1 | 1 | 0 | True |

## Files with ZERO issues

- `c01w.pptx`
- `c02w.pptx`
- `c03w.pptx`
- `c04w.pptx`
- `c05w.pptx`
- `c06w.pptx`
- `c07w.pptx`
- `c09w.pptx`
- `c19w.pptx`
- `p01w.pptx`
- `p02w.pptx`
- `p03w.pptx`
- `p04w.pptx`
- `p05w.pptx`
- `p06w.pptx`
- `p07w.pptx`
- `p08w.pptx`
- `p10w.pptx`
- `p11w.pptx`
- `p12w.pptx`
- `p13w.pptx`
- `p14w.pptx`
- `p15w.pptx`
- `p18w.pptx`
- `p19w.pptx`
- `p20w.pptx`
- `p21w.pptx`
- `p22w.pptx`
- `p23w.pptx`
- `p24w.pptx`
- `p25w.pptx`
- `p27w.pptx`
- `p28w.pptx`
- `t01w.pptx`
- `t02w.pptx`
- `t04w.pptx`
- `t06w.pptx`
- `t07w.pptx`
- `t09w.pptx`
- `t10w.pptx`
- `t11w.pptx`
- `t12w.pptx`
- `t13w.pptx`
- `t14w.pptx`
- `t15w.pptx`
- `t16w.pptx`
- `t17w.pptx`
- `t18w.pptx`
- `t19w.pptx`
- `t20w.pptx`
- `t21w.pptx`
- `t22w.pptx`
- `t23w.pptx`
- `t24w.pptx`
- `t25w.pptx`
- `t26w.pptx`
- `t28w.pptx`

## Binary upload blocker (GitHub MCP)

`user-Github` `create_or_update_file` / `push_files` accept a **UTF-8 text** `content` string and base64-encode it server-side for the Contents API.

Probe on branch `fix-slovak-grammar-prezentacie`:
- Uploaded base64 of a 25-byte binary as `content`
- GitHub blob **size=36** (= length of the base64 ASCII string), not 25
- Conclusion: passing base64 double-encodes / stores ASCII; raw pptx bytes (ZIP, nulls, non-UTF-8) cannot be uploaded intact via these MCP tools

No GitHub token is available in the box shell for a direct Contents/Git Data API binary PUT. Cloud Agents unavailable. Local desktop forbidden for this task.

### Fixed binaries ready on box (not on GitHub)
| File | Issues fixed | Local path | Local size |
|------|--------------|------------|------------|
| `c08w.pptx` | 2× `ináč`→`inak` | `/workspace/oop-slides/fixed/c08w.pptx` | 209199 |
| `p09w.pptx` | `žiaden`→`žiadny` | `/workspace/oop-slides/fixed/p09w.pptx` | 46355 |
| `t03w.pptx` | `dátove`→`dátové` | `/workspace/oop-slides/fixed/t03w.pptx` | 42904 |
| `t05w.pptx` | `ináč`→`inak` | `/workspace/oop-slides/fixed/t05w.pptx` | 136037 |
| `t08w.pptx` | `žiaden`→`žiadny` | `/workspace/oop-slides/fixed/t08w.pptx` | 52561 |
| `t27w.pptx` | `žiaden`→`žiadny` | `/workspace/oop-slides/fixed/t27w.pptx` | 61419 |

All six re-open with python-pptx; rescan remaining matches = 0.
