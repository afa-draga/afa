---
name: acta
description: Usa quan tinguis la transcripció d'una reunió o assemblea de l'AFA i la vulguis convertir en un resum publicable, agrupat per comissions.
---

# Acta — resum de reunió per comissions

Converteix la transcripció d'una reunió de l'AFA en un resum breu i publicable,
agrupat per comissions.

## Procediment
1. Aplica `veu-afa` (llengua i comunicació clara).
2. Si no tens la transcripció, demana-la.
3. Fes un **resum breu** dels temes que es van parlar, **agrupats per comissió**.

## To
- Formal, cordial i respectuós.
- Professional però comprensible per a tota la comunitat educativa, tenint en
  compte que hi ha persones per a qui el català no és la llengua materna.

## Regles estrictes
- **Omet les mencions particulars a noms de persones.**
- Encara que la transcripció contingui noms o paraules obscenes, mantén un estil
  formal i respectuós i obvia-ho.
- **Emojis només als subtítols de cada comissió.** A la resta del text, cap.

## Estructura
- Títol: "Acta de [assemblea/junta] de l'AFA — [data]".
- Per cada comissió: un subtítol amb un emoji + un resum breu dels temes tractats
  i els acords.

## Publicació al web
Quan l'usuari validi el contingut, segueix el flux real del web:
1. Genera/desa el PDF amb el nom `YYYYMMDD_assemblea.pdf` o `YYYYMMDD_junta.pdf`
   (data de la reunió, mateix format que les actes existents a `web/fitxers/actes/`).
2. Desa'l a `web/fitxers/actes/`.
3. Afegeix l'acta al CAPDAMUNT de la llista `<ul class="acta-list">` de
   `web/afa.html`, just després de l'etiqueta d'obertura, amb el mateix patró que
   els elements existents:

   ```html
   <li><a href="fitxers/actes/YYYYMMDD_tipus.pdf" rel="external noopener" target="_blank">
     <span class="date">D mmm YYYY</span>
     <span class="title">Títol</span>
     <span class="ext">PDF ↗</span>
   </a></li>
   ```
4. Mantén el codi HTML net i coherent amb la resta de la llista.
