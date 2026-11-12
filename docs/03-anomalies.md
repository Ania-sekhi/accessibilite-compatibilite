# Anomalies

[← Retour au projet](../README.md)

12 anomalies : 8 d'accessibilité (ACC), 4 de compatibilité (COMP). Elles sont prêtes à importer dans Jira : [`anomalies/jira-anomalies.csv`](../anomalies/jira-anomalies.csv).

**Sévérité** : *Critique* = une partie des utilisateurs ne peut pas utiliser la fonction · *Majeure* = utilisation difficile ou information perdue · *Mineure* = gêne, contournement simple.

| ID | Application | Titre | Référence | Sévérité | Trouvée par |
|---|---|---|---|:---:|---|
| ACC-01 | Générateur | Contraste insuffisant du bouton « Copier le code » (3,94:1) | RGAA 3.2 · WCAG 1.4.3 | Majeure | axe-core |
| ACC-02 | Générateur | Indicateur de sécurité sans alternative textuelle : niveau inconnu au lecteur d'écran | RGAA 1.1 · WCAG 1.1.1 | **Critique** | Manuel |
| ACC-03 | Générateur | Niveau de sécurité indiqué uniquement par la couleur | RGAA 3.1 · WCAG 1.4.1 | Majeure | Manuel |
| ACC-04 | Générateur | Textes de l'indicateur illisibles (blanc sur jaune 1,07:1, sur orange 1,97:1) | RGAA 3.2 · WCAG 1.4.3 | Majeure | Manuel (calcul des contrastes) |
| ACC-05 | Générateur | Mot de passe généré non annoncé aux technologies d'assistance | RGAA 7.5 · WCAG 4.1.3 | Majeure | Manuel |
| ACC-06 | Générateur | Images décoratives avec alternative : bouton lu « Copier Copier le code » | RGAA 1.2 · WCAG 1.1.1 | Mineure | Manuel |
| ACC-07 | Médiathèque | Champ « Emprunteur » sans étiquette (placeholder seul) | RGAA 11.1 · WCAG 3.3.2 | Majeure | Manuel |
| ACC-08 | Médiathèque | Champ ISBN invalide non signalé en erreur (`aria-invalid` absent) | RGAA 11.10 · WCAG 3.3.1 | Mineure | Manuel |
| COMP-01 | Générateur | Mobile 375 px : indicateur coupé à gauche, inatteignable | RGAA 10.11 · WCAG 1.4.10 | Majeure | Test d'affichage |
| COMP-02 | Générateur | Mobile 375 px : titre coupé en haut, mot de passe hors de son cadre | | Mineure | Test d'affichage |
| COMP-03 | Générateur | Zone « Très bon » qui dépasse du cadre (768 à 1 280 px) | | Mineure | Test d'affichage |
| COMP-04 | Médiathèque | Mobile 375 px et zoom 200 % : le tableau fait défiler toute la page horizontalement | RGAA 10.11 · WCAG 1.4.10 | Majeure | Test d'affichage |

---

## Exemple de fiche : ACC-02

| Champ | Valeur |
|---|---|
| **Titre** | Indicateur de sécurité sans alternative textuelle : niveau inconnu au lecteur d'écran |
| **Application / page** | Générateur de mot de passe, fieldset « Indicateur de sécurité » |
| **Environnement** | Chrome 153, Linux, 1 280 × 900 |
| **Référence** | RGAA 4.1 critère 1.1 · WCAG 2.1 critère 1.1.1 (niveau A) |
| **Sévérité / priorité** | Critique / P1 |
| **Étapes** | 1. Saisir 12, cocher « Minuscules », cliquer sur « Générer »<br>2. Inspecter l'élément `<canvas id="canvas">` |
| **Résultat attendu** | Le niveau atteint (« Bon ») est disponible en texte : contenu de repli du canvas, `role="img"` + `aria-label`, ou texte visible à côté |
| **Résultat obtenu** | Le canvas est vide (aucun contenu de repli), sans rôle ni nom accessible. Le niveau n'existe que sous forme de pixels colorés |
| **Impact** | Un utilisateur aveugle ne peut pas savoir si son mot de passe est sûr : la fonction de l'US 3 lui est inaccessible |
| **Pourquoi l'outil automatique ne le voit pas** | axe-core ne peut pas lire le contenu d'un canvas ; il ne vérifie pas qu'un canvas porteur d'information a une alternative |
| **Proposition** | `<p id="niveau" role="status">Sécurité : Bon</p>` mis à jour à chaque génération (corrige aussi ACC-03) |

## Exemple de fiche : COMP-04

| Champ | Valeur |
|---|---|
| **Titre** | Mobile 375 px et zoom 200 % : le tableau fait défiler toute la page horizontalement |
| **Environnement** | Chrome 153, fenêtre 375 × 812 ; puis 640 × 450 (zoom 200 %) |
| **Référence** | RGAA 10.11 · WCAG 1.4.10 (Redistribution) |
| **Sévérité / priorité** | Majeure / P2 |
| **Étapes** | 1. Ouvrir la Médiathèque sur un écran de 375 px de large<br>2. Faire défiler vers la droite pour atteindre les boutons « Emprunter » |
| **Résultat attendu** | Tout le contenu est lisible sans défilement horizontal de la page |
| **Résultat obtenu** | La page fait 648 px de large : le tableau dépasse, l'en-tête vert et le cadre « Ajouter un livre » s'arrêtent à 375 px ([capture](captures/mediatheque-mobile-375.png)) |
| **Proposition** | Tableau dans un conteneur `overflow-x: auto`, ou affichage en cartes sous 600 px |
