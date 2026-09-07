# Placeholders

Écris `{{ variable }}` n'importe où dans le texte d'une page: ça devient une zone éditable directement dans le navigateur. Change la valeur ici, elle se répercute partout où le même nom de variable apparaît sur le site, et reste mémorisée au rechargement de la page.

Essaie: bonjour {{ user }}, ravi de te revoir {{ user }} !

Une autre variable, indépendante de la première: le projet du jour est {{ project }}.

## Dans un bloc de code: `<variable>`

À l'intérieur d'un bloc de code, la syntaxe change pour `<variable>` (convention courante pour un placeholder dans un exemple de config/commande, ex. `<TOKEN>`). Sans danger ici contrairement au texte normal: le code est du texte échappé par Pygments, jamais interprété comme une vraie balise HTML.

Une admonition au-dessus, en texte normal (`{{ }}`), et le bloc `.codeblock` en dessous, en syntaxe code (`<>`), partagent le même nom de variable: modifier l'un met à jour l'autre en direct.

!!! tip "Personnalise cet exemple"
    Nom: {{ name }}, source des credentials: {{ source }}, namespace: {{ namespace }}.

    <button class="placeholder-reset" type="button">Réinitialiser</button>

``` { .yaml .codeblock title="crossplane/providers/provider-aws-config.yaml" }
apiVersion: aws.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: <name>
spec:
  credentials:
    source: <source>
    secretRef:
      namespace: <namespace>
      name: aws-secret
      key: creds
```

## Comment ça marche

- `{{ user }}` (texte normal) ou `<name>` (dans un bloc de code) affichent d'abord leur propre nom par défaut, la première fois.
- Cliquer dedans et taper une valeur met à jour **toutes** les occurrences du même nom sur la page (et sur les autres pages du site, la valeur est mémorisée en `localStorage`).
- `Entrée` valide et quitte la zone de texte au lieu d'insérer un retour à la ligne.
- `{{ variable }}` ne fonctionne que dans le texte normal (jamais dans un bloc de code), `<variable>` ne fonctionne que dans un bloc de code (jamais dans le texte normal): les deux syntaxes ne se marchent jamais dessus.
