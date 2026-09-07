# On this page

Rend cliquable le titre "On this page" en tête de la table des matières, pour remonter en haut de la page depuis n'importe quelle ancre, plutôt que de devoir remonter manuellement.

## Essaie-le

Regarde la table des matières à droite de cette page (ou en bas sur mobile): clique sur son titre "On this page" après avoir défilé plus bas sur la page, ça remonte tout en haut.

Ce n'est pas du JS: c'est un override du template `partials/toc.html` de Zensical, le titre devient un vrai lien `<a href="#">` au lieu d'un simple texte.

## Installation sur ton propre projet

Détails complets: [`on-this-page/README.md`](https://github.com/Mathod95/zensical/tree/main/on-this-page).

En résumé:

1. Copier `on-this-page/example/overrides/partials/toc.html` vers `overrides/partials/toc.html` dans ton projet.
2. S'assurer que `custom_dir` est actif dans `zensical.toml`:

```toml
[project.theme]
custom_dir = "overrides"
```

Si `custom_dir` pointe déjà ailleurs dans ton projet, copie `partials/toc.html` dans ce dossier existant plutôt que de le renommer.
