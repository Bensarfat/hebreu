# Consignes pour Claude — site de révision d'hébreu de Benjamin (`Bensarfat/hebreu`)

Ce dépôt est modifié par Benjamin et par Maayane (sa professeure d'hébreu), chacun avec son propre compte Claude.
Réponds toujours en français, en mots simples, sans jargon.

## Comment ce site est fabriqué

- **`index.html` est produit automatiquement : ne le modifie jamais à la main.** L'action « Assembler le site » le reconstruit
  toutes les heures, à chaque modification des données, et à la demande (onglet **Actions** › « Assembler le site » › **Run workflow**).
- Il est assemblé à partir de :
  - **le code des exercices** (quiz, flashcards, onglet « Réviser » et jeux, grammaire, statistiques, audio, XP, badges),
    qui vit dans le dépôt de Maayane : `Bensarfat/hebreu-avec-mayane`, fichiers `source/exercices/app.html`, `source/src/progress.js`, `source/src/game.js` ;
  - **les données personnelles de Benjamin**, ici :
    - `donnees.js` : vocabulaire (`VOCAB`), natures (`NATURE`), conjugaison (`CONJ`), thèmes, présent des verbes, grammaire (`GRAMMAR_CATEGORIES`) ;
    - `passe-futur.html` : le contenu de l'onglet « Passé & futur ».
- Le site est publié sur https://hebreu-eight.vercel.app. Il est privé : pas de référencement (balise noindex, robots.txt, en-tête X-Robots-Tag).

## Que modifier, et où

- **Ajouter ou corriger du vocabulaire, de la grammaire, la conjugaison, le cours Passé & futur de Benjamin** : ici, dans `donnees.js` ou `passe-futur.html`.
  `donnees.js` doit commencer par `const VOCAB = {` et ne contenir que des données (pas d'appel de fonction).
- **Changer un exercice, ajouter une révision ou un jeu, modifier l'apparence du site de révision** : dans le dépôt `Bensarfat/hebreu-avec-mayane`
  (ajoute-le à la session), en suivant son `CLAUDE.md`. Le changement arrive ici automatiquement après sa fusion, et chez les élèves de Maayane.

## Règles de travail

1. Ne pousse jamais sur `main` : branche (`benjamin/<sujet>` ou `maayane/<sujet>`), puis pull request.
2. Ne touche pas aux données de Benjamin sans demande explicite.
3. Avant la pull request, vérifie l'assemblage :
   ```
   git clone --depth 1 https://github.com/Bensarfat/hebreu-avec-mayane.git /tmp/m
   node /tmp/m/source/exercices/assemble.js --donnees donnees.js --passe-futur passe-futur.html --sortie /tmp/index.html
   ```
   puis ouvre `/tmp/index.html` dans un navigateur (ordinateur et 360 px) : aucune erreur dans la console. Ne commite pas ce fichier :
   l'action le publiera elle-même après la fusion.
4. Ne demande, n'affiche et n'écris jamais de mot de passe, de clé ou de jeton.
