# FPV Racer

Simulateur de drone FPV 3D dans le navigateur : acro, rates Betaflight, propwash, tunnels, bando, course et suivi de voitures en drift.

**▶ Démo en ligne : [https://yoannservices.github.io/fpv-racer/](https://yoannservices.github.io/fpv-racer/)**

## Fonctionnalités

- Mode acro réaliste (sans stabilisation) ou angle auto-level pour débuter
- Trois sessions : vol libre, course (9 checkpoints, chrono au tour), suivi de voitures en drift
- Rates Betaflight, propwash, OSD et effet vidéo analogique
- Manette Xbox / PlayStation ou radiocommande USB (EdgeTX / OpenTX en mode Joystick) avec calibrage
- Réglages enregistrés (`localStorage`)

## Commandes

| Touche | Action |
|---|---|
| Z / S (W/S en QWERTY) | Gaz |
| Q / D | Lacet |
| ↑ ↓ | Tangage |
| ← → | Roulis |
| Espace | Armer / désarmer |
| R | Reset |
| C | Caméra |
| Clic + glisser | Tourner autour du drone (vue poursuite) |
| T | Cible |
| H | OSD |
| Échap | Menu |

## Lancer en local

Aucune installation : ouvrez `index.html` dans un navigateur, ou servez le dossier :

```bash
python -m http.server 8000
```

puis allez sur http://localhost:8000 (dans le dossier `web/` de Mes Projets : http://localhost:8000/loisirs/fpv-racer/).

## Structure

- `index.html`, `style.css`
- `js/main.js` : boucle principale
- `js/physics.js` : physique du drone
- `js/input.js` : clavier, manette, radiocommande
- `js/world.js` : la carte
- `js/targets.js` : course et cibles
- `js/fx.js` : effets vidéo
- `js/ui.js`, `js/settings.js` : menus et réglages

## Technologies

HTML, CSS et JavaScript ; rendu 3D avec [Three.js](https://threejs.org/) r186 (chargé depuis un CDN par `assets/three-compat.js`). Sans étape de build.

---

Fait partie de [Mes Projets](https://yoannservices.github.io/) · [Portfolio](https://yoannservices.github.io/portfolio/) · par [@yoannservices](https://github.com/yoannservices)

## Fiche technique

Résumé de ce que fait le code, tenu à jour à chaque modification : le lire suffit pour vérifier une information sans relire tout le jeu.

- **Code** : scripts classiques `js/settings.js`, `input.js`, `physics.js`, `world.js`, `targets.js` (voitures à suivre), `fx.js`, `ui.js`, `main.js`, chargés par `three-compat.js` (`data-scripts`).
- **Drone** : `assets/models/fpv-drone.glb` (un seul maillage : hélices non séparées), `fitGLB(len = 0,36 × P.size)`, abaissé de 0,25 × len. Le châssis procédural est purgé sauf ses hélices et disques de flou, recalés sur les hélices du modèle à (±0,105 ; −0,004 ; ±0,123) (cotes mesurées) : ce sont eux qui tournent (`drone.props`, boucle principale).
- **Voitures suivies** (`targets.js`) : `ferrari-458.glb`, `fitGLB(4.53, 0)`, roues préparées par `rigWheels` avant de retourner le modèle (`rotation.y = π`, avant du jeu sur −Z), puis `poseWheel(w, c.spin, braquage)`. Carrosserie `Material.001` teintée par voiture.
- **Caméras (C)** : Caméra FPV ; Vue poursuite (distance 0,9 + 0,9 × taille, hauteur 0,3 + 0,3 × taille) ; Vue pilote (LOS, depuis le sol).
- **Sauvegarde** : records `fpvsim:best`.

## Crédits des modèles 3D

Véhicules réels sous licence CC-BY (Sketchfab) : auteurs et licences dans `assets/models/CREDITS.md`.
