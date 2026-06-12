# Context del projecte: monorepo AFA Escola La Draga

## Què és això
Repositori únic de l'AFA de l'Escola La Draga (Banyoles) amb el web, les skills de
Claude Code i les presentacions.

## Regla d'or: una sola font de veritat
- **Dades factuals** (qui som, comissions i qui les forma, contacte, dades
  recurrents): viuen NOMÉS al `web/`. No les dupliquis enlloc. Les skills les
  llegeixen del web.
- **Estil i to:** viu NOMÉS a `.claude/skills/_base/to-de-veu.md`.

## Estructura
- `web/` — web estàtic (HTML pur, Zero JS, Tailwind). Es desplega a GitHub Pages
  des d'aquesta carpeta via `.github/workflows/pages.yml`.
- `.claude/skills/` — skills: `comunicat-socis`, `acta-reunio`, `presentacio`,
  i la referència compartida `_base/to-de-veu.md`.
- `presentacions/` — presentacions Marp.

## Regles del web (heretades)
- Zero JS al client.
- Nomenclatura de fitxers a `web/fitxers/`: `YYYYMMDD_titol-del-fitxer.pdf`.
- Actes noves: afegir al capdamunt de la llista `<ul class="acta-list">` de `web/afa.html` (ordre cronològic invers).

## Redacció
- Català, frases curtes i clares, llenguatge planer (accessible per a infants).
