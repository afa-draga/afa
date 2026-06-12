# Monorepo `afa` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Construir un monorepo públic `afa` que reuneixi el web (font de veritat), una guia d'estil compartida i tres skills de Claude Code (comunicats, actes, presentacions) per generar comunicacions coherents de l'AFA.

**Architecture:** Un sol repositori. El web viu a `web/` i és l'única font de dades factuals; es desplega a GitHub Pages via Actions. La guia d'estil (`to-de-veu.md`) és l'única font d'estil. Les skills llegeixen de tots dos i generen sortides (comunicats transitoris, actes cap a `web/fitxers/actes/`, presentacions Marp a `presentacions/`).

**Tech Stack:** Git (subtree per migrar amb història), GitHub Pages + GitHub Actions, Markdown (skills i guia d'estil), Marp CLI (presentacions), HTML/Tailwind (web heretat).

**Notes de verificació:** Aquest pla no té tests unitaris clàssics. Cada tasca acaba amb una verificació concreta (fitxer existeix, capçalera YAML vàlida, Marp renderitza sense error, el web es serveix). El repo local ja existeix a `D:/40361989w/Dev/afa` amb branca `main` i el document de disseny commitat.

**Assumpcions a confirmar durant l'execució:**
- El repo del web actual és `afa-draga/web` i la seva branca és `master` (igual que la còpia local `D:/40361989w/Dev/web-afa`).
- La migració s'alimenta de la còpia local `D:/40361989w/Dev/web-afa` (té l'historial complet), no de la xarxa.
- Crear el repo públic `afa-draga/afa` a GitHub i traslladar el domini són accions cap enfora: es fan amb confirmació explícita i requereixen `gh auth` amb permisos sobre l'organització.

---

## Fase 1 — Esquelet del repositori

### Task 1: Fitxers base del repo (README, .gitignore, .editorconfig)

**Files:**
- Create: `D:/40361989w/Dev/afa/README.md`
- Create: `D:/40361989w/Dev/afa/.gitignore`

- [ ] **Step 1: Crear el README**

```markdown
# AFA Escola La Draga — coneixement i eines

Monorepo de l'AFA de l'Escola La Draga (Banyoles). Conté:

- `web/` — el web públic (https://afaladraga.cat). Font única de les dades de l'AFA.
- `.claude/skills/` — skills de Claude Code per generar comunicats, actes i presentacions.
- `presentacions/` — presentacions generades amb Marp.

## Principis

- **Una sola font de veritat:** les dades viuen al web; l'estil, a `.claude/skills/_base/to-de-veu.md`.
- **Català, llenguatge planer.** Frases curtes i clares.
- **Zero JS** al client del web.

## Com s'usa

Obre aquest repo amb Claude Code i demana, per exemple, "redacta un comunicat per
a les famílies sobre...". Les skills s'activen soles.
```

- [ ] **Step 2: Crear el .gitignore**

```gitignore
# Sortides exportades de presentacions
presentacions/**/*.pdf
presentacions/**/*.pptx
presentacions/**/*.html

# Sistema
.DS_Store
Thumbs.db
node_modules/
```

- [ ] **Step 3: Verificar i commitar**

Run: `cd D:/40361989w/Dev/afa && git add README.md .gitignore && git status --short`
Expected: tots dos fitxers en staging (`A  README.md`, `A  .gitignore`).

```bash
cd D:/40361989w/Dev/afa
git commit -m "Afegeix README i .gitignore del monorepo"
```

---

### Task 2: CLAUDE.md del monorepo

**Files:**
- Create: `D:/40361989w/Dev/afa/CLAUDE.md`

- [ ] **Step 1: Escriure el CLAUDE.md**

```markdown
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
```

- [ ] **Step 2: Verificar i commitar**

Run: `cd D:/40361989w/Dev/afa && git add CLAUDE.md && git diff --cached --stat`
Expected: `CLAUDE.md` apareix amb línies afegides.

```bash
cd D:/40361989w/Dev/afa
git commit -m "Afegeix CLAUDE.md del monorepo"
```

