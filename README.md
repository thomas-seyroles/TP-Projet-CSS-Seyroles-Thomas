# Ricochet, festival des berges

Site vitrine d'un festival de musique fictif (25 et 26 juin 2027, Saint-Galet). Projet noté, module CSS avancé, réalisé seul (rendu individuel).

## Groupe

| | |
|---|---|
| Nom du groupe | seyroles-thomas |
| Membre | Thomas Seyroles (seul : architecture, mise en page, thème, animations et accessibilité) |
| Événement / thème | Festival de musique : « Ricochet, festival des berges », 2 jours, 2 scènes |
| Lien du dépôt Git | à compléter |
| Convention de nommage | BEM : `.bloc`, `.bloc__element`, `.bloc--variante` (ex. `.offer`, `.offer__price`, `.artist--headliner`) ; mélange de blocs sur un même élément quand deux rôles se croisent (`class="perks offer__perks"`, `class="section surface surface--strong"`) ; utilitaires préfixés `u-` ; scène d'un créneau portée par l'attribut `data-stage` |

Direction visuelle : affiche sérigraphiée. Aplats de couleurs franches (jaune, tomate, cobalt, menthe) sur papier crème, bordures épaisses, titres en Anton capitales. Aucun dégradé ni ombre portée. Le décor du hero (cercles concentriques) illustre le nom : les ronds dans l'eau d'un ricochet.

Tokens : les couleurs, tailles de texte, bordures, rayons et durées ne sont utilisés qu'au niveau sémantique. L'échelle d'espacement `--space-1` à `--space-16` est utilisée directement dans les composants, comme dans le `.btn` du starter : c'est une échelle de rythme, pas un rôle ; les espacements qui portent un rôle ont leur token (`--space-section`, `--space-card`, `--space-gutter`, `--space-stack`).

## Localisation des exigences

| Exigence | Élément attendu | Fichier:ligne | Commentaire |
|---|---|---|---|
| 1. Architecture | Ordre des `@layer` | `css/main.css:2` | Déclaré une fois, avant les `@import` |
| 1. Architecture | Tokens primitifs | `css/tokens.css:4` | Palette oklch, espacements, tailles, rayons, durées, courbes, polices |
| 1. Architecture | Tokens sémantiques | `css/tokens.css:49` | Surfaces, textes, action, bandeaux, scènes, focus, typo fluide |
| 1. Architecture | Nesting natif | `css/components.css:252` | `.slot` avec `&:target` puis `& .slot__stage` (3 niveaux max) |
| 1. Architecture | Tokens de composant + `color-mix()` + repli `var()` | `css/components.css:100` | `--btn-bg`, `--btn-fg`… lus sur `.btn` ; état `:active` = survol mélangé à 30 % de `--color-tint` en oklab (`:106`), contraste du texte de 7,47 ; `border-radius: var(--btn-radius, var(--radius-control))` (`:114`). Même principe pour les bandeaux : `.surface` lit `--surface-bg` / `--surface-fg` (`css/layout.css:13`) |
| 2. Mise en page | `grid-template-areas` | `css/layout.css:78` | Programme : 5 zones empilées, puis 2 colonnes (48rem), puis 3 colonnes (64rem). Aussi le hero : `css/layout.css:52` |
| 2. Mise en page | `subgrid` | `css/layout.css:130` et `css/components.css:295` | Chaque `li` couvre 4 lignes et les transmet à la carte : images, noms, bios et liens alignés d'une carte à l'autre. Second subgrid en colonnes pour les horaires : `css/components.css:246` et `:252` |
| 2. Mise en page | Flexbox | `css/components.css:38` | Nav (`flex-wrap`, `gap`) ; aussi l'en-tête de carte `:321` et l'en-tête du site `css/layout.css:29` |
| 3. Responsive | Media query mobile-first | `css/layout.css:43` | `@media (width >= 48rem)`, uniquement pour élargir |
| 3. Responsive | `clamp()` | `css/tokens.css:97` | Échelle h1/h2/h3/corps/lead + `--space-section` (`:106`) dans les tokens |
| 3. Responsive | Container query | `css/layout.css:119` + `css/components.css:363` | Conteneur `artist` sur `.feature`. Même carte `.artist` dans deux contextes : tête d'affiche (pleine largeur, horizontale) et coup de cœur du programme (colonne étroite à 64rem, verticale ; horizontale entre 48 et 64rem). Titre en `cqi` : token `--font-size-feature` (`css/tokens.css:102`) utilisé `css/components.css:358`. Pas de conteneur sur `.lineup__item` : la containment y annulerait le subgrid |
| 3. Responsive | 320 px sans défilement horizontal | `css/layout.css:123` | `minmax(min(100%, 20rem), 1fr)`, `overflow-wrap` (`css/base.css:25`), `inline-size: min(…)` (`css/layout.css:4`). Vérifié : `scrollWidth` = 320 |
| 4. Sélecteurs | `:has()` | `css/components.css:391` | L'offre qui contient `.offer__badge` passe en avant (`index.html:255`). Aussi la bascule de thème `css/tokens.css:126` |
| 4. Sélecteurs | `:is()` / `:where()` | `css/base.css:19`, `css/components.css:210`, `:307` | `:where()` à spécificité nulle pour les titres et les scènes ; `:is(:hover, :focus-within)` factorise survol et clavier |
| 5. Thème | `light-dark()` + `color-scheme` | `css/tokens.css:51` | Tous les rôles de couleur basculent via `light-dark()` |
| 5. Thème (bonus) | Bascule CSS-only | `css/tokens.css:126` | La case inverse le thème du système (sombre si le système est clair, clair sinon) |
| 6. Animations | Transition n°1 | `css/components.css:123` | Boutons : couleurs + `translate` |
| 6. Animations | Transition n°2 | `css/components.css:318` | Pochette d'artiste : `rotate` au survol ou au focus |
| 6. Animations | `@keyframes` | `css/components.css:588` | `slam` (titre du hero) et `ripple-in` (cercles du hero), une seule fois au chargement |
| 6. Animations | `prefers-reduced-motion` | `css/components.css:609`, `css/utilities.css:13` | Les animations et les mouvements au survol (`css/components.css:134`, `:306`) ne sont déclarés qu'en `no-preference` : rien ne reste à `opacity: 0`. Les transitions sont coupées dans `utilities` |
| 6. Animations (bonus) | Scroll-driven | `css/components.css:623` | `animation-timeline: view()` dans `@supports` ; Firefox affiche la page sans |
| 6. Animations (bonus) | Popover + `@starting-style` | `css/components.css:470` | Popover « Tarifs réduits » (`index.html:281`) qui apparaît et disparaît en fondu ; `@starting-style` en `css/components.css:491` |
| 7. Accessibilité | Landmarks + titres | `index.html:17` | `header`, `nav`, `main`, sections titrées par `aria-labelledby`, `footer` (`index.html:330`), un seul `h1` (`index.html:42`), lien d'évitement (`index.html:14`) |
| 7. Accessibilité | `:focus-visible` | `css/base.css:55` | Anneau noir + halo jaune, visible sur toutes les surfaces |
| 7. Accessibilité | Contrastes AA | `css/tokens.css:49` | Tous les textes contrôlés dans les deux thèmes avec un script de calcul de ratio WCAG (Chromium) : aucun en dessous de 4,5:1 (3:1 pour le grand texte) |
| 7. Accessibilité | `alt` | `index.html:139` | Description des visuels ; le décor du hero est en `aria-hidden` |
| 8. Qualité | Validateur W3C | - | Passé le 13/11/2026 (profil CSS niveau 3 + SVG, par défaut) : `reset.css`, `base.css` et `utilities.css` valides ; aucune vraie erreur ailleurs, seulement les syntaxes récentes listées ci-dessous |
| 8. Qualité | `!important` | - | Aucun |

