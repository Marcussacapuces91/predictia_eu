Je recommande une configuration Jekyll **minimaliste, sans thème**, afin de conserver exactement le rendu actuel de `docs/index.html` et `docs/style.css`.

Le site actuel est déjà une page HTML statique fonctionnelle. Jekyll servira surtout à :

- centraliser les métadonnées ;
- gérer correctement les chemins GitHub Pages ;
- permettre l’ajout futur de layouts, pages et collections ;
- conserver le CSS et le design existants.

## Structure recommandée

```text
docs/
├── _config.yml
├── index.html
├── style.css
└── assets/
```

## Configuration `docs/_config.yml`

````yaml name=docs/_config.yml
title: PredictIA | Maintenance prédictive intelligente
description: Plateforme d'hypervision convergée et de maintenance prédictive intelligente.
lang: fr
encoding: utf-8

# Pour un domaine personnalisé comme predictia.eu :
url: "https://predictia.eu"
baseurl: ""

# Si le site est utilisé uniquement sur GitHub Pages,
# remplacer baseurl par :
# baseurl: "/predictia_eu"

theme: null

# Le CSS existant est conservé tel quel.
include:
  - style.css

exclude:
  - Gemfile
  - Gemfile.lock
  - README.md
  - vendor
````

## Adapter `docs/index.html`

Ajoutez un front matter Jekyll au début du fichier :

````html name=docs/index.html
---
layout: null
title: PredictIA | Maintenance prédictive intelligente
description: PredictIA connecte vos équipements critiques, analyse leur comportement et anticipe les pannes.
---

<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: 'fr' }}">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>{{ page.title }}</title>
    <meta name="description" content="{{ page.description }}">

    <link rel="stylesheet" href="{{ '/style.css' | relative_url }}">
</head>

<body>
    <!-- Le contenu HTML actuel peut rester inchangé ici -->
</body>
</html>
````

Le contenu actuel de votre page peut donc être conservé intégralement entre `<body>` et `</body>`. Le seul changement réellement indispensable est l’ajout du front matter et le remplacement de :

```html
<link rel="stylesheet" href="style.css">
```

par :

```liquid
<link rel="stylesheet" href="{{ '/style.css' | relative_url }}">
```

## Correction recommandée des ancres

Il existe une petite incohérence dans la navigation actuelle :

```html
<a href="#usages">Usages</a>
```

alors que la section correspondante utilise :

```html
<section id="usage">
```

Utilisez le même identifiant, par exemple :

```html
<a href="#usages">Usages</a>
```

et :

```html
<section id="usages">
```

## `Gemfile` facultatif

Pour reproduire localement le build GitHub Pages :

````ruby name=Gemfile
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
````

Commandes locales :

```bash
bundle install
bundle exec jekyll serve --source docs
```

Le site sera ensuite disponible sur :

```text
http://localhost:4000
```

## Paramétrage GitHub Pages

Dans **Settings → Pages** :

- **Source** : Deploy from a branch
- **Branch** : `main`
- **Folder** : `/docs`
- **Custom domain** : `predictia.eu`, si le domaine est bien configuré

Avec le domaine personnalisé `predictia.eu`, la valeur recommandée est :

```yaml
url: "https://predictia.eu"
baseurl: ""
```

Si le site est consulté uniquement à l’adresse GitHub Pages :

```yaml
url: "https://marcussacapuces91.github.io"
baseurl: "/predictia_eu"
```

Cette approche conserve le rendu actuel, évite d’introduire un thème Jekyll inutile et prépare le projet à évoluer vers des layouts et des pages réutilisables.
