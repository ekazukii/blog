---
author: Ekazuki
title: Heberger un site internet sur la blockchain avec ENS et IPFS
date: 2022-05-14T22:00:00+00:00
description: Heberger un site internet statique sur IPFS en utilisant Etheureum Name Service ou Unstoppable Domains
tags:
  - blockchain
  - ENS
  - Ethereum
  - IPFS
hideMeta: false
searchHidden: false
ShowBreadCrumbs: true
---

Dans cet article, on va voir les différentes technologies permettant d'héberger un site internet statique de manière décentralisé, ce ne sera pas un tutoriel expliquant pas à pas comment l'héberger.

#### Pourquoi

L'Hébergement décentralisé résout plusieurs problèmes que l'on peut retrouver sur l'hébergement traditionnel, à commencer évidemment par la confiance, vous n'avez pas à reposer sur la bonne volonté de votre hébergeur et vous êtes donc bien plus résistant à une censure de votre site. De plus, la décentralisation permet d'éviter les singles points of failure, si une node du réseau tombe il en reste encore des centaines, des milliers. Les technologies que l'on va voir ne pourront être utilisé que pour des sites internet statiques et évidemment toutes les données seront publiques donc il faut soit chiffrer les données soit ne pas envoyer de données sensibles.

#### IPFS

IPFS ou InterPlanetary File System est un réseau de stockage de fichier décentralisé, il permet de stocker des fichiers à travers un réseau d'ordinateurs connectés pairs-à-pairs. IPFS permet de stocker des fichiers gratuitement sur son réseau tout en réduisant le risque de crash d'un serveur par son aspect décentralisé.

Le protocole fonctionne en associant un fichier (ou un répertoire de fichier) au hash de son contenu, c'est ce que l'on appelle les CIDs (Content Identifiers), c'est ce CID qui sera utilisé pour retrouver le fichier sur le réseau (par analogie avec le web traditionnel, on pourrait comparer le CID à l'url d'un site internet.). Quand un client va faire une requête à un nœud IPFS, ce nœud va voir s'il possède le fichier associé au CID, s'il l'a, il va le retourner au client et s'il ne l'a pas, il va renvoyer la requête aux nodes voisines qui exécuteront le même protocole jusqu'à trouver une node possédant le fichier.

Pour enregistrer des fichiers sur le réseau vous devez avoir une node IPFS pour ça, vous avez plusieurs options, vous pouvez télécharger (IPFS Desktop)[https://docs.ipfs.io/install/ipfs-desktop/] qui permet d'avoir une node sur son propre ordinateur, mais vous ne pourrez accéder à vos fichiers que quand il sera allumé, vous pouvez également faire tourner une node IPFS sur votre NAS, un vps ou un serveur dédié, mais au prix d'un abonnement au mois. Finalement, vous pouvez utiliser un service comme Pinata qui va s'occuper d'heberger vos fichiers pour vous gratuitement.

Pour héberger son site internet sur IPFS, il va falloir que tous les liens sur le site soient relatifs, car vous ne pouvez pas vraiment connaître le CID avant de l'envoyer sur IPFS. Il faudra ensuite mettre tout votre site dans un dossier et avoir un fichier index.html à la racine de votre dossier, une fois que c'est fait publier le dossier de votre site sur IPFS (le dossier lui-même pas chaque fichier un par un) et vous pourrez y accéder à travers les clients ipfs par exemple. [https://ipfs.io/ipfs/WEBSITE_FOLDER_HASH/index.html]

#### IPNS

Maintenant on se retrouve avec un hash IPFS, sauf que le hash de votre site est lié au contenu de vos fichiers, et donc à chaque changement sur votre site vous avez un nouveau hash, pas très pratique, vous en conviendrez.

C'est pour cela l'équipe derrière IPFS à introduit IPNS (interplanetary Name System), il permet d'associer votre nodeId à un CID classique de IPFS. Il utilise des méthodes de chiffrement asymétriques pour permettre à l'utilisateur de changer quand il le souhaite le CID associé à son noeud. Pour votre site, cela permet d'avoir une adresse ipns unique qui ne changera pas quand le CID du site change (quand une nouvelle version du site est déployé).

#### Ethereum Name Service

Sauf que le nodeId d'IPNS est le hash d'une clef publique, il n'est donc pas facile à retenir pour un humain, pour pallier à ce problème, on peut utiliser Ethereum Name Service.

Si vous êtes sur Twitter, vous avez probablement déjà vu passer des pseudos d'utilisateur finissant par .eth, il s'agit justement d'ENS (Ethereum Name Service), un service permettant de lier un nom de domaine enregistré sur la blockchain ethereum à différentes une adresse ethereum, un compte Twitter ou ce qui nous intéresse un hash IPFS (ou ipns).

Pour cela, comme un nom de domaine classique vous pouvez le réserver sur (le site d'ENS)[https://ens.domains/] et vous pourrez ensuite associé le domaine à différents enregistrements. Comme pour les dns classiques il en existe une multitude, que ce soit des adresses de wallet de cryptomonnaies à une adresse e-mail ou une photo d'avatar. Mais l'enregistrement qui nous intéresse est celui du "content" il permet d'associer son nom de domaine .eth à une url, un CID ipfs, un hash IPNS. Dans notre cas pour lier notre .eth à notre site, il faut "mettre" l'enregistrement "content" à ipns://IPNS_HASH

Vous vous demandez peut-être pourquoi on utilise encore ipns avec ENS et pas simplement un enregistrement IPFS en le changeant à chaque changement du site, le problème étant que mettre à jour IPNS est gratuit alors que changer un enregistrement ENS coûte de l'argent du fait des frais de transactions ethereum (ENS vit sur la blockchain ethereum là ou ipns est sur le réseau ipfs). C'est pour cela qu'on préfère effectuer le moins de changement possible sur les enregistrements ENS.

Aussi, Ethereum Name Service est dirigé par une DAO, c'est-à-dire que le pouvoir de décision est partagé entre toutes les personnes détenant des tokens $ENS (donné à chaque possesseur d'un nom de domaine).

#### Unstoppable Domains

Néanmoins, ENS n'est pas le seul système de noms de domaine sur la blockchain, son "concurrent" majeur se nomme Unstoppable Domains, sauf qu'ils ont une philosophie assez différente, en effet là où ENS cherche plutôt à faire un annuaire de la blockchain à travers un unique tld le fameux.eth, Unstoppable Domains se focalise sur la résolution de site internet avec plusieurs nouveaux nom de domaine (.nft .dao et bien d'autre) et une intégration aux navigateurs (natif pour brave et opera et avec des extensions pour les autres).

Également Unstoppable Domains offre la possibilité de mint ses domaines sur la blockchain polygon pour presque aucuns frais de transactions sur le mint et les changements d'enregistrement dns.

Il est plus récent et moins connu que ENS (entendez donc, bien moins audité.) mais il n'en reste pas moins une alternative sérieuse, d'autant plus que les domaines achetés sur Unstoppable Domains sont permanant, vous n'avez qu'un achat à faire et vous le garderez à vie.
