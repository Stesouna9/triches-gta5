# Triches GTA — Mode histoire

Référence mobile-first des codes de triche des jeux GTA (mode solo).
Recherche live, onglets par méthode, combos manette en pastilles de boutons, copie au tap.
Installable comme app (PWA, hors-ligne).

**En ligne :** https://stesouna9.github.io/triches-gta5/

## Jeux couverts
- **GTA V** (`gta5.html`) — Téléphone / PS / Xbox / PC + page **Bourse** (`bourse.html`)
- **San Andreas · Vice City · GTA III** — clavier PC + combos PS2/Xbox (`game.html?g=…`)
- **GTA IV** — codes téléphone (`game.html?g=gta4`)

## Architecture
- `index.html` : hub, choix du jeu.
- `game.html` : moteur générique piloté par `data.js` (`?g=<id>`).
- `data.js` : base des codes des jeux classiques + IV (`window.GAMES`).
- `gta5.html` : page GTA V autonome (moteur dédié). `bourse.html` : plan d'investissement bourse.
- PWA : `manifest.json`, `sw.js`, icônes.

Vanilla HTML/CSS/JS, aucune dépendance (sauf Google Fonts), aucun build.

## Déploiement
GitHub Pages, branche `main`, source `/(root)`. Pousser suffit — Pages redéploie.

---
Construit pour Gabriel.
