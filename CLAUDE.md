# CLAUDE.md — Triches GTA V

Brief projet pour Claude Code. Lis ce fichier avant toute modification.

## Contexte
Site de référence **mobile-first** des codes de triche de **GTA V (mode histoire / solo uniquement)**.
But : consulter et copier un code en un tap depuis le téléphone. Fan-site, gratuit, hébergé sur GitHub Pages.
Propriétaire : Gabriel (`stesouna9`).

## État actuel
- **Déployé** sur GitHub Pages : https://stesouna9.github.io/triches-gta5/ (repo `Stesouna9/triches-gta5`, branche `main`, root).
- `index.html` : triches (36 codes). `bourse.html` : plan d'investissement bourse (assassinats Lester). Nav Triches⇄Bourse en haut des deux pages.
- **PWA** : `manifest.json` + `sw.js` (cache hors-ligne) + icônes (`icon-192/512/180.png`, `favicon.png`). Installable sur écran d'accueil.
- Preview local : macOS bloque l'accès Python (sandbox) à `~/Downloads` → http.server renvoie 404. Copier le site ailleurs (ex. scratchpad) pour le preview. Git/gh OK.

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
