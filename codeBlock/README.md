# codeBlock

Fusion de deux features indépendantes en un seul fichier JS/CSS, pour des blocs de code enrichis:

- **Collapsible**: transforme un bloc `.codeblock` en transcript commande/sortie repliable, avec boutons copier, icônes de type de fichier, et un mode "peek" (aperçu tronqué avec fondu).
- **Blur**: floute des lignes précises d'un bloc de code (`data-blur`), révélées au survol. Exige `.codeblock` comme toutes les autres options, pas de flou isolé sans le reste des features.
- **Zebra**: une ligne sur deux légèrement plus sombre (`.zebra`), opt-in, exige aussi `.codeblock`.

Certaines idées (prompts de commande, repli, surlignage de lignes) s'inspirent de [CodeblockCustomizer](https://github.com/mugiwara85/CodeblockCustomizer) et [Obsidian Code Styler](https://github.com/mayurankv/Obsidian-Code-Styler), deux plugins Obsidian de personnalisation de blocs de code, adaptées ici à l'écosystème Zensical/pymdownx.

## Pourquoi fusionnées, et pourquoi l'ordre interne compte

Sur un bloc `.codeblock` contenant au moins une ligne `$ commande` (mode transcript), la partie collapsible reconstruit entièrement le DOM du bloc et supprime la balise `<code>` d'origine. Si le flou s'appliquait après cette reconstruction, il ne retrouverait plus rien à flouter (silencieusement, sans erreur).

Dans `codeBlock.js`, le code de blur est donc placé **avant** celui de collapsible: chaque script s'abonne à `document$` dans l'ordre où il apparaît dans le fichier, et cet ordre d'abonnement est ce qui détermine si le flou s'applique avant que collapsible ne détruise le DOM. Inverser ces deux blocs à l'intérieur du fichier casserait le flou sur les blocs transcript (mais pas sur les blocs "contenu de fichier statique", qui ne sont jamais reconstruits).

## Où vont les fichiers

Dans un projet Zensical, à copier depuis `example/` en conservant l'arborescence:

- `example/docs/javascripts/codeBlock.js` → `docs/javascripts/codeBlock.js`
- `example/docs/stylesheets/codeBlock.css` → `docs/stylesheets/codeBlock.css`

## zensical.toml

`example/zensical.toml` contient les clés nécessaires à cette feature, à reporter dans le `zensical.toml` du projet cible:

```toml
[project]
extra_css = ["stylesheets/codeBlock.css"]
extra_javascript = ["javascripts/codeBlock.js"]

[project.markdown_extensions]
attr_list = {}
pymdownx.highlight.anchor_linenums = true
pymdownx.highlight.line_spans = "__span"
pymdownx.highlight.pygments_lang_class = true
pymdownx.inlinehilite = {}
pymdownx.superfences = {}
```

Si `extra_css`/`extra_javascript`/`markdown_extensions` existent déjà dans le projet cible, fusionner plutôt qu'écraser.

## Syntaxe dans les blocs de code

Attributs à poser sur la clôture du bloc, ex. ```` ``` { .yaml .codeblock data-blur="5 6 7" data-peek="5" title="fichier.yaml" } ````:

| Attribut | Effet |
| --- | --- |
| `.codeblock` | Active la feature sur ce bloc. Sans ligne `$ ...`, le bloc est laissé intact (juste l'icône de titre ajoutée). Avec au moins une ligne `$ ...`, transcript commande/sortie repliable. |
| `.expanded` | Sortie affichée dépliée par défaut (avec `.codeblock`). |
| `.no-copy` | Classe native Zensical (pas propre à codeBlock), qui sert normalement à masquer le bouton copier natif au cas par cas. Sans effet ici: `.codeblock` utilise toujours son propre bouton copier (par commande, ou pour le contenu d'un fichier) et masque systématiquement les contrôles natifs (`.md-code__nav`, copier et "Toggle line selection") à sa place, dans les deux modes. Inutile de l'ajouter, `.codeblock` s'en charge déjà tout seul. |
| `data-blur="2 4"` ou `"1-5 14-26"` | Floute les lignes indiquées, nettes au survol. Exige `.codeblock` sur le même bloc pour s'activer (sélecteur `.codeblock[data-blur]`); seul, sans `.codeblock`, n'a aucun effet. Ne fonctionne pas non plus combiné à `.codeblock` sur un bloc transcript (voir ci-dessus). |
| `data-peek="5"` | Affiche toujours un aperçu de N lignes avec fondu + "Afficher tout", au lieu du repli par défaut. |
| `title="chemin/fichier.ext"` | Bandeau de titre avec icône de fichier auto-détectée par extension (sur `.codeblock` uniquement). |
| `data-filetype="yaml"` | Force l'icône de type de fichier, indépendamment du texte du titre. |
| `linenums="N"` (natif pymdownx.highlight, pas propre à codeBlock) | Numéros de ligne. Compatible avec `.codeblock` (numérotation conservée dans le transcript reconstruit) et avec `data-peek` (colonne de numéros rognée en même temps que le code). |
| `hl_lines="N M-P"` (natif pymdownx.highlight, pas propre à codeBlock) | Surligne des lignes précises. Reste fonctionnel sur un bloc `.codeblock` en mode "contenu de fichier statique" (pas de ligne `$`); non testé/non garanti en mode transcript puisque ce mode reconstruit le bloc en segments commande/sortie. |
| `.zebra` | Une ligne sur deux reçoit un fond légèrement différent (`color-mix` à 5%, theme-aware). Exige `.codeblock` sur le même bloc pour s'activer (sélecteur `.codeblock.zebra`), comme `data-blur`. En mode transcript, le zébrage repart de zéro à chaque commande plutôt que de continuer sur tout le bloc. |

## Piège: `.codeblock` ne doit jamais être la seule classe

`{ .codeblock }` seul se fait consommer par `pymdownx.superfences` comme "le langage" du bloc (repli silencieux sur `language-text`), et la classe `codeblock` n'atterrit alors jamais sur la div générée: aucune feature ne s'active, retour aux boutons natifs Zensical, sans erreur ni avertissement au build. Toujours faire précéder `.codeblock` d'un langage, réel (`.yaml`, `.console`...) ou neutre si le contenu n'en a pas (`.text`):

```` ```{ .text .codeblock } ````
