# AFA Escola La Draga — coneixement i eines

Monorepo de l'AFA de l'Escola La Draga (Banyoles): el web públic, el coneixement
reutilitzable i les skills de Claude Code per generar comunicacions coherents.

## Estructura

Llegida amb el marc d'*[Under the River](https://shopify.engineering/under-the-river)*:
**l'aqüífer** (el substrat invisible que ho sosté tot) i **el riu** (la superfície
pública, que es modela a sobre).

```
afa-draga/afa/                       MONOREPO (un sol pou)
│
├── web/                             🌊 EL RIU — superfície pública
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
│   ├── CLAUDE.md                    regles del web (Zero JS, semàntica…)
│   ├── CNAME · .nojekyll            afaladraga.cat + desplegament
│
├── .claude/skills/
│   ├── veu-afa/                     💧 AQÜÍFER — com escriu l'AFA (to, llengua)
│   ├── identitat-afa/               💧 AQÜÍFER — identitat visual → styles.css
│   ├── comunicat/                   🚣 PERFIL — WhatsApp a les famílies
│   ├── acta/                        🚣 PERFIL — resum de reunió per comissions
│   ├── presentacio/                 🚣 PERFIL — Marp (tema reflecteix styles.css)
│   └── contingut-web/               🚣 PERFIL — afegir/editar contingut al web
│
├── presentacions/                   sortides del perfil presentacio
│   ├── temes/afa.css                tema Marp (DERIVAT de styles.css)
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
- **Substrat vs perfils:** el coneixement reutilitzable (`veu-afa`, `identitat-afa`)
  és l'aqüífer; cada lliurament (`comunicat`, `acta`, `presentacio`, `contingut-web`)
  és un perfil prim a sobre. Afegir un lliurament nou no toca el substrat.
- **Català, llenguatge planer.** Frases curtes i clares.
- **Zero JS** al client del web.

## Com s'usa

Obre aquest repo amb Claude Code i demana, per exemple, "redacta un comunicat per
a les famílies sobre…" o "fes un resum d'aquesta acta". Les skills s'activen soles.
