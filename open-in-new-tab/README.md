# open-in-new-tab

Ouvre automatiquement les liens externes (et les fichiers téléchargeables comme les PDF) dans un nouvel onglet, avec une petite icône après les liens externes.

Réimplémentation en JS/CSS pur de l'idée de [mkdocs-open-in-new-tab](https://github.com/JakubAndrysek/mkdocs-open-in-new-tab): ce plugin est packagé comme un plugin Python MkDocs, mais toute sa logique réelle est déjà du JS côté navigateur (le plugin Python ne fait qu'injecter le script). Pas besoin d'installer un paquet Python supplémentaire pour ça, un fichier `extra_javascript` suffit, cohérent avec le reste des personnalisations de ce projet.

## Comportement

- Un lien est considéré externe si son hostname diffère de celui de la page courante.
- Un lien interne pointant vers un fichier téléchargeable (`.pdf`, `.zip`, `.tar`, `.gz`, `.docx`, `.xlsx`, `.pptx`) s'ouvre aussi dans un nouvel onglet, même si le domaine est le même.
- `mailto:`, `tel:` et `javascript:` sont explicitement ignorés.
- Les liens concernés reçoivent `target="_blank"` et `rel="noopener noreferrer"`.
- Une petite icône (masque CSS, pas de police d'icônes chargée) est ajoutée après chaque lien externe, désactivable via `CONFIG.addIcon = false` dans le fichier JS.
- Compatible avec la navigation instantanée de Zensical (`document$.subscribe`), les nouveaux liens sont bien traités après chaque changement de page.

## Où vont les fichiers

Dans un projet Zensical, à copier depuis `example/` en conservant l'arborescence:

- `example/docs/javascripts/open-in-new-tab.js` → `docs/javascripts/open-in-new-tab.js`
- `example/docs/stylesheets/open-in-new-tab.css` → `docs/stylesheets/open-in-new-tab.css`

## zensical.toml

`example/zensical.toml` contient les clés nécessaires, à reporter dans le `zensical.toml` du projet cible:

```toml
[project]
extra_css = ["stylesheets/open-in-new-tab.css"]
extra_javascript = ["javascripts/open-in-new-tab.js"]
```

Si `extra_css`/`extra_javascript` existent déjà dans le projet cible, fusionner plutôt qu'écraser.
