# Toggle sidebar

Bouton dans le header du thème, plus raccourcis clavier, pour masquer/afficher indépendamment la navigation et la table des matières. L'état choisi est persisté en `localStorage`, donc conservé d'une page à l'autre et au rechargement.

## Essaie-le

Regarde le header de cette page: un bouton en forme d'icône (colonnes) a été ajouté à côté des contrôles habituels (recherche, thème). Clique dessus, ou utilise les raccourcis clavier ci-dessous (en dehors d'un champ de texte):

| Raccourci | Effet |
| --- | --- |
| `b` | Bascule navigation + table des matières ensemble |
| `m` | Bascule la navigation seule |
| `t` | Bascule la table des matières seule |

L'icône du bouton change selon l'état courant (deux colonnes, une colonne, ou aucune) pour refléter ce qui est actuellement visible.

## Installation sur ton propre projet

Détails complets et fichiers à copier: [`toggle-sidebar/README.md`](https://github.com/Mathod95/zensical/tree/main/toggle-sidebar).

En résumé:

1. Copier `toggle-sidebar/example/docs/javascripts/toggle-sidebar.js` et `toggle-sidebar/example/docs/stylesheets/toggle-sidebar.css` vers les mêmes chemins dans ton projet.
2. Ajouter dans `zensical.toml`:

```toml
[project]
extra_css = ["stylesheets/toggle-sidebar.css"]
extra_javascript = ["javascripts/toggle-sidebar.js"]
```

Le comportement par défaut (nav/ToC visibles ou non au premier chargement, quel bouton afficher) se configure directement en tête de `toggle-sidebar.js`, dans l'objet `CONFIG`.
