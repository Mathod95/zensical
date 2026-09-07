# On this page

Rend cliquable le titre "On this page" en tête de la table des matières, pour remonter en haut de la page depuis n'importe quelle ancre.

## Où va le fichier

Dans un projet Zensical, à copier depuis `example/` en conservant l'arborescence:

- `example/overrides/partials/toc.html` → `overrides/partials/toc.html`

## zensical.toml

`example/zensical.toml` contient uniquement la clé nécessaire à cette feature, à reporter dans la section `[project.theme]` du `zensical.toml` du projet cible:

```toml
[project.theme]
custom_dir = "overrides"
```

Si `custom_dir` pointe déjà vers un autre dossier dans le projet cible, y copier `partials/toc.html` plutôt que de renommer ce dossier en `overrides`.
