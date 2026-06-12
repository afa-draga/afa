---
name: contingut-web
description: Usa quan calgui afegir o actualitzar contingut al web de l'AFA — una pàgina nova, una secció, un document a fitxers/, o modificar text o estructura existents.
---

# Incorporar contingut al web

Afegeix o actualitza contingut a `web/` mantenint l'estil, l'estructura i
l'accessibilitat existents. El web és la font de veritat: aquí no s'inventa
estructura nova, es **reflecteix** la que ja hi ha.

## Procediment
1. Aplica `veu-afa` per a tot el text (català, comunicació clara).
2. Aplica `identitat-afa` per a qualsevol element visual (colors de
   `web/assets/styles.css`; no inventis estils).
3. **Abans d'editar, llegeix una pàgina existent semblant** a `web/` (p. ex.
   `comissions.html`, `menjador.html`) i imita'n l'estructura, les classes i els
   components. Reutilitza classes existents (`.wrap`, `.block`, `.btn`,
   `.eyebrow`, `.lead`…); no afegeixis estils en línia.
4. Fes el canvi mínim que resol la tasca.

## Regles del web (obligatòries)
- **Zero JS al client.** Tota interacció amb HTML i CSS.
- HTML semàntic (`<header>`, `<main>`, `<nav>`, `<section>`, `<article>`).
- Accessibilitat: `lang="ca"`, `alt` descriptius, contrast alt, `aria-current` a
  l'enllaç de la pàgina activa, enllaç "Salta al contingut".
- Documents a `web/fitxers/`: nom `YYYYMMDD_titol-del-fitxer.pdf`.

## Casos habituals
- **Pàgina nova:** parteix d'una pàgina existent com a plantilla. Afegeix l'enllaç
  a la navegació `<nav class="main-nav">`, que es repeteix a **totes** les pàgines
  HTML: actualitza-la a cadascuna i posa `aria-current="page"` a la pàgina activa.
- **Document nou (menú, normativa…):** anomena'l `YYYYMMDD_*.pdf`, desa'l a la
  carpeta de `web/fitxers/` corresponent i afegeix l'enllaç al capdamunt de la
  llista pertinent (ordre cronològic invers).
- **Actes de reunió:** usa la skill `acta` (ja cobreix el flux complet).

## Verificació
Obre la pàgina al navegador i comprova que es veu bé i que els enllaços funcionen
abans de pujar. El desplegament a GitHub Pages és automàtic en fer push a `master`.