---

## Fase 2 — Migració del web amb història

### Task 3: Incorporar el web a `web/` conservant l'historial

**Files:**
- Create (via subtree): `D:/40361989w/Dev/afa/web/**` (tot el web actual)

- [ ] **Step 1: Confirmar la branca i l'estat net de la font**

Run: `cd D:/40361989w/Dev/web-afa && git rev-parse --abbrev-ref HEAD && git status --short`
Expected: la branca (p. ex. `master`). Si hi ha canvis sense commitar rellevants
(p. ex. `claude.md` no rastrejat), no afecten el subtree (només usa commits).

- [ ] **Step 2: Afegir la còpia local com a remot i fer fetch**

```bash
cd D:/40361989w/Dev/afa
git remote add web-src "D:/40361989w/Dev/web-afa"
git fetch web-src
```

Run: `git remote -v`
Expected: apareix `web-src` apuntant a la ruta local.

- [ ] **Step 3: Fer el subtree add a `web/`**

```bash
cd D:/40361989w/Dev/afa
git subtree add --prefix=web web-src/master
```
(Si la branca no és `master`, usar la detectada al Step 1.)

Run: `git log --oneline | head -5`
Expected: apareix un commit de merge del subtree i, amb `git log --oneline web/`,
es veu l'historial del web.

- [ ] **Step 4: Verificar que el web és a la carpeta**

Run: `ls D:/40361989w/Dev/afa/web/`
Expected: es veuen `index.html`, `afa.html`, `fitxers/`, `assets/`, etc.

- [ ] **Step 5: Netejar el remot temporal**

```bash
cd D:/40361989w/Dev/afa
git remote remove web-src
```

Run: `git remote -v`
Expected: cap remot `web-src`.

---

### Task 4: Preparar el web per al desplegament des de subcarpeta

**Files:**
- Create/verify: `D:/40361989w/Dev/afa/web/.nojekyll`
- Create/verify: `D:/40361989w/Dev/afa/web/CNAME`

- [ ] **Step 1: Comprovar si ja existeixen CNAME i .nojekyll**

Run: `ls -a D:/40361989w/Dev/afa/web/ | grep -E 'CNAME|.nojekyll'`
Expected: pot existir `CNAME` (heretat del repo antic). Anota què hi ha.

- [ ] **Step 2: Garantir `.nojekyll`** (HTML pur, sense processament Jekyll)

Si no existeix, crear `D:/40361989w/Dev/afa/web/.nojekyll` buit.

- [ ] **Step 3: Garantir `CNAME`** amb el domini

Si no existeix, crear `D:/40361989w/Dev/afa/web/CNAME` amb el contingut exacte:
```
afaladraga.cat
```
Si ja existeix amb aquest contingut, deixar-lo igual.

- [ ] **Step 4: Commitar el que s'hagi creat**

Run: `cd D:/40361989w/Dev/afa && git status --short web/`
Expected: només els fitxers nous/modificats d'aquest pas (o res, si ja existien).

```bash
cd D:/40361989w/Dev/afa
git add web/.nojekyll web/CNAME
git commit -m "Prepara el web per a desplegament des de subcarpeta" || echo "res a commitar"
```

---

## Fase 3 — Desplegament a GitHub Pages

### Task 5: Workflow d'Actions que desplega només `web/`

**Files:**
- Create: `D:/40361989w/Dev/afa/.github/workflows/pages.yml`

- [ ] **Step 1: Escriure el workflow**

