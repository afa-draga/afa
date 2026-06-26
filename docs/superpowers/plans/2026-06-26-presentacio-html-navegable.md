# Presentació HTML navegable Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Generar una versió HTML navegable i animada de la presentació de benvinguda, consultable des del web, sense duplicar contingut.

**Architecture:** El `.md` Marp existent segueix sent l'única font. S'hi afegeixen marques d'animació natives de Marp (transició + fragments) que no trenquen el PDF. Es genera un HTML *bespoke* autocontingut a `web/presentacions/` i s'hi enllaça des de `web/afa.html`.

**Tech Stack:** Marp (`@marp-team/marp-cli` via `npx`), tema `presentacions/temes/afa.css`, HTML/CSS pur al web. Verificació amb Playwright.

## Global Constraints

- **Zero JS al web institucional** (`web/*.html` de l'arrel). El JS només és acceptable dins l'HTML generat a `web/presentacions/`.
- **Font única**: el contingut viu només al `.md`; no es duplica.
- **Comanda de generació** (exacta, cal `--no-stdin` o es penja):
  ```
  npx -y @marp-team/marp-cli@latest presentacions/20260619_Presentacio_Benvinguda_Families.md --theme presentacions/temes/afa.css --html --no-stdin -o web/presentacions/benvinguda-families.html
  ```
- **Sintaxi d'animació Marp** (verificada): `transition: fade` al front-matter; llistes amb `*` (no `-`) generen fragments revelables.
- **El workflow `pages.yml` només desplega `web/`** — no es modifica.
- Català, frases curtes i clares.

---

### Task 1: Afegir transició i fragments al `.md` font

**Files:**
- Modify: `presentacions/20260619_Presentacio_Benvinguda_Families.md`

**Interfaces:**
- Consumes: res.
- Produces: un `.md` que, en renderitzar-se a HTML, conté `data-transition` (fade) i `data-marpit-fragment` a les llistes seleccionades. La comanda de render del Global Constraints és l'única interfície cap a la Task 2.

**Quines llistes es fragmenten** (canviar `-` per `*`, mantenint indentació i contingut idèntics):
- «Què us expliquem avui»
- «L'AFA sou totes les famílies»
- «Què fem des de l'AFA»
- «On us representem»
- «Menjador: un servei de confiança»
- «Menjador: el dia a dia» (la llista de 3 punts sota les targetes)
- «Treballem en comissions» (les dues llistes dins de les `.card`)
- «Sumeu-vos-hi»

**No es toquen:** diapositives `lead`, la taula d'horaris d'extraescolars, ni els blocs `.card` que no contenen llista (serveis, festes, com estar al dia, contacte).

- [ ] **Step 1: Afegir la directiva de transició al front-matter**

Al bloc front-matter (línies 1–6), afegir `transition: fade` després de `html: true`:

```yaml
---
marp: true
theme: afa
paginate: true
html: true
transition: fade
---
```

- [ ] **Step 2: Convertir a fragments les llistes seleccionades**

A cadascuna de les diapositives llistades a dalt, canviar el prefix `- ` per `* ` a cada element de primer nivell de la llista. Exemple a «Què us expliquem avui»:

```markdown
## Què us expliquem avui

* Qui som i què fem des de l'AFA
* Els serveis que teniu a l'abast: **menjador** i **extraescolars**
* Com podeu participar-hi: les **comissions**
* Com estar al dia i com trobar-nos

<p class="muted">Sense pressa: tot això ho anireu descobrint al llarg del curs.</p>
```

A «Treballem en comissions», les llistes són dins de `<div class="card">`; canviar igualment `-` per `*` dins de cada card (mantenint la línia en blanc abans de la llista que Marp requereix dins de blocs HTML).

- [ ] **Step 3: Renderitzar a un HTML temporal per verificar**

Run:
```
npx -y @marp-team/marp-cli@latest presentacions/20260619_Presentacio_Benvinguda_Families.md --theme presentacions/temes/afa.css --html --no-stdin -o "$TMPDIR/check.html"
```
(substituir `$TMPDIR` per una ruta temporal qualsevol fora del repo)
Expected: `EXIT=0`, missatge `=> ...check.html`.

- [ ] **Step 4: Verificar que hi ha fragments i transició**

Run:
```
grep -c 'data-marpit-fragment' "$TMPDIR/check.html"
grep -c 'data-transition=' "$TMPDIR/check.html"
```
Expected: el primer comptador > 20 (moltes llistes fragmentades), el segon > 0 (transició present).

- [ ] **Step 5: Commit**

```bash
git add presentacions/20260619_Presentacio_Benvinguda_Families.md
git commit -m "Afegeix transició i fragments a la presentació de benvinguda"
```

---

### Task 2: Generar l'HTML publicat i documentar la regeneració

**Files:**
- Create: `web/presentacions/benvinguda-families.html` (artefacte generat, es committeja)
- Create: `presentacions/README.md` (documenta la comanda de regeneració)

**Interfaces:**
- Consumes: el `.md` de la Task 1 i la comanda del Global Constraints.
- Produces: el fitxer `web/presentacions/benvinguda-families.html` (ruta exacta que enllaçarà la Task 3) i la URL `presentacions/benvinguda-families.html` relativa a `web/`.

- [ ] **Step 1: Crear la carpeta i generar l'HTML a la ubicació publicada**

Run:
```
npx -y @marp-team/marp-cli@latest presentacions/20260619_Presentacio_Benvinguda_Families.md --theme presentacions/temes/afa.css --html --no-stdin -o web/presentacions/benvinguda-families.html
```
Expected: `EXIT=0`. Es crea `web/presentacions/benvinguda-families.html`.

- [ ] **Step 2: Verificar que l'HTML és autocontingut i navegable**

Run:
```
grep -c 'bespoke' web/presentacions/benvinguda-families.html
grep -c '#0056b3\|--afa-blau' web/presentacions/benvinguda-families.html
```
Expected: el primer > 0 (motor de navegació inclòs), el segon > 0 (tema `afa.css` inlinejat).

- [ ] **Step 3: Escriure `presentacions/README.md` amb la comanda de regeneració**

Contingut exacte:

```markdown
# Presentacions

Presentacions de l'AFA en format Marp. El `.md` és l'única font: genera el PDF i la versió HTML.

## Regenerar la versió HTML navegable

Després de modificar `20260619_Presentacio_Benvinguda_Families.md`, regenera l'HTML
publicat (cal `--no-stdin` o el procés es penja esperant stdin):

\`\`\`
npx -y @marp-team/marp-cli@latest \
  presentacions/20260619_Presentacio_Benvinguda_Families.md \
  --theme presentacions/temes/afa.css \
  --html --no-stdin \
  -o web/presentacions/benvinguda-families.html
\`\`\`

L'HTML es genera sota `web/` perquè el desplegament a GitHub Pages només publica
aquesta carpeta. Es committeja l'artefacte generat (com el PDF).

## Animació

- `transition: fade` al front-matter: transició suau entre diapositives.
- Llistes amb `*` (en comptes de `-`): es revelen punt a punt (fragments).
```

(Nota per a l'implementador: al fitxer real, els backticks de dins del bloc `\`\`\`` han de ser backticks normals de tres; aquí van escapats només per mostrar-los.)

- [ ] **Step 4: Commit**

```bash
git add web/presentacions/benvinguda-families.html presentacions/README.md
git commit -m "Publica la versió HTML navegable de la presentació de benvinguda"
```

---

### Task 3: Enllaçar la presentació des de `web/afa.html`

**Files:**
- Modify: `web/afa.html` (afegir una targeta enllaç dins d'una secció existent)

**Interfaces:**
- Consumes: la ruta `presentacions/benvinguda-families.html` de la Task 2.
- Produces: un enllaç visible a la pàgina L'AFA. Zero JS.

- [ ] **Step 1: Afegir una secció amb la targeta de la presentació**

Inserir aquesta secció a `web/afa.html` just abans del comentari `<!-- ===== ACTES ===== -->` (cap a la línia 132), seguint el to i marcatge del web:

```html
  <!-- ===== PRESENTACIÓ ===== -->
  <section class="block bg-subtle">
    <div class="wrap wrap-narrow">
      <span class="eyebrow">Per a noves famílies</span>
      <h2>Presentació de benvinguda</h2>
      <p>Una presentació que explica qui som, els serveis i com participar-hi. La podeu consultar en línia, diapositiva a diapositiva.</p>
      <p style="margin-top: 24px;"><a class="btn btn-primary" href="presentacions/benvinguda-families.html" target="_blank" rel="noopener">Obrir la presentació →</a></p>
    </div>
  </section>

```

- [ ] **Step 2: Verificar que l'enllaç i el marcatge són correctes**

Run:
```
grep -n 'presentacions/benvinguda-families.html' web/afa.html
```
Expected: una coincidència amb `href="presentacions/benvinguda-families.html"`.

Verificar Zero JS al fitxer:
```
grep -c '<script' web/afa.html
```
Expected: `0`.

- [ ] **Step 3: Commit**

```bash
git add web/afa.html
git commit -m "Enllaça la presentació de benvinguda des de la pàgina L'AFA"
```

---

### Task 4: Verificació funcional amb Playwright (evidència real)

**Files:**
- Cap canvi de codi. Verificació sobre `web/presentacions/benvinguda-families.html`.

**Interfaces:**
- Consumes: l'HTML generat (Task 2) i l'enllaç (Task 3).
- Produces: evidència que el deck navega, anima i es veu correcte.

- [ ] **Step 1: Obrir l'HTML al navegador**

Amb Playwright, navegar a `file://` de la ruta absoluta de `web/presentacions/benvinguda-families.html`. Fer una captura. Verificar visualment que es veu la portada amb el tema AFA (blau corporatiu, accent groc).

- [ ] **Step 2: Verificar navegació i fragments**

Prémer `ArrowRight` diverses vegades. Verificar amb captures que: (a) s'avança de diapositiva; (b) en una diapositiva amb llista fragmentada, els punts apareixen un a un abans de passar a la següent.

- [ ] **Step 3: Verificar navegació per hash**

Navegar a `...benvinguda-families.html#5` (o `#/5` segons el format que generi Marp). Verificar amb captura que salta a la diapositiva corresponent.

- [ ] **Step 4: Verificar l'enllaç des del web**

Obrir `web/afa.html` i comprovar que el botó «Obrir la presentació →» apunta a l'HTML. (Pot ser amb `grep` ja fet a Task 3; aquí confirmar visualment la targeta.)

- [ ] **Step 5: Reportar evidència**

Resumir els resultats amb les captures com a prova. No declarar "fet" sense aquesta evidència.

---

## Self-Review

**Spec coverage:**
- Font única + marques d'animació al `.md` → Task 1. ✓
- HTML a `web/presentacions/` + comanda documentada → Task 2. ✓
- Enllaç a `web/afa.html` → Task 3. ✓
- Verificació real (navegació, fragments, hash, tema) → Task 4. ✓
- Fora d'abast (no reveal.js, no Actions, no canvi de text, no JS al web) → respectat. ✓

**Placeholder scan:** Sense TBD/TODO. Tot el codi i comandes són concrets.

**Type/path consistency:** La ruta `web/presentacions/benvinguda-families.html` i l'enllaç relatiu `presentacions/benvinguda-families.html` són coherents entre Task 2 i Task 3. La comanda de generació és idèntica al Global Constraints, Task 1 (verificació) i Task 2 (publicació).
