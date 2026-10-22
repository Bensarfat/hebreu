# Consignes pour Claude — site de révision d'hébreu de Benjamin (`Bensarfat/hebreu`)

Ce dépôt est modifié par Benjamin et par Maayane (sa professeure d'hébreu, pas développeuse), chacun avec son propre compte Claude.
Réponds toujours en français, en mots simples, sans jargon. Explique ce que tu changes du point de vue de la personne qui révise.

## À quoi sert ce dépôt

- `index.html` est **tout le site** de révision : quiz, flashcards, onglet « Réviser » (révision espacée et jeux), grammaire,
  vocabulaire, « Passé & futur », statistiques, XP et badges. Il est publié sur https://hebreu-eight.vercel.app (site perso de Benjamin).
- **Le même code sert aussi aux élèves de Maayane.** Toutes les heures, le dépôt `Bensarfat/hebreu-avec-mayane` recopie
  `index.html` et en tire l'espace élève de son site (en remplaçant les données de Benjamin par celles de chaque élève).
  Une nouvelle révision ajoutée ici apparaît donc sur les deux sites : immédiatement sur celui-ci, dans l'heure sur celui de Maayane
  (ou tout de suite via l'onglet **Actions** du dépôt de Maayane › « Synchroniser avec le site de Benjamin » › **Run workflow**).

## Règles de travail (obligatoires)

1. **Ne pousse jamais sur `main`.** Travaille sur une branche nouvelle (`maayane/<sujet-court>` ou `benjamin/<sujet-court>`),
   puis ouvre une pull request vers `main`. La fusion publie le site perso de Benjamin en une minute environ.
2. **Une demande = une branche = une pull request.** Changements petits et ciblés.
3. **Ne touche pas aux données personnelles de Benjamin** (le vocabulaire et la grammaire déjà présents dans `VOCAB`, `GRAMMAR_CATEGORIES`, `CONJ`, etc.)
   sauf demande explicite. Une nouvelle révision doit fonctionner avec les données existantes.
4. **Vérifie que le site de Maayane se construit toujours** (étape indispensable : sinon sa synchronisation s'arrête) :
   ```
   git clone --depth 1 https://github.com/Bensarfat/hebreu-avec-mayane.git /tmp/m
   cp index.html /tmp/m/source/src/original.html
   cd /tmp/m/source && node src/build_templates.js && node build.js
   ```
   Le message doit commencer par « ✓ gabarit élève ». S'il affiche « ✗ Construction impossible », corrige avant d'ouvrir la pull request.
5. Ouvre `index.html` dans un navigateur (ou avec Playwright) sur ordinateur **et** en largeur téléphone (360 px) : aucune erreur dans la console,
   pas de défilement horizontal.
6. Dans la description de la pull request, écris en français : ce qui change pour la personne qui révise, comment le vérifier sur l'aperçu,
   et ce qui n'a pas été fait.
7. Ne demande, n'affiche et n'écris jamais de mot de passe, de clé ou de jeton.

## Repères à ne pas casser (le site de Maayane en dépend)

Chacun doit rester présent **une seule fois**, à l'identique :
- le bloc de données qui commence par `const VOCAB = {` et se termine juste avant `/* ============================================\n   STATE & STORAGE` ;
  il ne doit contenir que des données (pas d'appel de fonction ni de variable externe) ;
- `<div class="panel" id="panel-tenses">` puis, plus loin, `<div class="panel" id="panel-stats">` ; ne place **aucun** nouveau panneau entre les deux
  (ce qui s'y trouve est remplacé par le cours de chaque élève) ;
- `const STORAGE_KEY = 'hebrew_quiz_v2';` et la ligne `const raw = localStorage.getItem(STORAGE_KEY);` dans `loadData()` ;
- `<div class="container">` et la balise `<meta name="viewport" …>` ;
- le bloc `<!-- HP:BEGIN … -->` … `<!-- HP:END -->` (progression, XP, badges : `var HP = (function` puis `var HPG = (function`).

## Ajouter une nouvelle révision ou un nouveau jeu

- L'onglet « Réviser » (`#panel-revise`, module `RÉVISER` placé après `<!-- HP:END -->`) est le bon endroit pour un jeu court :
  ajoute une entrée dans l'objet `GAMES` (titre, `start`, `click`, `end`) et une carte dans `render()`.
- Enregistre chaque réponse avec `updateWordStat({ he, fr }, 'correct' | 'wrong')` : statistiques, XP et révision espacée suivent automatiquement.
- Si la révision a besoin d'une **nouvelle donnée** (par exemple une définition ou une phrase d'exemple par mot) :
  ajoute-la dans le bloc de données avec une valeur par défaut, utilise-la avec une garde (`typeof DEFS !== 'undefined'`),
  et cache le jeu quand la donnée manque. Pour que les élèves de Maayane l'aient aussi, il faut ensuite, dans `Bensarfat/hebreu-avec-mayane` :
  la produire lors de l'analyse des documents (`source/src/analyse.js`) et la transmettre dans le chargeur de `source/src/build_templates.js`
  (liste `const VOCAB = __D.VOCAB || {}` …). Fais-le dans une pull request séparée sur ce dépôt-là.
- Tutoiement pour la personne qui révise. Mots hébreux dans un élément `lang="he" dir="rtl"`. Garde l'identité visuelle (variables CSS existantes)
  et les contrastes lisibles, en clair comme en sombre.
