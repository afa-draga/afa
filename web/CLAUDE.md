# Context del Projecte: Web AFA Escola La Draga

## 1. Visió General
- **Entitat:** AFA de l'Escola La Draga (Banyoles).
- **Objectiu:** Crear un web estàtic, extremadament ràpid, accessible i fàcil de mantenir.
- **Públic:** Famílies de l'escola i comunitat educativa. El llenguatge ha de ser planer i entenedor, adaptat fins i tot per a infants de 6 anys.
- **Entorns:**
  - **Producció:** https://afaladraga.cat/
  - **Pre-producció:** https://afa-draga.github.io/afa/

## 2. Stack Tecnològic i Regles Tècniques
- **Plataforma:** GitHub Pages.
- **Llenguatges:** HTML5 pur (semàntic) i CSS.
- **Framework CSS:** Tailwind CSS (via CDN o build process simple).
- **Restricció Crítica:** **Zero JS**. Prohibit l'ús de JavaScript al client. Totes les interaccions i estils han de funcionar exclusivament amb HTML i CSS.

## 3. Identitat Visual i Accessibilitat
- **Logotip:** Blau corporatiu basat en el fitxer `afa_blanc.jpg`.
- **Paleta:** Blaus (#0056b3 aprox.) i blancs. Estil net, institucional i proper.
- **Tipografia:** Sans-serif de fàcil lectura amb mides de font generoses.
- **Accessibilitat (A11y):** Ús estricte d'etiquetes semàntiques (`<nav>`, `<main>`, `<article>`, `<section>`). Alt contrast i textos `alt` descriptius en totes les imatges.

## 4. Estructura de Fitxers i Directoris
- `/` : Arrel amb els fitxers HTML de les pàgines.
- `/assets/` : Totes les imatges, el logotip i els estils del web.
- `/fitxers/menjador/` : PDFs de menús mensuals i normativa.
- `/fitxers/actes/` : PDFs de les actes de les assemblees i juntes.

### Regla de Nomenclatura de Fitxers
Tots els fitxers nous pujats a `/fitxers/` han de seguir el format estricte:
`YYYYMMDD_titol-del-fitxer.pdf`

## 5. Arquitectura d'Informació (Jerarquia)
- **Inici:** Portada principal.
- **AFA:** Qui som i què fem.
  - **Actes de la Junta:** Llistat cronològic a `afa.html` (`<ul class="acta-list">`) amb enllaços a `/fitxers/actes/`.
- **Comissions:** Pàgina índex i pàgines individuals (Menjador, Extraescolars, Festes, Carnaval, Comunicació, Eduquem en família, Coeducació i diversitat, Patí, Esportiva).
  - *Estructura Comissió:* Seccions "Què fem" i "Qui som" (Nom sense cognom).
- **Extraescolars:** Graella horària i informació d'inscripció.
- **Menjador:** Enllaços a menús mensuals i normativa.
- **Escola:** Enllaç extern al web oficial.
- **Contacte:** Pàgina única amb email (ampa@ceipladraga.cat), QR i telèfon de la Comunitat de WhatsApp, i dades del Coordinador de menjador (mòbil i horari).

## 6. Workflows d'Actualització (Regles per a la IA)
Quan es rebi un nou document (ex: Menú de maig o una Acta):
1. **Renombrar:** Assignar el nom segons el format `YYYYMMDD_titol.pdf`.
2. **Classificar:** Ubicar el fitxer a la carpeta corresponent (`/fitxers/menjador/` o `/fitxers/actes/`).
3. **Actualitzar HTML:**
   - Obrir el fitxer HTML pertinent (`menjador.html` o `afa.html`).
   - Inserir el nou enllaç al capdamunt de la llista (ordre cronològic invers).
   - Assegurar que el text de l'enllaç sigui planer i el codi es mantingui net.

## 7. Principis de Redacció
- **Idioma:** Català (única llengua).
- **Estil:** Frases curtes, clares i directes. Evitar tecnicismes innecessaris per mantenir l'accessibilitat cognitiva.
