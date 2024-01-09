# AnalogJS
![](./analog-logo.svg)

The full-stack Angular meta-framework


## Hi, my name is
<div class="row">
<img src="/assets/images/benjamin-legrand.png" style="border-radius: 100%;width: 40%" />

- Benjamin Legrand <br> [@benjilegnard](https://twitter.com/benjilegnard)
- Tech Lead ( onepoint )
- Angular/Typescript/Node
</div>


## Analog.JS
<div class="row">

<img src="/assets/images/brandon-roberts.jpeg" style="border-radius: 100%;width: 40%; float:left;" />

- Brandon Roberts [@brandontroberts](https://twitter.com/brandontroberts)
- NGRX contributor
- Angular GDE
</div>
---
Auteur d'analogJS
Contributeur ngrx


## Plan
- c'est quoi un meta-framework ?
- les features d'analogjs
- h3 / nitro
---
démo peut-être



## Introduction


### Wait, wat?

- What is a meta-framework ?
- Terms & definitions
- SSR, SSG, CSR, WTF, BBQ.


### SSR
- Server-Side Rendering
---
- Rendu serveur
- historiquement on a toujours fait ca


<img src="/assets/ssr.drawio.svg" style="width: 100%" />


### CSR
- Client Side Rendering


<img src="/assets/csr.drawio.svg" style="width: 80%" />
---
- On distingue du SSR: c'est le browser qui fait la génération du HTML.


### SPA
- Single Page Application
---
- Historiquement: client-side only
- ne veut pas dire qu'on fait qu'une seule page


#### MPA
<img src="/assets/spa-before.drawio.svg" style="width: 80%" />


#### SPA
<img src="/assets/spa-after.drawio.svg" style="width: 80%" />


### SSG
- Static Site Generation


<img src="/assets/ssg.drawio.svg" style="width: 80%" />
---
Conclusion:
- Analog nous permets de faire tout ca, du rendu serveur, des spa angular, de la génération de sites statiques



## Meta-framework ?


### Qu'est ce que c'est ?


#### Take your client-only libs

- Angular
- React
- Vue
- Svelte
- Qwik


#### Put them on a server

- Angular + Analog
- React + Next
- Vue + Nuxt
- Svelte + SvelteKit
- Qwik + Qwik City
---
## Joke
Y U NO NGXT ?


### Add some features
- accès à requête / réponse HTTP<!-- .element: class="fragment" -->
- routage "universel"<!-- .element: class="fragment" -->
- file-based routing<!-- .element: class="fragment" -->
- routes d'api<!-- .element: class="fragment" -->


### Avantages / Inconvénients


### Meta-frameworks:<br/> the good parts

- Même code / composants <br/> entre serveur et client<!-- .element: class="fragment" -->
- Navigations full page et SPA.<!-- .element: class="fragment" -->
- Bénéfices du CSR et du SSR<!-- .element: class="fragment" -->
---
- surtout important on évite le contexte switch
- on évite d'avoir du rendu fait serveur différent de coté client


### Meta-frameworks:<br/> the bad parts

- file-based routing :
```bash
.
├── checkout
│   ├── basket
│   │   └── page.tsx
│   ├── cart
│   │   └── page.tsx
│   └── summary
│       └── page.tsx
├── home
│   └── page.tsx
└── products
    ├── [id]
    │   └── page.tsx
    └── page.tsx
```
---
- index.page.tsx everywhere
- "une des critiques"


#### Meta-frameworks:<br/> the bad parts

- abstractions<!-- .element: class="fragment" -->
- vendor-locking<!-- .element: class="fragment" -->
---
- abstractions bizarres
- use client / use server
- code spécifiques a la solution
- analogjs ne résouds pas forcément ca, car il fournit des abstractions.


## AnalogJS Features


### Vite / Vitest
- AnalogJS is a vite plugin<!-- .element: class="fragment" -->
- Everything is a vite plugin<!-- .element: class="fragment" -->
- NX libs integration<!-- .element: class="fragment" -->
- e2e testing with Playwright<!-- .element: class="fragment" -->
---
- Vite est un bundler nouvelle génération, vient du monde vue, mais est utilisable un peu partout.
- utilise esbuild et rollup en dessous
- quelques autres intégrations


`vite.config.ts`
```ts
import { defineConfig } from 'vite';
import analog from '@analogjs/platform';

export default defineConfig(({ mode }) => ({
  // ...
  plugins: [analog({
    // analog configuration
  })],
  // ...
}));
```


`angular.json`
```json
{ 
  // ...
      "architect": {
        "build": {
          "builder": "@analogjs/platform:vite",
          // ...
        },
        "serve": {
          "builder": "@analogjs/platform:vite-dev-server",
          // ...
        },
        "test": {
          "builder": "@analogjs/platform:vitest"
        }
      }
  // ...
}
```
---
- analog fournit des plugins de build pour compiler votre appli angular
- utilise vite et le compilateur angular via le plugin
- si vous voulez utiliser vite et vitest sans utiliser les autres features d'angular, vous pouvez.
- astro j'ai pas testé. mais en gros c'est plutôt, intégrer de l'angular dans app astro


### Nice libs integration
- NX - <https://nx.dev/>
- Playwright - <https://playwright.dev>
- Astro - <https://astro.build/> 
---
- analogjs a une forte dépendance à @nx
- playwrigth pour les tests de bout en bout


### File-based routing

```
src/app/pages
├── checkout
│   ├── (checkout).page.ts
│   ├── basket.page.ts
│   ├── cart.page.ts
│   └── summary.page.ts
├── home.page.ts
├── (layout).page.ts
└── products
    ├── [id].page.ts
    └── (list).page.tsx
```
---
- chaque fichier .ts contient un composant
- notez les crochets et parenthèse 


#### Donne ces URLs
```
/home
/products
/products/{id}
/checkout/basket
/checkout/cart
/checkout/summary
```
---
- C'est le nommage des fichiers qui a définit nos url's


#### Path params

- \[myParam\].page.ts


#### Pathless page component

- `path.page.ts` => `/path`
- `path/index.page.ts` => `/path`
- `path/(path).page.ts` => `/path`
---
- les trois sont équivalents 


#### Dynamic routes from folder tree
```typescript
const appRoutes: Route[] = [
  {
    path: '',
    component: LayoutPage, // layout.page.ts
    children: [
      {
        path: 'checkout',
        component: CheckoutPage, // checkout/(checkout).page.ts
        children:
        [
          // etc...
        ] 
      }
    ]
  }
]
```
---
- en interne, analogjs reconstruit l'arbre de routage et les chemin pour angular
- vous n'avez plus besoin de ces définitions


#### Page component type
`home.page.ts`
```typescript [|7]
import { Component } from '@angular/core';

@Component({
  standalone: true,
  template: ` <h2>Welcome</h2> `,
})
export default class HomePage {}
```
---
- noter le export default
- tout seul pas suffisant pour reproduire la puissance du routeur


#### Page metadata
```ts
export const meta: RouteMeta = {
  // guards, canActivate, canMatch, etc...
  // guard, resolvers, providers etc...
}

@Component({/** */})
export default class HomePage {}
```
---
- On peut retrouver toute l'API du routeur grâce à cette balise meta
- 


### Markdown as content routes

`src/content/*`
```bash
src
├── app 
├── content
│   ├── mon-premier-article.md
│   └── un-autre-article.md
└── pages
```


#### Front-matter

```markdown
---
title: My First Post
slug: 2022-12-27-my-first-post
description: My First Post Description
coverImage: https://example/url-image/png
---

## titre

contenu au format __markdown__
```


#### Markdown component

```typescript [|1|6-11|24-26|18|19]
// /src/app/pages/articles.[slug].page.ts
import { injectContent, MarkdownComponent } from '@analogjs/content';
import { AsyncPipe, NgIf } from '@angular/common';
import { Component } from '@angular/core';

export interface ArticleAttributes {
  title: string;
  slug: string;
  description: string;
  coverImage: string;
}

@Component({
  standalone: true,
  imports: [MarkdownComponent, AsyncPipe, NgIf],
  template: `
    <ng-container *ngIf="article$ | async as article">
      <h1>{{ article.attributes.title }}</h1>
      <analog-markdown [content]="article.content"></analog-markdown>
    </ng-container>
  `,
})
export default class ArticleComponent {
  readonly article$ = injectContent<ArticleAttributes>({
    param: 'slug', 
  });
}
```
---
- urls vont être `/articles/[slug]` vu le nom du composant
- front-matter récupérables en tant qu'object
- contenu markdown de votre fichier, injecté en tant que html


#### Mermaid support
`app.config.ts`
```typescript
withMarkdownRenderer({
  loadMermaid: () => import('mermaid'),
});
```
---
- transformation markdown, donc possible d'avoir des plugins markdown
- mermaid en est un
- lazy-loading


<!-- .slide: data-background-iframe="http://mermaid.js.org/intro/" data-background-interactive -->


#### Et aussi :

- coloration syntaxique avec prismjs
- extensibilité
---
- fin des features markdown


### Hybrid SSR/SSG

- pré-rendu
- rendu serveur ( nitro )
---
- SSG : il faut lister les pages dans la config
- autre avantage: génération de sitemap.xml 
- SSR : analogjs builde un serveur node.js basé sur nitro pour servir les pages


### Api routes / server
---
- de la même manière que pour nos pages front
- analogjs fournit un système ou on peut utiliser
- l'arborescence des répertoires pour définir nos routes


#### h3 / nitro

<https://nitro.unjs.io/>


#### Définir des routes
`src/server/routes` => /api


`src/server/routes/v1/hello.ts`
```typescript
import { defineEventHandler } from 'h3';

export default defineEventHandler(() => {
  return { message: 'Hello World' }
});
```


```
src/server/routes
└── v1
    ├── users
    │   └── [id].get.ts  // GET /api/v1/users/{id}
    └── users.post.ts    // POST /api/v1/users
```
---
- Noter que le "type" .component.ts est le verbe http.



## Demo



## One more thing...

new component authoring format
- [Discussion github](https://github.com/analogjs/analog/discussions/824)
---
- Vu qu'analog utilise déja le compilateur angular.
- C'est tout frais, semaine dernière


### .ng files
`hello.ng`
```html
<template>
  <p>hello works!</p>
</template>
```


### ts in .ng
```html
<script lang="ts">
  import { signal } from '@angular/core';

  const counter = signal(1);

  const increment = () => {
    counter.update((value) => value + 1);
  };

  function decrement() {
    counter.update((value) => value - 1);
  }
</script>
<template>
  <p>Counter: {{ counter() }}</p>
  <button (click)="increment()">increment</button>
  <button (click)="decrement()">decrement</button>
</template>
<style>
  p { color: red }
</style>
```


### ts in .ng
```html
<script lang="ts">
  import { inject, signal, effect, computed } from '@angular/core';
  import { JsonPipe } from '@angular/common';
  import { HttpClient } from '@angular/common/http';
  import { delay } from 'rxjs';

  import Hello from './hello.ng';
  import Highlight from './highlight.ng';
  import Doubled from './doubled.ng';

  defineMetadata({
    selector: 'app-root',
    imports: [JsonPipe],
  });

  const title = 'Analog';

  const http = inject(HttpClient);

  const counter = signal(1);
  const doubled = computed(() => counter() * 2);
  const todo = signal(null);

  const increment = () => {
    counter.update((value) => value + 1);
  };

  function decrement() {
    counter.update((value) => value - 1);
  }

  effect(() => {
    console.log('counter changed', counter());
  });

  onInit(() => {
    console.log('App init');
    http
      .get('https://jsonplaceholder.typicode.com/todos/1')
      .pipe(delay(2000))
      .subscribe((data) => {
        todo.set(data);
        console.log('data', data);
      });
  });
</script>

<template>
  @if (counter() > 2) {
    <Hello />
  }

  <p>Counter: {{ counter() }}</p>
  <p highlight>Doubled: {{ doubled() }}</p>
  <p>Doubled with Pipe: {{ counter() | doubled }}</p>
  <button (click)="increment()">increment</button>
  <button (click)="decrement()">decrement</button>

  @if (todo(); as todo) {
  <pre>{{todo | json }}</pre>
  } @else {
  <p>Loading todo...</p>
  }
</template>

<style>
  p {
    color: red;
  }
</style>
```


### Vue.js called

They want their shirt back.


### Experimental !

```typescript [|11]
import { defineConfig } from 'vite';
import analog from '@analogjs/platform';

export default defineConfig(({ mode }) => ({
  // ... other config
  plugins: [
    analog({
      ssr: false,
      vite: {
        experimental: {
          dangerouslySupportNgFormat: true
        }
      },
    }),
  ],
}));
```


## Conclusion

- étonnant, non ?
- cas d'usage ou AnalogJS est intéressant
---
- quand déja une base angular
- compatible nx
- certaines partie de l'appli
- sites institutionnel, FAQ / CMS / blogs / docs


### Cycle de vie de la donnée
- mise en cache
- péremption
- tout
---
Cas d'usage ou AnalogJS est intéréssant
- dépends du cycle de vie de 


## Questions

?

## Et merci
