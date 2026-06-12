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
