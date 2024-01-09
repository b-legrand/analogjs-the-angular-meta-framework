# Analogjs, le meta-framework pour angular que l'on n'attendait plus. 

<div style="text-align: center">
![](./public/analog-logo.svg)
</div>

Les slides sont [déployés sur github-pages](https://b-legrand.github.io/analogjs-the-angular-meta-framework/).

## Abstract

```
AnalogJS est un nouveau venu dans le monde des meta-frameworks.

Qu'est ce qu'un meta-framework déjà ? Chaque framework SPA en a un, React a Next.js ou Remix, Vue.JS a Nuxt, Svelte a sveltekit, Qwik a Qwik city

Et maintenant Angular a AnalogJS. Qu'est ce qu'on peut faire avec ce genre de solutions ?

Du routage basé sur l'arborescence de répertoire et de fichiers, du rendu serveur dès la sortie de la boite, et plein d'autres fonctionnalités...

Voyons ensemble comment tout ça fonctionne et pourquoi c'est super-ultra-méga-cool.

```

## Plan

- Introduction ( définissons quelques termes... ) ( 10 minutes )
  - SPA
  - SSR
  - SSG
  - JamStack ?
  - tout ca avec angular aujourd'hui était compliqué ( scully / angular/universal )
- L'éco-systèmes des meta-frameworks et leurs features ( 10 minutes )
  - Vue et Nuxt, React et Next, Svelte et SvelteKit etc...
  - le File-based routing
  - le pré rendu
  - la gestion des tags meta
  - markdown et front-matter
  - les avantages et les inconvénients des meta-frameworks
- Comment AnalogJS s'inspire de tout ça pour faire mieux ( 10 minutes )
  - content based components
  - toute la puissance du Router angular est toujours là
  - Vite et Vitest
  - markdown, marked et mermaid "out of the box"
  - les schematics angular et les generators
  - intégration avec des solutions de déploiement
- h3 / nitro ( 5 minutes )
  - api routes
  - get / post
  - query / get params
  - cas d'usage: sitemap, rss, bff
- Démo ( 5 minutes )
  - generate app from scratch
  - generate pages, component.md
  - intégration
- Conclusion ( 5 minutes )
  - Cas d'usages ou AnalogJS est intéressant
  - Quand ne pas l'utiliser
  - Réfléchissez au cycle de vie de votre donnée
    - mise en cache
    - péremption
    - tout n'est pas API

 

## Sources

- [la doc officielle](https://analogjs.org/)
- [la chaine youtube de Brandon Roberts](https://www.youtube.com/@BrandonRobertsDev)
- [la doc de nitro](https://nitro.unjs.io/)