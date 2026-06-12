---
name: identitat-afa
description: Usa quan creïs o revisis qualsevol material visual de l'AFA — presentacions, gràfics, cartells o elements del web.
---

# Identitat visual de l'AFA Escola La Draga

La identitat visual NO es duplica: la font canònica és el full d'estil del web,
`web/assets/styles.css` (variables CSS). Qualsevol material visual ha de
**reflectir** aquests valors; no inventis colors nous.

## Paleta (variables de `web/assets/styles.css`)
| Variable | Hex | Ús |
|---|---|---|
| `--blue` | `#0056b3` | Blau corporatiu principal |
| `--blue-dark` | `#003d7a` | Blau fosc (enllaços, èmfasi) |
| `--blue-darker` | `#002851` | Blau més fosc |
| `--blue-light` | `#e6eff8` | Fons blau clar |
| `--blue-lighter` | `#f4f8fc` | Fons blau molt clar |
| `--accent` | `#f5c842` | Groc d'accent |
| `--accent-soft` | `#fdf5d6` | Groc suau (fons) |
| `--bg` | `#fbf9f4` | Fons general |
| `--surface` | `#ffffff` | Superfícies / targetes |
| `--ink` | `#14202c` | Text principal |
| `--ink-soft` | `#4a5763` | Text secundari |
| `--line` | `#e6e2d6` | Línies / separadors |

## Logotip i tipografia
- Logotip a `web/assets/` (blau corporatiu sobre fons clar).
- Tipografia: sans-serif de fàcil lectura, mides generoses.

## Regla d'or
Si la paleta del web canvia, actualitza els derivats (p. ex. el tema Marp
`presentacions/temes/afa.css`) perquè segueixin reflectint `web/assets/styles.css`.
