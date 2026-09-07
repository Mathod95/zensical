# CodeBlock

Des blocs de code enrichis pour Zensical: transcripts commande/sortie repliables, flou de lignes sensibles, aperçus tronqués, icônes de fichier réelles, le tout derrière un seul attribut `.codeblock` à poser sur le bloc.

Fusion de deux scripts indépendants ([`codeBlock.js`/`codeBlock.css`](https://github.com/Mathod95/zensical/tree/main/codeBlock)) en une seule feature cohérente, pensée pour documenter des commandes shell et des fichiers de config sans surcharger chaque page de blocs de code bruts et statiques.

## Features

- **Transcript repliable**: un bloc contenant des lignes `$ commande` se découpe automatiquement en commandes/sorties, chaque sortie repliée par défaut derrière un bouton "Afficher la sortie".
- **Copie ciblée**: un bouton copier par commande, par sortie, ou pour le contenu entier d'un fichier statique, jamais un copier générique qui récupère tout en vrac. Les contrôles natifs de Zensical (copier, sélection de lignes) sont masqués automatiquement, aucun conflit visuel.
- **Icônes de fichier réelles**: détection automatique par extension (ou forcée via `data-filetype`) à partir des vraies icônes [Simple Icons](https://simpleicons.org), couleurs de marque officielles. Chaque ligne `$ ...` reçoit aussi son icône terminal, sans rien à configurer.
- **Aperçu tronqué (peek)**: alternative au repli, un extrait toujours visible (N lignes) avec un bouton "Afficher tout", pratique pour un fichier long qu'on veut pouvoir parcourir sans clic.
- **Flou de lignes sensibles**: masque des lignes précises (secrets, tokens, IDs) derrière un flou, révélées au survol.
- **Zébrage de lignes**: une ligne sur deux légèrement plus sombre (`.zebra`), pour ne pas se perdre sur un bloc large ou long.
- **Compatible nativement** avec `linenums=` et `hl_lines=` de pymdownx.highlight, sans rien casser.
- **Un seul interrupteur**: `.codeblock` active tout le nécessaire, les autres attributs restent optionnels et combinables librement.

## Attributs

Attributs disponibles sur un bloc de code pour activer les features de [`codeBlock.js`/`codeBlock.css`](https://github.com/Mathod95/zensical/tree/main/codeBlock), à poser sur la clôture du bloc, ex.:

```` ```{ .yaml .codeblock data-blur="5 6 7" data-peek="5" title="fichier.yaml" } ````

| Attribut | Effet |
| --- | --- |
| `.codeblock` | Active la feature sur ce bloc. Sans ligne `$ ...`, le bloc est laissé intact (juste l'icône de titre ajoutée). Avec au moins une ligne `$ ...`, transcript commande/sortie repliable. |
| `.expanded` | Sortie affichée dépliée par défaut (avec `.codeblock`). |
| `.no-copy` | Classe native Zensical (pas propre à codeBlock), qui sert normalement à masquer le bouton copier natif au cas par cas. Sans effet ici: `.codeblock` utilise toujours son propre bouton copier (par commande, ou pour le contenu d'un fichier) et masque systématiquement les contrôles natifs (`.md-code__nav`, copier et "Toggle line selection") à sa place, dans les deux modes. Inutile de l'ajouter, `.codeblock` s'en charge déjà tout seul. |
| `data-blur="2 4"` ou `"1-5 14-26"` | Floute les lignes indiquées, nettes au survol. Exige `.codeblock` sur le même bloc pour s'activer, seul il n'a aucun effet. Ne fonctionne pas non plus combiné à `.codeblock` sur un bloc transcript (le `<code>` d'origine est détruit avant que le flou ne s'applique). |
| `data-peek="5"` | Affiche toujours un aperçu de N lignes avec fondu + "Afficher tout", au lieu du repli par défaut. Mutuellement exclusif avec le toggle replié/déplié. |
| `title="chemin/fichier.ext"` | Natif pymdownx.superfences. Bandeau de titre avec icône de fichier auto-détectée par extension (sur `.codeblock` uniquement). |
| `data-filetype="yaml"` | Force l'icône de type de fichier, indépendamment du texte du titre. |
| `linenums="N"` | Natif pymdownx.highlight. Numéros de ligne, compatibles avec `.codeblock` et `data-peek` (la colonne de numéros est rognée en même temps que le code). |
| `hl_lines="N M-P"` | Natif pymdownx.highlight. Surligne des lignes précises. Fonctionne sur un bloc `.codeblock` en mode fichier statique; pas garanti en mode transcript (bloc reconstruit en segments). |
| `.zebra` | Une ligne sur deux reçoit un fond légèrement différent (`color-mix` à 5%, theme-aware). Exige `.codeblock` sur le même bloc pour s'activer, comme `data-blur`. En mode transcript, le zébrage repart de zéro à chaque commande plutôt que de continuer sur tout le bloc. |

Automatisme sans attribut: chaque ligne `$ ...` d'un transcript reçoit automatiquement l'icône terminal.

## Exemple

En-tête utilisé pour le bloc ci-dessous:

``` { .text .codeblock }
{ .yaml .codeblock .zebra data-blur="5-7" data-peek="5" title="crossplane/providers/provider-aws-config.yaml" linenums="1" hl_lines="3" }
```

``` { .yaml .codeblock .zebra data-blur="5-7" data-peek="5" title="crossplane/providers/provider-aws-config.yaml" linenums="1" hl_lines="3" }
apiVersion: aws.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: aws-secret
      key: creds
```

## Exemple: transcript multi-commandes

Seul un prompt `$ ` en tout début de ligne est reconnu comme une commande (voir `.codeblock` dans le tableau plus haut), un vrai prompt shell du style `user@host:~$` ne l'est pas: la seconde commande ci-dessous est donc écrite `$ apt list --upgradable` plutôt que `mathod@Mathod:~$ apt list --upgradable`. Chaque commande obtient son propre bloc de sortie repliable.

En-tête utilisé pour le bloc ci-dessous:

```` { .shell .codeblock }
{ .shell .codeblock }
````

``` { .shell .codeblock }
$ sudo apt update
Hit:1 http://deb.debian.org/debian trixie InRelease
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 https://apt.releases.hashicorp.com trixie InRelease [12.9 kB]
Get:5 http://deb.debian.org/debian-security trixie-security/main amd64 Packages [251 kB]
Get:6 https://apt.releases.hashicorp.com trixie/main amd64 Packages [316 kB]
Fetched 671 kB in 0s (5,349 kB/s)
8 packages can be upgraded. Run 'apt list --upgradable' to see them.
$ apt list --upgradable
chromium-common/stable-security 152.0.7977.82-1~deb13u1 amd64 [upgradable from: 151.0.7922.169-1~deb13u1]
chromium-sandbox/stable-security 152.0.7977.82-1~deb13u1 amd64 [upgradable from: 151.0.7922.169-1~deb13u1]
chromium/stable-security 152.0.7977.82-1~deb13u1 amd64 [upgradable from: 151.0.7922.169-1~deb13u1]
libssl3t64/stable-security 3.5.7-1~deb13u2 amd64 [upgradable from: 3.5.6-1~deb13u2]
linux-libc-dev/stable-security 6.12.107-1 all [upgradable from: 6.12.101-1]
openssl-provider-legacy/stable-security 3.5.7-1~deb13u2 amd64 [upgradable from: 3.5.6-1~deb13u2]
openssl/stable-security 3.5.7-1~deb13u2 amd64 [upgradable from: 3.5.6-1~deb13u2]
terraform/trixie 1.16.1-1 amd64 [upgradable from: 1.16.0-1]
```