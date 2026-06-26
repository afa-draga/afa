# Presentacions

Presentacions de l'AFA en format Marp. El `.md` és l'única font: genera el PDF i la versió HTML.

## Regenerar la versió HTML navegable

Després de modificar `20260619_Presentacio_Benvinguda_Families.md`, regenera l'HTML
publicat (cal `--no-stdin` o el procés es penja esperant stdin):

```
npx -y @marp-team/marp-cli@latest -c presentacions/marp.config.mjs presentacions/20260619_Presentacio_Benvinguda_Families.md --theme presentacions/temes/afa.css --html --no-stdin -o web/presentacions/benvinguda-families.html
```

El flag `-c presentacions/marp.config.mjs` desactiva la conversió d'emojis a imatges
twemoji via CDN, cosa que fa l'HTML autocontingut i projectable sense connexió a internet.

L'HTML es genera sota `web/` perquè el desplegament a GitHub Pages només publica
aquesta carpeta. Es committeja l'artefacte generat (com el PDF).

## Animació

- `transition: fade` al front-matter: transició suau entre diapositives.
- Llistes amb `*` (en comptes de `-`): es revelen punt a punt (fragments).