```yaml
name: Desplega el web a GitHub Pages

on:
  push:
    branches: [main]
    paths:
      - 'web/**'
      - '.github/workflows/pages.yml'
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Configurar Pages
        uses: actions/configure-pages@v5
      - name: Pujar artefacte (només web/)
        uses: actions/upload-pages-artifact@v3
        with:
          path: web
      - name: Desplegar
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 2: Validar la sintaxi YAML**

Run: `cd D:/40361989w/Dev/afa && python -c "import yaml,sys; yaml.safe_load(open('.github/workflows/pages.yml')); print('YAML OK')"`
Expected: `YAML OK`

- [ ] **Step 3: Commitar**

```bash
cd D:/40361989w/Dev/afa
git add .github/workflows/pages.yml
git commit -m "Afegeix workflow de desplegament a GitHub Pages des de web/"
```

---

### Task 6: Crear el repo a GitHub i verificar el desplegament (acció confirmada)

> ⚠️ Acció cap enfora. Confirmar amb l'usuari abans d'executar. Requereix `gh auth`
> amb permisos sobre l'organització `afa-draga`. NO es toca el domini en aquesta tasca.

**Files:** cap (operacions remotes).

- [ ] **Step 1: Confirmar autenticació i organització**

Run: `gh auth status`
Expected: sessió activa amb accés a `afa-draga`.

- [ ] **Step 2: Crear el repo públic i pujar**

```bash
cd D:/40361989w/Dev/afa
gh repo create afa-draga/afa --public --source=. --remote=origin --push
```

Run: `git remote -v`
Expected: `origin` apunta a `github.com/afa-draga/afa`.

- [ ] **Step 3: Activar Pages amb origen GitHub Actions**

```bash
gh api -X POST repos/afa-draga/afa/pages -f build_type=workflow || \
gh api -X PUT repos/afa-draga/afa/pages -f build_type=workflow
```

- [ ] **Step 4: Verificar que el workflow s'executa correctament**

Run: `gh run list --repo afa-draga/afa --workflow pages.yml -L 1`
Expected: una execució amb conclusió `success` (esperar si està `in_progress`).

- [ ] **Step 5: Verificar que el web es serveix a la URL temporal de Pages**

Run: `gh api repos/afa-draga/afa/pages --jq .html_url`
Després: `curl -sS -o /dev/null -w "%{http_code}" "$(gh api repos/afa-draga/afa/pages --jq .html_url)"`
Expected: codi `200`. (Encara servit a la URL de github.io, NO al domini.)

---

## Fase 4 — Guia d'estil i skills

### Task 7: Guia d'estil compartida `to-de-veu.md`

**Files:**
- Create: `D:/40361989w/Dev/afa/.claude/skills/_base/to-de-veu.md`

- [ ] **Step 1: Escriure la guia d'estil**

```markdown
# To de veu de l'AFA Escola La Draga

Aquesta és l'ÚNICA font d'estil. Totes les skills de comunicació la llegeixen.

## Principis
- **Llengua:** català, sempre.
- **Claredat:** frases curtes i directes. Una idea per frase.
- **Llenguatge planer:** entenedor fins i tot per a infants de 6 anys. Evita
  tecnicismes, paraules rebuscades i sigles sense explicar.
- **To:** proper, càlid i respectuós. Som famílies parlant amb famílies.
- **Persona:** ens adrecem a "les famílies" amb proximitat (sense formalismes
  freds, sense "vostè").

## Sempre
- Digues primer el més important (què passa i què cal fer).
- Concreta dates, hores i llocs sense ambigüitat.
- Acaba amb una acció clara quan calgui ("Cal apuntar-s'hi abans del...").

## Evita
- Paràgrafs llargs i frases subordinades encadenades.
- Tecnicismes administratius ("a tals efectes", "el present comunicat").
- Majúscules per cridar l'atenció i signes d'exclamació en excés.

