# zensical

Site de documentation basé sur [Zensical](https://zensical.org/), le générateur de site statique des créateurs de Material for MkDocs.

Ce dépôt sert aussi de terrain d'expérimentation pour développer des scripts JS et des personnalisations autour du thème (bascule de la sidebar, overrides de templates, etc.).

Le site publié est disponible sur [mathod95.github.io/zensical](https://mathod95.github.io/zensical/).

## Structure

- `docs/` contient le contenu Markdown du site, ainsi que les scripts et styles additionnels (`docs/javascripts/`, `docs/stylesheets/`).
- `overrides/` contient les templates HTML surchargeant ceux fournis par le thème Zensical.
- `zensical.toml` est le fichier de configuration du site (navigation, thème, extensions Markdown).
- `.github/workflows/docs.yml` construit et publie le site sur GitHub Pages à chaque changement sur `docs/`, `zensical.toml` ou le workflow lui-même.
- `toggle-sidebar/`, `on-this-page/`, `codeBlock/`, `open-in-new-tab/` et `placeholders/` contiennent chacun un `example/` avec une copie de référence des fichiers de la personnalisation correspondante, indépendamment de leur emplacement fonctionnel réel.

## Fonctionnalités

### Toggle sidebar

Bouton dans le header, plus raccourcis clavier (`b` bascule nav + ToC, `m` bascule la nav seule, `t` bascule la ToC seule), pour masquer/afficher indépendamment la navigation et la table des matières. L'état choisi est persisté en `localStorage`.

Fichiers fonctionnels: `docs/javascripts/toggle-sidebar.js`, `docs/stylesheets/toggle-sidebar.css` (déclarés via `extra_javascript`/`extra_css` dans `zensical.toml`). Exemple: [`toggle-sidebar/example/`](toggle-sidebar/example/).

### On this page

Le titre "On this page" en tête de la table des matières devient cliquable et remonte en haut de la page, quelle que soit l'ancre active.

Fichier fonctionnel: `overrides/partials/toc.html` (surcharge du template via `custom_dir` dans `zensical.toml`). Exemple: [`on-this-page/example/`](on-this-page/example/).

### codeBlock

Fusion de deux features de blocs de code: transcript commande/sortie repliable avec boutons copier, icônes de fichier et mode "peek" (`.codeblock`), et flou de lignes précises révélées au survol (`data-blur`). Les deux scripts d'origine sont fusionnés dans un seul fichier, dans un ordre précis: le code de flou doit s'abonner à `document$` avant celui du collapsible, sinon le flou ne s'applique plus sur un bloc transcript (le collapsible reconstruit le DOM et supprime la balise `<code>` d'origine dans ce cas).

Fichiers fonctionnels: `docs/javascripts/codeBlock.js`, `docs/stylesheets/codeBlock.css` (déclarés via `extra_javascript`/`extra_css` dans `zensical.toml`). Exemple: [`codeBlock/example/`](codeBlock/example/), détails de la syntaxe dans [`codeBlock/README.md`](codeBlock/README.md).

### open-in-new-tab

Ouvre automatiquement les liens externes et les fichiers téléchargeables (PDF, zip...) dans un nouvel onglet, avec une petite icône après les liens externes. Réimplémentation JS/CSS pur de l'idée de [mkdocs-open-in-new-tab](https://github.com/JakubAndrysek/mkdocs-open-in-new-tab), dont la logique réelle est déjà du JS côté navigateur, sans avoir besoin d'installer le plugin Python.

Fichiers fonctionnels: `docs/javascripts/open-in-new-tab.js`, `docs/stylesheets/open-in-new-tab.css` (déclarés via `extra_javascript`/`extra_css` dans `zensical.toml`). Exemple: [`open-in-new-tab/example/`](open-in-new-tab/example/), détails dans [`open-in-new-tab/README.md`](open-in-new-tab/README.md).

### placeholders

Transforme `{{ variable }}` écrit dans le texte d'une page en zone éditable directement dans le navigateur: chaque lecteur peut taper sa propre valeur, répercutée en direct sur toutes les occurrences du même nom sur le site, et mémorisée en `localStorage`. Différent des variables Jinja2/macros natives de zensical (`zensical.extensions.macros`), qui sont figées à la génération du site et identiques pour tous les visiteurs, ici c'est une valeur choisie par chaque lecteur, jamais envoyée à un serveur.

Fichiers fonctionnels: `docs/javascripts/placeholders.js`, `docs/stylesheets/placeholders.css` (déclarés via `extra_javascript`/`extra_css` dans `zensical.toml`). Exemple: [`placeholders/example/`](placeholders/example/), détails dans [`placeholders/README.md`](placeholders/README.md).

## Développement local

Zensical s'installe dans un environnement virtuel Python:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install zensical
```

Lancer le serveur local avec rechargement automatique:

```bash
zensical serve
```

Le site est servi sur `http://localhost:8000`.

Pour un build de contrôle sans serveur:

```bash
zensical build
```

La sortie est générée dans `site/`.

## Prérequis GitHub Pages

Dans les settings du dépôt GitHub, `Settings > Pages > Build and deployment > Source` doit être réglé sur **GitHub Actions** pour que le workflow de publication fonctionne.
