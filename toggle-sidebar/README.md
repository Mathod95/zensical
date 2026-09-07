# Toggle sidebar

Bouton dans le header du thème Zensical, plus raccourcis clavier (`b` bascule nav + ToC, `m` bascule la nav seule, `t` bascule la ToC seule), pour masquer/afficher indépendamment la navigation et la table des matières. L'état choisi est persisté en `localStorage`.

## Où vont les fichiers

Dans un projet Zensical, à copier depuis `example/` en conservant l'arborescence:

- `example/docs/javascripts/toggle-sidebar.js` → `docs/javascripts/toggle-sidebar.js`
- `example/docs/stylesheets/toggle-sidebar.css` → `docs/stylesheets/toggle-sidebar.css`

## zensical.toml

`example/zensical.toml` contient uniquement les clés nécessaires à cette feature, à reporter dans la section `[project]` du `zensical.toml` du projet cible:

```toml
[project]
extra_css = ["stylesheets/toggle-sidebar.css"]
extra_javascript = ["javascripts/toggle-sidebar.js"]
```

Si `extra_css`/`extra_javascript` existent déjà dans le projet cible, fusionner les listes plutôt que d'écraser les entrées existantes.
