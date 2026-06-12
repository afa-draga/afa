---
name: presentacio
description: Usa quan calgui crear una presentació de l'AFA — per a assemblees, juntes o xerrades.
---

# Crear una presentació

Genera presentacions amb Marp (Markdown → diapositives) amb la identitat de l'AFA.

## Procediment
1. Aplica `veu-afa` (poc text per diapositiva, frases curtes, llenguatge planer).
2. Aplica `identitat-afa` (colors i logo segons `web/assets/styles.css`).
3. Llegeix del `web/` les dades que necessitis (font de veritat).
4. Si falta informació, pregunta: tema, públic, durada aproximada, missatges clau.
5. Crea un fitxer Markdown de Marp a `presentacions/[nom].md` amb aquesta capçalera:

   ```markdown
   ---
   marp: true
   theme: afa
   paginate: true
   ---
   ```

   El tema `afa` és a `presentacions/temes/afa.css`.
6. Estructura recomanada: portada (classe `lead`), índex breu, una idea per
   diapositiva, diapositiva de tancament amb contacte (llegit del web).
7. Per exportar, indica a l'usuari l'ordre (no cal executar-la si no ho demana):
   `npx @marp-team/marp-cli presentacions/[nom].md --theme presentacions/temes/afa.css -o presentacions/[nom].pdf`

## Recordatori
- Poc text per diapositiva; el detall es diu de viva veu.
- Mantén la identitat: blau corporatiu, groc d'accent i blanc (ja al tema).
