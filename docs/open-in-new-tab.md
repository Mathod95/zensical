# open-in-new-tab

Ouvre automatiquement les liens externes (et les fichiers téléchargeables comme les PDF) dans un nouvel onglet, avec une petite icône après les liens externes pour prévenir le lecteur avant qu'il clique.

Réimplémentation JS/CSS pur de l'idée de [mkdocs-open-in-new-tab](https://github.com/JakubAndrysek/mkdocs-open-in-new-tab): ce plugin est packagé comme un plugin Python MkDocs, mais toute sa logique réelle est déjà du JS côté navigateur, pas besoin d'installer un paquet Python pour ça.

## Essaie-le

Ce lien est externe, il s'ouvre dans un nouvel onglet: [zensical.org](https://zensical.org/). Une petite icône apparaît après le texte du lien.

Ce lien est interne (une autre page de ce site), il s'ouvre normalement, dans le même onglet: [CodeBlock](codeblock.md).

## Comportement

- Un lien est externe si son hostname diffère de celui de la page courante.
- Un lien interne pointant vers un fichier téléchargeable (`.pdf`, `.zip`, `.tar`, `.gz`, `.docx`, `.xlsx`, `.pptx`) s'ouvre aussi dans un nouvel onglet, même sur le même domaine.
- `mailto:`, `tel:` et `javascript:` sont explicitement ignorés (jamais transformés).
- Les liens concernés reçoivent `target="_blank"` et `rel="noopener noreferrer"`.

## Installation sur ton propre projet

Détails complets: [`open-in-new-tab/README.md`](https://github.com/Mathod95/zensical/tree/main/open-in-new-tab).

En résumé:

1. Copier `open-in-new-tab/example/docs/javascripts/open-in-new-tab.js` et `open-in-new-tab/example/docs/stylesheets/open-in-new-tab.css` vers les mêmes chemins dans ton projet.
2. Ajouter dans `zensical.toml`:

```toml
[project]
extra_css = ["stylesheets/open-in-new-tab.css"]
extra_javascript = ["javascripts/open-in-new-tab.js"]
```

La liste d'extensions considérées comme téléchargement, et l'icône (activable/désactivable), se configurent en tête de `open-in-new-tab.js`, dans l'objet `CONFIG`.
