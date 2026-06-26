# Presentació HTML navegable de benvinguda

**Data:** 2026-06-26
**Estat:** aprovat (pendent de revisió de l'spec per l'usuari)

## Problema

Tenim una presentació de benvinguda per a noves famílies en format Marp
(`presentacions/20260619_Presentacio_Benvinguda_Families.md`) que es projecta com a
PDF. Projectar el PDF és incòmode. Volem una versió **HTML navegable** (estil
reveal.js / tensormedical.ai: tecles, transicions, `#/N` a la URL), consultable des
del web, amb alguna **animació** per donar-hi interactivitat.

## Decisions preses (brainstorming)

1. **JS només a `/presentacions/`.** La regla d'or Zero JS es manté per al web
   institucional (pàgines `.html` de l'arrel de `web/`). La presentació és un
   artefacte separat i sí que pot fer servir JS.
2. **Eina: exportar el Marp a HTML** (no reveal.js, no deck propi). Una sola font
   (el `.md`) genera PDF **i** HTML amb el mateix tema `temes/afa.css`. Respecta el
   principi de font única del repo.
3. **Animació amb fragments**: revelar punt a punt les llistes i targetes, més una
   transició suau entre diapositives.
4. **Enllaç al web**: una targeta a `web/afa.html`.

## Validació tècnica (de-risk)

`npx @marp-team/marp-cli --html --no-stdin` genera un HTML autocontingut amb el motor
de navegació *bespoke* (tecles ← →, `#/N`, pantalla completa amb `f`), amb el tema
`afa.css` inlinejat. Comprovat: exit 0, fitxer únic ~130 KB. **Cal `--no-stdin`** o el
procés es penja esperant stdin.

## Disseny

### Font única
- El `.md` existent segueix sent l'única font de contingut. Generarà PDF i HTML.
- L'única modificació al `.md` és **afegir marques d'animació** que no trenquen el PDF:
  - Directiva global de transició: `<!-- transition: fade -->` al front-matter o
    com a comentari de diapositiva.
  - Fragments: convertir les llistes que volem revelar punt a punt a la sintaxi de
    fragments de Marp (llistes amb `*` o atribut `data-marpit-fragment`). Aplicar-ho
    a les diapositives amb contingut enumerable (Què expliquem, Què fem, serveis,
    comissions, com estar al dia). Les diapositives `lead` i la taula d'horaris no
    porten fragments.

### Ubicació de fitxers
- Font i tema: es queden a `/presentacions/` (sense moure).
- HTML publicat: `web/presentacions/benvinguda-families.html`. Sota `web/` perquè
  `.github/workflows/pages.yml` només desplega aquesta carpeta.
- URL final: `afaladraga.cat/presentacions/benvinguda-families.html`.

### Regeneració (procés documentat)
Comanda per regenerar l'HTML quan canvia el `.md`:

```
npx -y @marp-team/marp-cli@latest \
  presentacions/20260619_Presentacio_Benvinguda_Families.md \
  --theme presentacions/temes/afa.css \
  --html --no-stdin \
  -o web/presentacions/benvinguda-families.html
```

L'HTML generat es committeja al repo, igual que ja es fa amb el PDF. **No es toca
`pages.yml`.** Aquesta comanda es documenta (README de `presentacions/` o comentari)
perquè sigui reproduïble.

### Enllaç des del web
- Afegir una targeta/enllaç a `web/afa.html`, coherent amb les targetes existents
  (mateix marcatge i classes que la resta del web). Text: «Presentació de benvinguda
  per a noves famílies». Obre l'HTML (pestanya nova, `target="_blank"
  rel="noopener"`).
- Aquest canvi a `web/afa.html` és HTML pur, respecta Zero JS al web institucional.

## Verificació (evidència real)
Amb Playwright sobre l'HTML generat:
1. Es carrega i es veu la portada amb el tema AFA (blau/groc).
2. Fletxa dreta avança; els **fragments** es revelen punt a punt a les diapositives
   amb llistes.
3. La **transició** entre diapositives funciona.
4. `#/N` a la URL salta a la diapositiva N.
5. La targeta a `web/afa.html` enllaça correctament al fitxer.

## Fora d'abast (YAGNI)
- No es modifica el contingut textual de la presentació (ja és del curs 2026–2027).
- No s'automatitza la generació via GitHub Actions (es committeja l'artefacte, com el
  PDF).
- No es migra a reveal.js ni es crea un deck propi.
- No s'afegeix navegació per fletxes ni JS al web institucional.