## Dades i contacte
Les dades concretes (email, comissions, persones, enllaços) NO es copien aquí:
es llegeixen del web (`web/`), que és la font de veritat.
```

- [ ] **Step 2: Verificar i commitar**

Run: `ls D:/40361989w/Dev/afa/.claude/skills/_base/to-de-veu.md`
Expected: el fitxer existeix.

```bash
cd D:/40361989w/Dev/afa
git add .claude/skills/_base/to-de-veu.md
git commit -m "Afegeix guia d'estil compartida (to de veu)"
```

---

### Task 8: Skill `acta-reunio`

**Files:**
- Create: `D:/40361989w/Dev/afa/.claude/skills/acta-reunio/SKILL.md`

- [ ] **Step 1: Escriure el SKILL.md**

````markdown
---
name: acta-reunio
description: Usa quan calgui redactar l'acta d'una reunió de l'AFA (assemblea o junta) a partir de notes o d'un guió. Genera l'acta amb el format estàndard i la prepara per publicar-la al web.
---

# Redactar una acta de reunió

## Quan s'usa
Quan l'usuari aporta notes, un guió o un resum d'una reunió i vol l'acta redactada.

## Procediment
1. Llegeix la guia d'estil: `.claude/skills/_base/to-de-veu.md`. Aplica-la (català,
   frases curtes, llenguatge planer).
2. Si manca informació clau, pregunta-la abans de redactar: data, tipus de reunió
   (assemblea / junta), assistents, ordre del dia, acords.
3. Redacta l'acta amb aquesta estructura:

   - **Títol:** "Acta de [assemblea/junta] de l'AFA — [data]"
   - **Dades:** data, hora d'inici i fi, lloc, persones assistents (nom sense
     cognom), persones excusades.
   - **Ordre del dia:** llista numerada dels punts tractats.
   - **Desenvolupament i acords:** per cada punt, un resum breu i l'acord pres.
   - **Tasques:** taula de tasques amb responsable i termini (si n'hi ha).
   - **Propera reunió:** data prevista (si s'ha fixat).

4. Mantén-ho concís: resumeix, no transcriguis paraula per paraula.

## Publicació al web
Quan l'usuari validi el contingut, segueix el flux del web:
1. Genera/desa el PDF amb el nom `YYYYMMDD_acta-[tipus].pdf` (data de la reunió).
2. Desa'l a `web/fitxers/actes/`.
3. Afegeix l'enllaç al CAPDAMUNT de la llista `<ul class="acta-list">` de `web/afa.html` (ordre cronològic
   invers), amb un text d'enllaç planer (p. ex. "Acta de la junta del 12 de juny de 2026").
4. Mantén el codi HTML net i coherent amb la resta de la llista.
````

- [ ] **Step 2: Validar la capçalera YAML**

Run:
```bash
cd D:/40361989w/Dev/afa && python -c "import yaml; t=open('.claude/skills/acta-reunio/SKILL.md').read(); fm=t.split('---')[1]; d=yaml.safe_load(fm); assert d['name']=='acta-reunio' and d['description']; print('frontmatter OK')"
```
Expected: `frontmatter OK`

- [ ] **Step 3: Commitar**

```bash
cd D:/40361989w/Dev/afa
git add .claude/skills/acta-reunio/SKILL.md
git commit -m "Afegeix skill acta-reunio"
```

---

### Task 9: Skill `comunicat-socis`

**Files:**
- Create: `D:/40361989w/Dev/afa/.claude/skills/comunicat-socis/SKILL.md`

- [ ] **Step 1: Escriure el SKILL.md**

````markdown
---
name: comunicat-socis
description: Usa quan calgui redactar un avís o comunicat per a les famílies de l'AFA (comunitat de WhatsApp o correu). Genera un missatge breu i clar, llest per copiar i enviar.
---

# Redactar un comunicat per a les famílies

## Quan s'usa
Quan l'usuari vol comunicar alguna cosa a les famílies: un avís, un recordatori,
una convocatòria, una novetat d'una comissió, etc.

## Procediment
1. Llegeix la guia d'estil: `.claude/skills/_base/to-de-veu.md`. Aplica-la.
2. Si necessites dades concretes (email de contacte, qui forma una comissió, un
   enllaç del web), llegeix-les del `web/` (és la font de veritat). No te les
   inventis ni les dupliquis.
3. Si falta informació clau, pregunta-la: què es comunica, a qui, quan/on, i quina
   acció s'espera de les famílies.
4. Genera DUES versions:
   - **WhatsApp:** molt breu (2-5 frases), directa, amb emoji només si aporta
     claredat. Comença pel més important.
   - **Correu:** una mica més desenvolupada, amb assumpte clar i comiat cordial.
5. La sortida és transitòria: es mostra a la conversa perquè l'usuari la copiï. No
   es desa cap fitxer.

## Recordatori d'estil
- Primer el què i el què cal fer; després els detalls.
- Dates, hores i llocs sense ambigüitat.
- Sense tecnicismes ni paràgrafs llargs.
````

- [ ] **Step 2: Validar la capçalera YAML**

Run:
```bash
cd D:/40361989w/Dev/afa && python -c "import yaml; t=open('.claude/skills/comunicat-socis/SKILL.md').read(); fm=t.split('---')[1]; d=yaml.safe_load(fm); assert d['name']=='comunicat-socis' and d['description']; print('frontmatter OK')"
```
Expected: `frontmatter OK`

- [ ] **Step 3: Commitar**

```bash
cd D:/40361989w/Dev/afa
git add .claude/skills/comunicat-socis/SKILL.md
git commit -m "Afegeix skill comunicat-socis"
```

---

### Task 10: Tema Marp amb la identitat del web

**Files:**
- Create: `D:/40361989w/Dev/afa/presentacions/temes/afa.css`

- [ ] **Step 1: Escriure el tema Marp** (paleta del web: blau #0056b3 i blanc)

```css
/* @theme afa */
@import 'default';

