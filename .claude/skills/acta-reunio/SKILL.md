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
Quan l'usuari validi el contingut, segueix el flux real del web:
1. Genera/desa el PDF amb el nom `YYYYMMDD_assemblea.pdf` o `YYYYMMDD_junta.pdf`
   segons el tipus, amb la data de la reunió (mateix format que les actes ja
   existents a `web/fitxers/actes/`).
2. Desa'l a `web/fitxers/actes/`.
3. Afegeix l'acta al CAPDAMUNT de la llista a `web/afa.html`: just després de
   l'etiqueta `<ul class="acta-list" ...>`, insereix un element nou amb aquesta
   estructura exacta (mateix patró que els elements existents):

   ```html
   <li><a href="fitxers/actes/YYYYMMDD_tipus.pdf" rel="external noopener" target="_blank">
     <span class="date">D mmm YYYY</span>
     <span class="title">Títol</span>
     <span class="ext">PDF ↗</span>
   </a></li>
   ```

   On la data va en format curt català (p. ex. "4 jun 2026") i el títol és planer
   (p. ex. "Assemblea" o "Junta").
4. Mantén el codi HTML net i coherent amb la resta de la llista.
