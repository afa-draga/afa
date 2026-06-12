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
