# Audit d'accessibilité

[← Retour au projet](../README.md)

## 1. Méthode

| Étape | Outil | Ce qu'elle trouve | Limite |
|---|---|---|---|
| **Analyse automatique** | Moteur **axe-core 4.14** (celui de l'extension *axe DevTools*), règles WCAG 2.1 A et AA + bonnes pratiques | Contrastes du HTML, attributs manquants, structure | Ne voit **rien** dans un `<canvas>`, accepte un `placeholder` comme étiquette, ne juge pas la pertinence |
| **Vérification manuelle** | Grille **RGAA 4.1** (échantillon de 15 critères), navigation **au clavier seul** | Ce que l'automatique ne peut pas juger | Plus long |
| **Zoom et petits écrans** | Fenêtre de 640 px (= zoom 200 % sur un écran de 1 280 px) et 375 px (mobile) | Contenu coupé, défilement horizontal | |

> Une analyse automatique trouve en général une **petite partie** des problèmes. Ici, axe-core signale **1** anomalie ; la vérification manuelle et les essais au zoom en trouvent **9 autres**.

Pages auditées : la Médiathèque (catalogue avec 3 livres, dont 1 emprunté) et le générateur de mot de passe (avant et après une génération).

## 2. Résultats de l'analyse automatique (axe-core)

| Page | Violations | Détail |
|---|:---:|---|
| Médiathèque | **0** | |
| Générateur, avant génération | **1** | `color-contrast` (sérieux) : bouton « Copier le code », blanc sur gris `#808080` = **3,94:1** au lieu de 4,5:1 |
| Générateur, après génération | **1** | Même anomalie |

**0 violation ne veut pas dire « accessible »** : la Médiathèque a un champ sans étiquette et une mise en page qui casse au zoom (voir plus bas), qu'axe-core ne signale pas.

## 3. Vérification manuelle : grille RGAA (échantillon)

✅ conforme · ❌ non conforme · — non applicable

| Critère RGAA 4.1 | WCAG | Médiathèque | Générateur | Constat |
|---|---|:---:|:---:|---|
| 1.1 Image porteuse d'information : alternative | 1.1.1 | — | ❌ | Le `<canvas>` de l'indicateur de sécurité n'a **aucune alternative** : un lecteur d'écran ne sait pas si le mot de passe est « Bon » ou « Mauvais » (ACC-02) |
| 1.2 Image décorative ignorée | 1.1.1 | — | ❌ | Cadenas décoratif avec `alt="Image de cadenas"` ; icône du bouton avec `alt="Copier"` : le bouton est lu « Copier Copier le code » (ACC-06) |
| 3.1 Information pas donnée uniquement par la couleur | 1.4.1 | ✅ | ❌ | Le niveau atteint n'est indiqué que par la **couleur** de la zone (ACC-03). Médiathèque : le statut est écrit en toutes lettres |
| 3.2 Contraste du texte | 1.4.3 | ✅ | ❌ | Bouton « Copier » 3,94:1 (ACC-01). Textes du canvas : blanc sur **jaune 1,07:1**, sur orange 1,97:1, sur gris 3,95:1, en police d'environ 10 px (ACC-04) |
| 7.1 Composants compatibles avec les technologies d'assistance | 4.1.2 | ✅ | ✅ | Boutons et cases à cocher natifs |
| 7.5 Messages de statut restitués | 4.1.3 | ✅ | ❌ | Médiathèque : message en `role="alert"`. Générateur : le mot de passe apparaît **sans être annoncé** (ni `aria-live`, ni `role="status"`) (ACC-05) |
| 8.3 Langue de la page | 3.1.1 | ✅ | ✅ | `lang="fr"` |
| 8.5 Titre de la page | 2.4.2 | ✅ | ✅ | « Médiathèque », « Générateur de mot de passe » |
| 9.1 Structure par titres | 1.3.1 | ✅ | ✅ | h1 puis h2 (Médiathèque) ; h1 + `fieldset` / `legend` (générateur) |
| 10.7 Focus visible | 2.4.7 | ✅ | ✅ | Contour du navigateur visible sur chaque élément ([capture](captures/generateur-focus-clavier.png)) |
| 10.11 Pas de défilement horizontal à 320 px de large | 1.4.10 | ❌ | ❌ | Médiathèque : le tableau déborde (COMP-04). Générateur : l'indicateur est coupé et **inatteignable** (COMP-01) |
| 11.1 Champ de formulaire avec étiquette | 1.3.1, 3.3.2 | ❌ | ✅ | Champ « Emprunteur » : seulement un `placeholder`, qui disparaît à la saisie (ACC-07). axe-core l'accepte, le RGAA non |
| 11.10 Contrôle de saisie pertinent | 3.3.1 | ❌ | ❌ | Médiathèque : message d'erreur clair, mais le champ ISBN n'est pas signalé en erreur (`aria-invalid`) (ACC-08). Générateur : longueur 0, -1 ou vide acceptée sans message |
| 12.6 Zones regroupées et identifiables | 1.3.1 | ✅ | ✅ | Sections avec titres / `fieldset` avec `legend` |
| 12.8 Ordre de tabulation cohérent | 2.4.3 | ✅ | ✅ | Ordre de lecture, aucun piège clavier |

**Navigation au clavier seul** : les deux applications sont **entièrement utilisables** au clavier (Tab, Espace pour cocher, Entrée pour valider). Parcours testés : générer un mot de passe de 16 caractères ; emprunter un livre ; ajouter un livre avec un ISBN invalide.

## 4. Bilan

| | Médiathèque | Générateur |
|---|:---:|:---:|
| Critères vérifiés | 13 | 15 |
| Conformes | 10 | 8 |
| Non conformes | 3 | 7 |
| Taux de conformité de l'échantillon | **77 %** | **53 %** |

Les anomalies sont détaillées dans [03-anomalies.md](03-anomalies.md).
