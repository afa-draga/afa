---
name: presentacio
description: Usa quan calgui crear una presentació de l'AFA — per a assemblees, juntes o xerrades.
---

# Crear una presentació

Genera presentacions amb Marp (Markdown → diapositives) amb la identitat de l'AFA.

## Procediment
1. Aplica `veu-afa` (poc text per diapositiva, frases curtes, llenguatge planer).
   El to es regula segons el públic: per a **famílies** (benvingudes, xerrades)
   fes-lo **proper i càlid**, amb tractament de «vosaltres».
2. Aplica `identitat-afa` (colors i logo segons `web/assets/styles.css`).
3. Llegeix del `web/` les dades que necessitis (font de veritat).
4. Si falta informació, pregunta: tema, públic, durada aproximada, missatges clau.
5. Crea el fitxer a `presentacions/AAAAMMDD_Presentacio_[Titol].md` (format datat,
   per mantenir-les ordenades) amb aquesta capçalera:

   ```markdown
   ---
   marp: true
   theme: afa
   paginate: true
   html: true
   ---
   ```

   `html: true` cal per als blocs de maquetació (targetes, columnes, taules).
   El tema `afa` és a `presentacions/temes/afa.css`.
6. Estructura: portada (classe `lead`), una idea per diapositiva, tancament amb
   contacte (llegit del web). **No** posis diapositives divisòries de secció:
   el nom de la secció ja és el títol (`##`) de la seva diapositiva.
7. Per exportar, indica a l'usuari l'ordre (no cal executar-la si no ho demana):
   `npx @marp-team/marp-cli presentacions/[nom].md --theme presentacions/temes/afa.css --html -o presentacions/[nom].pdf`

## Estil (decisions vigents)
- **Fons sempre clars**, com el web (crema `--afa-bg`). Res de fons blaus plens;
  les portades `lead` també són clares, amb títol blau i subratllat groc.
- **Poc text** per diapositiva; el detall es diu de viva veu.
- **Emoticones** als títols de targeta i als ítems de llista (p. ex. la llista de
  comissions). Una per element, sòbria i coherent amb el contingut.

## Utilitats del tema (`presentacions/temes/afa.css`)
Amb `html: true` pots fer servir:
- `<!-- _class: lead -->` — portada i tancament (fons clar, títol blau amb accent).
- `<!-- _class: dense -->` — diapositives amb més contingut (lletra més petita).
- `<div class="cols">…</div>` — dues columnes.
- `<div class="card">` / `<div class="card accent">` — targetes (accent = vora groga).
- `<p class="muted">` — text secundari.
- `<mark>` i `<span class="badge">` — ressaltat groc i etiqueta (p. ex. «Novetat»).
- `<div class="destacat">` — caixa destacada a la cantonada superior dreta.
- `<div class="tt-wrap"><table class="tt">` — quadre horari centrat
  (cel·les `td.h` hora, `td.acollida`, `span.lv` nivells, `td.x` buida).

Si afegeixes utilitats noves al tema, documenta-les aquí i mantén la paleta de
`identitat-afa` (no inventis colors).
