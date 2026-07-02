# CLAUDE.md — Triches GTA V

Brief projet pour Claude Code. Lis ce fichier avant toute modification.

## Contexte
Site de référence **mobile-first** des codes de triche de **GTA V (mode histoire / solo uniquement)**.
But : consulter et copier un code en un tap depuis le téléphone. Fan-site, gratuit, hébergé sur GitHub Pages.
Propriétaire : Gabriel (`stesouna9`).

## État actuel — hub multi-jeux
- **Déployé** sur GitHub Pages : https://stesouna9.github.io/triches-gta5/ (repo `Stesouna9/triches-gta5`, branche `main`, root).
- `index.html` = **hub** (choix du jeu, liste `HUB` dans le script).
- `game.html` = **moteur générique** piloté par `data.js`, via `?g=<id>`. Gère plateformes variables par jeu (type `word` = clavier PC, `ps`/`xbox` = combos pastilles, `phone` = numéro).
- `data.js` = `window.GAMES` : San Andreas (57), Vice City (43), GTA III (22), GTA IV (26). Chaque jeu : `platforms[]`, `cats[]`, `howto`, `warn`, `cheats[]`.
- `gta5.html` = page GTA V autonome (moteur d'origine, non migré, 36 codes). `bourse.html` = plan bourse (GTA V). Nav Jeux/Triches/Bourse.
- **PWA** : `manifest.json` + `sw.js` (cache v4, hors-ligne) + icônes. Installable.
- Preview local : macOS bloque l'accès Python (sandbox) à `~/Downloads` → http.server 404. Copier le site dans le scratchpad pour le preview. Git/gh OK.
- Pour **ajouter un jeu** : 1 entrée dans `GAMES` (data.js) + 1 carte dans `HUB` (index.html). Pour **ajouter un code** : 1 objet dans le tableau `cheats` du jeu.
- Fiabilité données : codes clavier PC = les plus fiables (cross-vérifiés). Certaines combos PS2/Xbox dérivées par mapping, quelques `null` là où incertain. Météo VC/III : libellés PC clavier = point le moins sûr.

## Stack & contraintes
- **Vanilla HTML/CSS/JS dans un seul fichier.** Aucun build, aucune dépendance, aucun framework.
- Seule ressource externe : Google Fonts (`Anton`, `Archivo`, `JetBrains Mono`).
- **Garder le mono-fichier** sauf décision explicite de Gabriel. Pas de bundler, pas de npm.
- Cible : GitHub Pages (statique pur). Donc `localStorage`/`sessionStorage` **autorisés** ici (contrairement aux artifacts Claude.ai).
- Accessibilité : focus clavier visible, `prefers-reduced-motion` respecté → maintenir.

## Design tokens (à respecter pour toute évolution)
Direction : « Los Santos au crépuscule » — charbon désaturé, sable, ambre de smog.
```
--bg:#13181a  --bg2:#1a2124  --surface:#1f282b  --surface2:#26312f  --line:#33403e
--bone:#ECE4D2 (texte)  --muted:#8c958f
--amber:#F0A437 (accent principal)  --money:#54bd80 (succès/copie)
--vinewood:#E8487E (danger)  --gold:#F2C14E (avertissement/étoiles)
Boutons PS (couleurs classiques) : tri #62cf8b, circle #e87a90, cross #6fa6e6, square #d68fcf
```
Typo : `Anton` (titres condensés, parcimonie) · `Archivo` (UI/corps) · `JetBrains Mono` (codes).

## Modèle de données
Tout est dans la constante `CHEATS` (tableau d'objets) dans `index.html` :
```js
{ cat, name, tag?:[type,label], phone, ps, xbox, pc }
```
- `cat` : `"combat" | "move" | "world" | "veh" | "unlock"`
- `tag` (optionnel) : `["time"|"risk"|"unlock", "libellé"]`
- `ps` / `xbox` : chaîne `"Droite, X, R1, ..."` (tokens séparés par `", "`), ou `null` si pas de combo (cas des véhicules à débloquer).
- `phone` : `"1-999-..."` · `pc` : mot-clé console.

Rendu :
- `renderCombo(str, plat)` transforme une combo en pastilles de boutons (mapping `PS_FACE`, `XB_FACE`, `DIRS`).
- Onglet actif piloté par `document.body.dataset.platform` (`phone|ps|xbox|pc`), affichage CSS via `.cv-*`.
- Copie au tap : `copyCheat()` (copie le n° sans tirets en mode téléphone), toast + `navigator.vibrate`.

Pour **ajouter un code** : un seul objet dans `CHEATS`, rien d'autre à toucher (les sections/compteurs se génèrent).

## Tâche immédiate — déployer
1. Repo `stesouna9/triches-gta5`, `index.html` à la racine.
2. GitHub Pages : Settings → Pages → Source `main` / `(root)`.
3. URL cible : `https://stesouna9.github.io/triches-gta5/`.
   - Alternative : intégrer comme sous-dossier d'OKALAM → `okalamstudio/gta/index.html`
     (adapter `<title>` et l'URL du README en conséquence).

## Backlog (par ordre suggéré)
1. **Bilingue FR/EN** : toggle de langue (Gabriel travaille en bilingue). Externaliser les libellés UI + noms de triches dans un objet `i18n`, persister le choix en `localStorage`. (À étendre à `bourse.html`.)
2. **Favoris** : étoile par carte, persistés en `localStorage`, + filtre « Favoris ». (Possible ici car site réel, pas artifact.)
3. ~~**PWA légère**~~ — FAIT (manifest + sw.js + icônes).
4. **Intégration OKALAM** si choisie : harmoniser nav/retour vers l'écosystème.

## bourse.html
Page statique (pas de données JS dynamiques) : les 5 missions d'assassinat de Lester en cartes `.mission`, chacune avec étapes `.step` (acheter avant/après). Données sourcées (gtabase / GTA Wiki / G2A) : RWC Redwood = jackpot ~+300%, objectif ~2 Mds$/perso. Si refonte en données : sérialiser en tableau `INVEST` similaire à `CHEATS`.

## Préférences de travail (Gabriel)
- Direct et honnête, sans caveats répétés. Dire les choses une fois, clairement.
- Montrer les drafts/diffs clairement avant d'appliquer du lourd.
- Ne pas casser le mono-fichier ni alourdir avec des dépendances sans raison.