:root {
  --afa-blau: #0056b3;
  --afa-blau-fosc: #003d80;
  --afa-text: #1a2733;
}

section {
  font-family: -apple-system, "Segoe UI", system-ui, sans-serif;
  font-size: 28px;
  color: var(--afa-text);
  padding: 60px;
}

h1, h2 {
  color: var(--afa-blau);
}

section.lead h1 {
  color: #fff;
}

section.lead {
  background: var(--afa-blau);
  color: #fff;
}

a { color: var(--afa-blau-fosc); }

header, footer {
  color: var(--afa-blau);
}
```

- [ ] **Step 2: Verificar i commitar**

Run: `ls D:/40361989w/Dev/afa/presentacions/temes/afa.css`
Expected: el fitxer existeix.

```bash
cd D:/40361989w/Dev/afa
git add presentacions/temes/afa.css
git commit -m "Afegeix tema Marp amb la identitat del web"
```

---

### Task 11: Skill `presentacio`

**Files:**
- Create: `D:/40361989w/Dev/afa/.claude/skills/presentacio/SKILL.md`
- Create: `D:/40361989w/Dev/afa/presentacions/exemple.md` (presentació mínima per verificar el tema)

- [ ] **Step 1: Escriure el SKILL.md**

````markdown
---
name: presentacio
description: Usa quan calgui crear una presentació de l'AFA (per a assemblees, juntes o xerrades). Genera diapositives amb Marp i el tema visual de l'AFA.
---

# Crear una presentació

## Quan s'usa
Quan l'usuari vol una presentació: assemblea de famílies, reunió de junta, xerrada,
resum de curs, etc.

## Procediment
1. Llegeix la guia d'estil: `.claude/skills/_base/to-de-veu.md`. Aplica-la (poc
   text per diapositiva, frases curtes, llenguatge planer).
2. Llegeix del `web/` les dades que necessitis (és la font de veritat).
3. Si falta informació, pregunta: tema, públic, durada aproximada, missatges clau.
4. Crea un fitxer Markdown de Marp a `presentacions/[nom].md` amb aquesta capçalera:

   ```markdown
   ---
   marp: true
   theme: afa
   paginate: true
   ---
   ```

   El tema `afa` és a `presentacions/temes/afa.css`.
5. Estructura recomanada: portada (classe `lead`), índex breu, una idea per
   diapositiva, diapositiva de tancament amb contacte (llegit del web).
6. Per exportar, indica a l'usuari l'ordre (no cal executar-la si no ho demana):
   `npx @marp-team/marp-cli presentacions/[nom].md --theme presentacions/temes/afa.css -o presentacions/[nom].pdf`

## Recordatori
- Poc text per diapositiva; el detall es diu de viva veu.
- Mantén la identitat: blau corporatiu i blanc (ja al tema).
````

- [ ] **Step 2: Crear una presentació d'exemple mínima**

```markdown
---
marp: true
theme: afa
paginate: true
---

