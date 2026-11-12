# Tests de compatibilité

[← Retour au projet](../README.md)

## 1. Périmètre

| Dimension | Configurations | Pourquoi |
|---|---|---|
| **Navigateur** | Chrome 153 | Le navigateur le plus utilisé ; tous les essais sont faits dans le même navigateur pour isoler l'effet de la taille d'écran |
| **Tailles d'écran** | 375 px (mobile), 768 px (tablette), 1 280 px (portable), 1 920 px (grand écran) | Les points de rupture courants |
| **Zoom** | 200 % (fenêtre de 1 280 px → équivaut à 640 px) | Exigence d'accessibilité (WCAG 1.4.4 et 1.4.10) |

**Parcours vérifié à chaque configuration**

- Médiathèque : afficher le catalogue, ajouter un livre, emprunter, rendre, supprimer.
- Générateur : saisir 20, cocher les 4 critères, générer, lire le mot de passe et l'indicateur.

**Ce qui est contrôlé** : contenu entier visible et atteignable, pas de défilement horizontal, textes non coupés, boutons utilisables, fonctionnement identique.

## 2. Matrice des résultats

✅ OK · ⚠️ défaut d'affichage mineur · ❌ contenu coupé ou inutilisable

### Médiathèque

| Taille | Résultat | Constat |
|---|:---:|---|
| 375 px | ❌ | Le tableau (648 px) déborde : toute la page défile horizontalement, l'en-tête et les cadres s'arrêtent à 375 px ([capture](captures/mediatheque-mobile-375.png)) → **COMP-04** |
| 768 px | ✅ | [capture](captures/mediatheque-tablette-768.png) |
| 1 280 px | ✅ | [capture](captures/mediatheque-portable-1280.png) |
| 1 920 px | ✅ | [capture](captures/mediatheque-ecran-1920.png) |
| Zoom 200 % | ❌ | Même débordement du tableau (648 px pour 640 px) ([capture](captures/mediatheque-zoom200-640.png)) → **COMP-04** |

### Générateur de mot de passe

| Taille | Résultat | Constat |
|---|:---:|---|
| 375 px | ❌ | L'indicateur de sécurité (504 px) est coupé **à gauche** : la zone « Mauvais » est inatteignable, même en faisant défiler. Titre coupé en haut, mot de passe de 20 caractères qui sort de son cadre ([capture](captures/generateur-mobile-375.png)) → **COMP-01**, **COMP-02** |
| 768 px | ⚠️ | Zone « Très bon » qui dépasse du cadre de l'indicateur ([capture](captures/generateur-tablette-768.png)) → **COMP-03** |
| 1 280 px | ⚠️ | Idem COMP-03 ([capture](captures/generateur-portable-1280.png)) |
| 1 920 px | ✅ | [capture](captures/generateur-ecran-1920.png) |
| Zoom 200 % | ⚠️ | Utilisable ; idem COMP-03 ([capture](captures/generateur-zoom200-640.png)) |

**Fonctionnement** : les deux parcours donnent le même résultat à toutes les tailles d'écran ; seuls l'affichage et l'accès au contenu changent.

## 3. Analyse

| Cause | Anomalies | Correction à proposer |
|---|---|---|
| Largeurs fixes en pixels (`<canvas width="500px">`, tableau à 5 colonnes) | COMP-01, COMP-04 | Canvas redimensionné à la largeur disponible ; tableau dans un conteneur à défilement, ou affiché en cartes sous 600 px |
| Contenu centré plus large que l'écran (`align-items: center`) | COMP-01 | Un débordement centré part des **deux côtés** : la partie gauche ne peut pas être atteinte |
| Hauteur fixe de l'en-tête (`height: 10vh`) avec un titre de 3rem | COMP-02 | Hauteur automatique, taille de titre adaptée au mobile |
| Canvas de 500 px fixes, plus large que l'intérieur de son cadre ; la zone « Très bon » est en plus dessinée jusqu'à 530 px, donc tronquée | COMP-03 | Canvas à la largeur du cadre, coordonnées corrigées dans `canvas.js` |
