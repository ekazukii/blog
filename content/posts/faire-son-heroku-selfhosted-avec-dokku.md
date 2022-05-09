---
author: Ekazuki
title: Créer sa propre PaaS (Heroku, Netlify) avec Dokku.
date: 2022-04-26T22:00:00+00:00
description:
  Comment créer un clone de Heroku / Vercel sur votre propre machine en
  utilisant Dokku.
tags:
  - self hosted
  - docker
  - dokku
hideMeta: false
searchHidden: false
ShowBreadCrumbs: true
---

### Pourquoi faire sa propre Platform as a Service ?

Les PaaS modernes possèdent d'innombrables avantages à commencer par la simplicité d'utilisation, plus besoin d'être devops pour déployer son site sur internet, il suffit de quelques clics. Mais ces solutions ne sont pas magiques, elles coûtent en général plus cher que les services d'hostings traditionnels et permettent moins de contrôle sur les machines hébergeant votre serveur.

C'est pour cela que nous allons voir dans cet article comment installer une PaaS sur notre propre hardware (ou sur votre VPS / serveur dédié).

### Installation de Dokku

Pour l'installation, vous devez avoir un serveur tournant avec Ubuntu 18.04/20.04 ou Debian 9+. Dokku est compatible avec des architectures ARM si vous comptez par exemple le faire tourner sur une Raspberry PI.

Vous devez télécharger l'installateur (sur votre serveur) et l'exécuter avec la dernière version en date de dokku. (Pour les fanatiques de docker vous pouvez trouver l'image ici [https://hub.docker.com/r/dokku/dokku](https://hub.docker.com/r/dokku/dokku "https://hub.docker.com/r/dokku/dokku") mais l'installation est un peu plus longue).

```shell
wget https://raw.githubusercontent.com/dokku/dokku/v0.27.1/bootstrap.sh
sudo DOKKU_TAG=v0.27.1 bash bootstrap.sh
```

Ensuite, il faut configurer Dokku, pas besoin de modifier de fichier, il suffit de rentrer deux commandes. Dokku marche comme Heroku c'est-à-dire qu'il va déployer un serveur git pour chaque application sur le serveur, il faut donc générer des clefs ssh pour s'authentifier avec git (ce que vous avez deja probablement si vous utilisez Github)

Si vous avez déjà importer vos clefs ssh sur le serveur, il suffit de les importer depuis le fichier authorized_keys, sinon je vous laisse les importer sur votre serveur, c'est assez simple.

```shell
cat ~/.ssh/authorized_keys | dokku ssh-keys:add admin
```

Dernière étape, il faut donner le nom de domaine global de votre site, par exemple si vous avez le domaine `site.com` et deux applications : blog et portfolio vous y accéderez via `blog.site.com`et `portfolio.site.com`(C'est bien évidemment configurable.). Si vous n'avez pas de nom de domaine, vous pouvez utiliser [https://sslip.io/](https://sslip.io/ "https://sslip.io/"), cela permet d'avoir un nom de domaine grâce a votre ip. Par exemple si votre ip est 198.0.0.1 vous aurez `198.0.0.1.sslip.io` **et tous les sous-domaines** qui renverront vers votre site.

```shell
dokku domains:set-global site.com
```

Et voila, c'est tout ! L'installation de dokku sur votre serveur est terminé. On va ensuite déployer une application nodejs simple.

### Installation d'une application Node.js

Un des principaux atouts de Dokku est qu'il embarque les buildpacks de Heroku (en utilisant [herokuish](https://github.com/gliderlabs/herokuish)) et donc ses milliers d'intégrations. Dans notre cas, on va installer l'application Node.js d'exemple de Heroku.

Sur le serveur ou dokku est installé on va créer une nouvelle application dokku que l'on va appeler node-app.

```shell
dokku apps:create node-app
```

Pour commencer, il va falloir cloner le repository sur votre ordinateur pour ça rien de plus simple on fait :

```shell
git clone https://github.com/heroku/node-js-getting-started.git
```

Vous pouvez remarquer que dans le dossier que vous venez de cloner il y a un fichier Procfile (notez la majuscule, elle est importante) contenant l'instruction `web: npm start` . C'est elle qui va indiquer a Dokku la commande qu'il faut lancer au démarrage de l'application. Dans le cas d'une application Node.js qu'il n'y a pas besoin d'indiquer de buildpack, car Dokku l'installe automatiquement quand il trouve un package.json.

On va ensuite ajouter le serveur dokku en tant que nouvelle remote git puis push le code vers l'instance Dokku.

```shell
git remote add dokku ssh://dokku@IP_ADDRESS/node-app
git push dokku master
```

Et voilà ! C'est fait, vous pouvez désormais accéder à votre application à l'adresse [http://blog.SITE.COM]()

### Deployer une application React avec Dokku

Comme beaucoup de stack le déploiement d'une application react demande quelques étapes de plus. En effet contrairement à une application nodejs dokku ne détecte pas le buildpack automatiquement, il faut donc faire un fichier .buildpacks dans la racine du projet et y inclure le buildpack de React.

```shell
echo "https://github.com/mars/create-react-app-buildpack.git" > .buildpacks
```

Il faut ensuite modifier le fichier Procfile pour dire à Dokku de servir le dossier. On remplace donc le `web: npm start` par `web: bin/boot` qui servira le dossier build générer par react.

Et c'est tout ce qu'il faut pour déployer une application react sur Dokku, vous pouvez voir la liste des quelque 9 000 buildpacks créer par la communauté à l'adresse https://elements.heroku.com/buildpacks. Vous pouvez aussi installer plusieurs buildpacks au sein du même projet (C'est ce que fait le buildpack React, il installe celui de node pour build le projet et celui de nginx pour le servir.).

### Ajouter des certificats SSL

Sauf que pour le moment, vos applications sont servies uniquement en http pas en https, pour se faire il y a deux options : 1. vous avez déjà un certificat ssl pour votre domaine et 2. Vous n'avez pas encore de certificat ssl.

#### 1. Importer ses certificats sur dokku.

Vous devez donc avoir au moins deux fichiers, un fichier .crt qui contient le certificiat de votre site et la clef privé associée en .key. Il faut donc associé votre certificat aux applications dokku que vous voulez passer en https pour cela rien de plus simple

```shell
dokku certs:add node-app server.crt server.key
```

#### 2. Vous n'avez pas encore de certificat.

Si vous n'avez pas de certificat ssl avec votre domaine ce n'est pas un problème car dokku à un plugin permettant de générer automatiquement un certificat ssl via [Let's Encrypt](https://letsencrypt.org/fr/). Pour cela, il suffit de l'installer depuis github, de configurer un email (obligatoire pour let's encrypt) et d'activer le cron-job permettant de renouveler le certificat automatiquement.

```shell
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku config:set --global DOKKU_LETSENCRYPT_EMAIL=your-email@your.domain.com
dokku letsencrypt:cron-job --add
```

Finalement on ajoute le certificat à notre application

```shell
dokku letsencrypt:enable node-app
```

J'espère que vous avez apprécié cet article et qu'il vous aura été utile, si vous avez besoin de plus d'information vous pouvez aller faire un tour sur la [documentation de Dokku](https://dokku.com/docs/getting-started/installation/).

N'hésitez pas à me suivre sur [Twitter](https://twitter.com/ekazukiii) !
