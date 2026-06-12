# Disseny: monorepo `afa` — coneixement i skills de l'AFA

- **Data:** 2026-06-11
- **Estat:** aprovat (pendent de revisió del document)
- **Entitat:** AFA de l'Escola La Draga (Banyoles)

## 1. Objectiu

Crear un únic repositori públic que reuneixi el coneixement de l'AFA i les
skills de Claude Code per generar, de forma coherent i ràpida, les sortides
recurrents: actualitzacions del **web**, **presentacions** i **comunicats** a
les famílies. Tot en un sol lloc per optimitzar el manteniment i poder-ho
compartir amb la junta i les comissions.

## 2. Decisions preses

- **Un sol repositori públic**, sota l'organització `afa-draga`, anomenat `afa`.
- **Tot el contingut serà públic** (decisió explícita): no s'hi guarda res
  sensible ni esborranys privats.
- **Font única de veritat dels fets = el web.** Les dades de l'AFA (qui som,
  comissions i qui les forma, contacte, dades recurrents) ja viuen i es mantenen
  al web. No es dupliquen en cap altre lloc; les skills les llegeixen del web.
- **El to de veu és guia d'estil, no una dada ni una skill.** Viu en un únic
  fitxer de referència compartit que totes les skills de comunicació llegeixen.
- **Migració del web amb història** (opció A): el web actual s'incorpora a `web/`
  conservant l'historial de git, i GitHub Pages passa a desplegar des de `web/`.
- **Presentacions amb Marp** (Markdown → diapositives), amb un tema CSS que
  replica la identitat visual del web.
- **Els comunicats són transitoris:** es generen, es copien a WhatsApp/email i no
  s'emmagatzemen. No hi ha carpeta `comunicats/`.

## 3. Estructura del repositori

```
afa/                          (públic, org afa-draga)
├── .claude/
│   └── skills/
│       ├── _base/
│       │   └── to-de-veu.md          ← guia d'estil compartida (única còpia)
│       ├── comunicat-socis/SKILL.md
│       ├── acta-reunio/SKILL.md
│       └── presentacio/SKILL.md
├── web/                  ← web actual migrat amb història; font de veritat + Pages
├── presentacions/        ← sortides Marp (.md) + exports (PDF/PPTX)
├── .github/workflows/pages.yml       ← desplega NOMÉS web/
├── CLAUDE.md
└── README.md
```

**Principi rector:** el web és la veritat dels *fets*; `_base/to-de-veu.md` és la
veritat de l'*estil*; les skills són les *tasques* que combinen les dues coses.

## 4. Components

### 4.1 `web/` — el web i font de veritat
- Conté el web estàtic actual (HTML pur, Zero JS al client, Tailwind).
- És l'única font de les dades factuals de l'AFA.
- Es desplega a GitHub Pages mitjançant Actions (vegeu §5).

### 4.2 `.claude/skills/_base/to-de-veu.md` — guia d'estil compartida
- Defineix com escriu l'AFA: català, frases curtes i clares, llenguatge planer
  (accessible fins i tot per a infants de 6 anys), to proper i institucional.
- Una sola còpia; les tres skills de comunicació la llegeixen per garantir una
  identitat coherent a comunicats, actes i presentacions.

### 4.3 Skills
Cada skill és un `SKILL.md` amb capçalera (`name`, `description`) i instruccions,
seguint el format de les skills de superpowers/gencat ja instal·lades.

- **`comunicat-socis`** — Redacta avisos i comunicats per a les famílies
  (WhatsApp/email). Llegeix les dades necessàries del `web/` i el to de
  `_base/to-de-veu.md`. Sortida transitòria (text a la conversa per copiar).
- **`acta-reunio`** — Redacta actes de reunió (assemblees i juntes) amb un format
  estàndard. La sortida alimenta el flux existent del web: s'anomena
  `YYYYMMDD_titol.pdf`, es desa a `web/fitxers/actes/` i s'enllaça a
  `web/actes.html` en ordre cronològic invers.
- **`presentacio`** — Genera presentacions amb Marp (Markdown → diapositives), amb
  un tema CSS que replica la identitat del web. Llegeix dades del `web/` i el to
  de `_base/to-de-veu.md`. Sortida a `presentacions/`.

## 5. Desplegament del web (GitHub Pages)

- Workflow `.github/workflows/pages.yml` que publica **només** el contingut de
  `web/` com a artefacte de Pages (`upload-pages-artifact` + `deploy-pages`).
- `web/` inclou `.nojekyll` (és HTML pur, no cal processament Jekyll) i `CNAME`
  amb `afaladraga.cat`.
- La resta del repo (skills, presentacions) queda al repositori però **no es
  serveix** com a web.
- La migració del domini `afaladraga.cat` es fa un cop verificat que el nou
  desplegament funciona, per evitar caigudes del web en producció.

## 6. Migració del web actual (opció A — amb història)

1. Incorporar el contingut del repo `afa-draga/web` a `afa/web/` conservant
   l'historial de git (`git subtree add` o `git filter-repo`).
2. Afegir el workflow de Pages i verificar el desplegament a la URL de pre-prod.
3. Traslladar el domini personalitzat `afaladraga.cat` al nou repositori.
4. Jubilar (arxivar) el repositori `web` antic.

## 7. Principis

- **Simplicitat primer:** el canvi mínim que resol el problema.
- **Zero duplicació:** una sola font per a cada cosa (fets al web, estil a
  `to-de-veu.md`).
- **Zero JS al client** al web (regla heretada del projecte web).
- **Català, llenguatge planer** a totes les sortides.

## 8. Fora d'abast (YAGNI)

- Cap altra skill més enllà de les tres inicials.
- Cap emmagatzematge de comunicats.
- Cap base de dades ni back-end; tot són fitxers de text versionats.
- Cap automatització de l'enviament a WhatsApp/email (es fa manualment).

## 9. Riscos i mitigacions

- **Caiguda del web durant la migració del domini** → migrar el domini només
  després de verificar el nou desplegament; mantenir el repo antic fins llavors.
- **Lectura de dades des d'HTML més sorollosa que d'un markdown net** → acceptat
  conscientment a canvi de tenir una única font de veritat sense deriva.
- **Contingut públic** → no s'hi puja res sensible; els comunicats no es desen.