## Avertissements du validateur W3C

Résultat du validateur (https://jigsaw.w3.org/css-validator/, saisie directe fichier par fichier, profil CSS niveau 3 + SVG). Toutes les erreurs restantes portent sur des syntaxes récentes que le validateur ne connaît pas encore :

| Fichier:ligne | Message du validateur | Pourquoi c'est valide |
|---|---|---|
| `css/main.css:4` à `:9` | `@import` non autorisé après une autre déclaration | La déclaration `@layer reset, base, …;` est la seule règle autorisée avant les `@import` (Cascade Layers, Baseline 2022) |
| `css/tokens.css:53` à `:78` | Erreur interne du validateur (`Cannot invoke … getType()`) sur les lignes en `light-dark()` | `light-dark()`, CSS Color 5, Baseline 2024 : le validateur plante sur la fonction |
| `css/tokens.css:102` | Erreur lors de l'analyse grammaticale | Unité `cqi` dans `clamp()`, Container Queries, Baseline 2023 |
| `css/layout.css:4` | `min(100% - 2 * var(…), var(…))` n'est pas une valeur de `inline-size` | Fonction mathématique avec `var()`, valide (CSS Values 4) ; le validateur ne résout pas `var()` dans `min()` |
| `css/layout.css:120` | La propriété `container` n'existe pas | Raccourci de `container-name` / `container-type`, Container Queries, Baseline 2023 |
| `css/components.css:363` | `@container` n'est pas reconnue | Container Queries, Baseline 2023 |
| `css/components.css:491` | `@starting-style` n'est pas reconnue | CSS Transitions 2, Baseline 2024 |
| `css/components.css:623`, `:627`, `:628` | `animation-timeline` et `animation-range` n'existent pas | Scroll-driven Animations, en amélioration progressive dans `@supports` |

Le nesting natif, `oklch()`, `color-mix()`, `:has()`, `text-wrap`, les media queries `(width >= …)` et `:popover-open` passent sans erreur.

## Bonus technique revendiqué

| Fonctionnalité | Fichier:ligne | Explication et repli |
|---|---|---|
| Bascule de thème CSS-only | `css/tokens.css:126` | `:root:has(#theme-toggle:checked)` change `color-scheme`, donc tous les `light-dark()` d'un coup. Sans `:has()`, le thème système s'applique |
| Animation au défilement | `css/components.css:623` | Créneaux, cartes et offres montent en entrant dans la vue. Hors `@supports` (Firefox), elles sont simplement affichées |
| Popover animé | `css/components.css:470` | Popover déclaratif (`popovertarget`, sans JS). `@starting-style` (`:491`) donne l'état de départ à l'ouverture, `transition` sur `display` et `overlay` en `allow-discrete` pour animer la fermeture. Sans prise en charge, il s'ouvre simplement sans animation |

## JavaScript

Aucun.

## Déclaration d'usage d'outils d'IA

| Outil | Pour quoi faire | Parties concernées |
|---|---|---|
| Claude (Anthropic) | Proposition de la structure HTML/CSS, des tokens, des textes fictifs et des illustrations SVG ; vérification des contrastes et du rendu à 320 px | Ensemble du projet, relu et compris |

## Qui a fait quoi

| Membre | Principales contributions |
|---|---|
| Thomas Seyroles | Tout le projet : HTML, CSS (les 7 fichiers), illustrations, README |

## Crédits

- Polices : Anton (Vernon Adams) et Archivo (Omnibus-Type), Google Fonts, licence SIL Open Font License.
- Illustrations : visuels SVG des artistes (`img/`) et décor du hero réalisés par le groupe.