<!-- _class: lead -->
# AFA Escola La Draga
Presentació d'exemple

---

## Qui som
- Famílies de l'Escola La Draga
- Organitzades en comissions

---

<!-- _class: lead -->
# Gràcies
ampa@ceipladraga.cat
```

- [ ] **Step 3: Validar la capçalera YAML del SKILL.md**

Run:
```bash
cd D:/40361989w/Dev/afa && python -c "import yaml; t=open('.claude/skills/presentacio/SKILL.md').read(); fm=t.split('---')[1]; d=yaml.safe_load(fm); assert d['name']=='presentacio' and d['description']; print('frontmatter OK')"
```
Expected: `frontmatter OK`

- [ ] **Step 4: Verificar que Marp renderitza l'exemple amb el tema**

Run:
```bash
cd D:/40361989w/Dev/afa && npx -y @marp-team/marp-cli presentacions/exemple.md --theme presentacions/temes/afa.css -o D:/tmp/exemple.pdf && ls D:/tmp/exemple.pdf
```
Expected: es genera `D:/tmp/exemple.pdf` sense errors. (És una verificació; el PDF
no es commita — `presentacions/**/*.pdf` està al `.gitignore`.)

- [ ] **Step 5: Commitar**

```bash
cd D:/40361989w/Dev/afa
git add .claude/skills/presentacio/SKILL.md presentacions/exemple.md
git commit -m "Afegeix skill presentacio i presentació d'exemple"
```

---

## Fase 5 — Migració del domini (final, acció confirmada)

### Task 12: Traslladar `afaladraga.cat` al nou repositori

> ⚠️ Acció cap enfora i sensible: canvia on apunta el web en producció. Fer-ho
> NOMÉS després de verificar (Task 6) que el nou desplegament serveix correctament.
> Confirmar amb l'usuari abans de cada pas.

**Files:** cap (configuració remota).

- [ ] **Step 1: Treure el domini personalitzat del repo antic**

Al repo `afa-draga/web`: Settings → Pages → treure el custom domain `afaladraga.cat`.
(Es pot fer per web o amb `gh api -X DELETE repos/afa-draga/web/pages` si es vol
desactivar Pages al repo antic.)

- [ ] **Step 2: Assignar el domini al nou repo**

```bash
gh api -X PUT repos/afa-draga/afa/pages -f cname=afaladraga.cat
```
(El fitxer `web/CNAME` ja conté `afaladraga.cat` des de la Task 4.)

- [ ] **Step 3: Verificar que el domini serveix el nou web**

Run: `curl -sS -o /dev/null -w "%{http_code}" https://afaladraga.cat/`
Expected: `200`. Comprovar visualment que és el web correcte.

- [ ] **Step 4: Arxivar el repo antic**

```bash
gh repo archive afa-draga/web --yes
```

---

## Self-Review (fet per l'autor del pla)

**Cobertura del spec:**
- §3 estructura → Tasks 1,2,5,7-11 creen tots els directoris/fitxers. ✓
- §4.1 web font de veritat → Task 3 (migració). ✓
- §4.2 to-de-veu → Task 7. ✓
- §4.3 skills (3) → Tasks 8,9,11. ✓
- §5 desplegament Pages des de web/ → Tasks 5,6. ✓
- §6 migració amb història + domini al final → Tasks 3,12. ✓
- §7 principis (zero duplicació, zero JS) → reflectits a CLAUDE.md (Task 2) i skills. ✓
- §8 YAGNI (sense comunicats/, sense backend) → cap tasca els crea. ✓

**Placeholders:** cap "TBD/TODO"; tot el contingut de fitxers és complet.

**Consistència de noms:** `afa.css` / tema `afa`, `web/`, `_base/to-de-veu.md`,
noms de skills (`acta-reunio`, `comunicat-socis`, `presentacio`) usats igual a tot
el pla i coincidents amb el spec.
