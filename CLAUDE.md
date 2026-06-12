# Context del projecte: monorepo AFA Escola La Draga

## Què és això
Repositori únic de l'AFA de l'Escola La Draga (Banyoles) amb el web, les skills de
Claude Code i les presentacions.

## Regla d'or: una sola font de veritat
- **Dades factuals** (qui som, comissions i qui les forma, contacte, dades
  recurrents): viuen NOMÉS al `web/`. No les dupliquis enlloc. Les skills les
  llegeixen del web.
- **To i llengua:** viuen NOMÉS a la skill `veu-afa`.
- **Identitat visual:** viu NOMÉS a `web/assets/styles.css` (skill `identitat-afa`
  hi apunta). No copiïs colors; reflecteix-los.

## Estructura
- `web/` — web estàtic (HTML pur, Zero JS, Tailwind). Es desplega a GitHub Pages
  des d'aquesta carpeta via `.github/workflows/pages.yml`.
- `.claude/skills/` — dues capes:
  - **Substrat** (genèric, reutilitzable): `veu-afa` (to i llengua),
    `identitat-afa` (identitat visual).
  - **Perfils** (prims, per lliurament): `comunicat` (WhatsApp), `acta`
    (resum de reunió per comissions), `presentacio` (Marp), `contingut-web`
    (afegir/actualitzar contingut al web). Cada perfil aplica el substrat i
    llegeix dades del web.
- `presentacions/` — presentacions Marp.

## Regles del web (heretades)
- Zero JS al client.
- Nomenclatura de fitxers a `web/fitxers/`: `YYYYMMDD_titol-del-fitxer.pdf`.
- Actes noves: afegir al capdamunt de la llista `<ul class="acta-list">` de `web/afa.html` (ordre cronològic invers).

## Redacció
- Català, frases curtes i clares, llenguatge planer (accessible per a infants).
