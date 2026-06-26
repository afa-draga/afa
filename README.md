# AFA Escola La Draga — coneixement i eines

Monorepo de l'AFA de l'Escola La Draga (Banyoles): el web públic, el coneixement
reutilitzable i les skills de Claude Code per generar comunicacions coherents.

## Estructura

Dues capes: **el substrat** (el coneixement reutilitzable que ho sosté tot) i **el
web públic**, que es construeix a sobre.

```
afa-draga/afa/                       MONOREPO
│
├── web/                             WEB PÚBLIC
│   │                                  FONT DE VERITAT dels fets
│   ├── index.html                   portada
│   ├── afa.html                     l'AFA + actes (<ul class="acta-list">)
│   ├── comissions.html
│   ├── extraescolars.html
│   ├── menjador.html
│   ├── contacte.html
│   ├── assets/
│   │   ├── styles.css               ⭐ FONT CANÒNICA de la identitat visual
│   │   └── logo-afa.jpg
│   ├── fitxers/
│   │   ├── actes/                   YYYYMMDD_*.pdf
│   │   └── menjador/                YYYYMMDD_*.pdf
│   ├── presentacions/               decks HTML navegables (artefactes Marp)
│   ├── CLAUDE.md                    regles del web (Zero JS, semàntica…)
│   ├── CNAME · .nojekyll            afaladraga.cat + desplegament
│
├── .claude/skills/
│   ├── veu-afa/                     SUBSTRAT — com escriu l'AFA (to, llengua)
│   ├── identitat-afa/               SUBSTRAT — identitat visual → styles.css
│   ├── comunicat/                   EINA — WhatsApp a les famílies
│   ├── acta/                        EINA — resum de reunió per comissions
│   ├── presentacio/                 EINA — Marp (tema reflecteix styles.css)
│   └── contingut-web/               EINA — afegir/editar contingut al web
│
├── presentacions/                   fonts .md de l'eina presentacio
│   ├── temes/afa.css                tema Marp (DERIVAT de styles.css)
│   ├── marp.config.mjs              config Marp (emojis natius, sense CDN)
│   ├── README.md                    com regenerar el deck HTML
│   └── exemple.md
│
├── .github/workflows/pages.yml      desplega només web/ (push a master)
├── docs/superpowers/                disseny i pla (registre)
├── CLAUDE.md · README.md · .gitignore
```

## Principis

- **Una sola font de veritat:** els *fets* viuen al `web/`; l'*estil i el to*, a la
  skill `veu-afa`; la *identitat visual*, a `web/assets/styles.css`. Tot el demés
  hi **apunta** — res es duplica.
- **Substrat vs eines:** el coneixement reutilitzable (`veu-afa`, `identitat-afa`)
  és el substrat; cada eina de lliurament (`comunicat`, `acta`, `presentacio`,
  `contingut-web`) s'hi construeix a sobre. Afegir-ne una de nova no toca el substrat.
- **Català, llenguatge planer.** Frases curtes i clares.
- **Zero JS** al client del web institucional. Excepció: els decks HTML a
  `web/presentacions/` són artefactes de Marp i porten JS de navegació.

## Com s'usa

Obre aquest repo amb Claude Code i demana, per exemple, "redacta un comunicat per
a les famílies sobre…" o "fes un resum d'aquesta acta". Les skills s'activen soles.
