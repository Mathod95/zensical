# placeholders

Transforme un placeholder écrit dans une page en zone éditable directement dans le navigateur: chaque lecteur peut taper sa propre valeur, qui se répercute en direct sur toutes les occurrences du même nom sur la page, sur les autres pages du site, et reste mémorisée en `localStorage`.

Différent des variables Jinja2/macros natives de zensical (`zensical.extensions.macros`): celles-ci sont figées à la génération du site, identiques pour tous les visiteurs. `placeholders` est l'inverse, une valeur choisie par chaque lecteur dans son propre navigateur, jamais envoyée à un serveur.

## Deux syntaxes, deux contextes

- **`{{ variable }}`** dans le texte normal (paragraphes, titres, admonitions...).
- **`<variable>`** à l'intérieur d'un bloc de code, la convention courante pour un placeholder dans un exemple de config/commande (`<TOKEN>`, `<namespace>`...). Sans danger là où `{{ }}` ne le serait pas: le code est du texte échappé par Pygments, jamais interprété comme une vraie balise HTML, contrairement à `<variable>` en texte normal qui serait avalé comme un élément HTML inconnu et invisible.
- Les deux syntaxes partagent le même espace de noms: `{{ name }}` en texte normal et `<name>` dans un bloc de code se synchronisent entre eux s'ils portent le même nom.
- Le texte `{{ variable }}` ou `<variable>` affiché dans du **code inline** (\`entre simples backticks\`) n'est jamais transformé, pour documenter la syntaxe sans qu'elle s'active elle-même. Seul un vrai bloc de code (` ``` `) ou le texte normal en dehors de tout code l'active.

## Comportement

- Chaque placeholder affiche d'abord son propre nom par défaut, la première fois.
- Toutes les occurrences d'un même nom se synchronisent entre elles en direct (taper dans l'une met à jour toutes les autres, même entre les deux syntaxes).
- `Entrée` valide et quitte la zone au lieu d'insérer un retour à la ligne.
- La valeur est mémorisée en `localStorage` (partagée entre toutes les pages du site), donc conservée au rechargement de la page. Un rechargement forcé (Ctrl+Shift+R) ne la vide pas non plus: ça ne vide que le cache HTTP, jamais le `localStorage`, comportement standard de tous les navigateurs.
- Le bouton copier d'un bloc [`codeBlock`](../codeBlock/README.md) lit le contenu en direct au moment du clic, donc une valeur éditée via `placeholders` est bien incluse à la copie (voir `codeBlock.js`, `plainCopyBtn`/`outputCopyBtn`).

## Réinitialiser

Aucune interface fournie par défaut: n'importe quel élément marqué de la classe `placeholder-reset` déclenche la réinitialisation (vide tout le `localStorage` des placeholders et remet chaque zone visible à son nom par défaut), à placer où on veut en Markdown, par exemple dans une admonition:

```markdown
<button class="placeholder-reset" type="button">Réinitialiser</button>
```

## Où vont les fichiers

Dans un projet Zensical, à copier depuis `example/` en conservant l'arborescence:

- `example/docs/javascripts/placeholders.js` → `docs/javascripts/placeholders.js`
- `example/docs/stylesheets/placeholders.css` → `docs/stylesheets/placeholders.css`

## zensical.toml

`example/zensical.toml` contient les clés nécessaires, à reporter dans le `zensical.toml` du projet cible:

```toml
[project]
extra_css = ["stylesheets/placeholders.css"]
extra_javascript = ["javascripts/placeholders.js"]
```

Si `extra_css`/`extra_javascript` existent déjà dans le projet cible, fusionner plutôt qu'écraser.
