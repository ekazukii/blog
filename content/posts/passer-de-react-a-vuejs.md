---
author: Ekazuki
title: Pourquoi passer de React à NuxtJS (donc Vue.js)
date: 2022-06-07T22:00:00+00:00
description: Les nouvelles fonctionnalités de Nuxt3 et pourquoi faudrait-il l'utiliser ?
tags:
  - React
  - Javascript
  - Vue
  - Nuxt
hideMeta: false
searchHidden: false
ShowBreadCrumbs: true
draft: true
---

Depuis maintenant presque deux mois la version 3.0 de Nuxt.js est sorti en version stable, elle propose de nouvelles fonctionnalités qui, sur le papier, ont l'air absolument incroyable.

## C'est quoi NuxtJs concretement

Si vous êtes développeur React, vous avez probablement déjà entendu parler de Next.js, le framework permettant, entre autre, de faire du server side rendering avec React, et bien Nuxt, c'est son alternative pour les utilisateurs de Vuejs.

Pour expliquer plus globalement, là où Vuejs permet de faire une application complètement coté client (s'exécutant sur le navigateur) en utilisant une seule page web, Nuxt.js permet de faire plusieurs pages web (Plus optimisé pour le SEO, car on peut changer les titres, descriptions et autres balises meta pour chaque page) en SSR (server side rendering). C'est-à-dire qu'à la place d'exécuter le javascript permettant de transformer le composant Vue en du HTML/CSS/JS sur le navigateur du client, on l'exécute directement sur le serveur et on envoie le fichier fini au client.

Le SSR n'est pas le seul mode de rendu de Nuxt.js, il propose aussi un rendu classique au niveau du client, permettant d'upgrade Vue.js avec le système d'API de Nuxt.js. On peut aussi opter pour du SSG (Static site generation) qui permet de générer la page HTML/JS/CSS directement au moment de build le projet, c'est utile pour des projet plutôt statique ou le contenu ne change que très rarement comme un blog par exemple 👀. On pourra aussi dans un futur proche opter pour un mixe de ses options, pour un mode de rendu hybride ou certaines pages sont générer au moment du bail, d'autre sont en SSR et d'autre en page Vue.js classique.

## Qu'apporte la nouvelle version ?
