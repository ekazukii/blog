---
author: Ekazuki
title: Heberger un site internet sur la blockchain avec ENS et IPFS
date: 2022-04-27T22:00:00+00:00
description: EZZZZ
tags:
  - blockchain
  - ENS
  - Ethereum
  - IPFS
hideMeta: false
searchHidden: false
ShowBreadCrumbs: true
draft: true
---

#### Pourquoi

    -> Décentralisation
    -> No single point of failure (ou presque)
    -> Seulement site statique

#### IPFS

IPFS ou InterPlanetary File System est un reseau de stockage de fichier decentralisé, il permet de stocker de manière permanante (ou presque on y reviendra) des fichiers à travers un reseau d'ordinateur connectés pairs-à-pairs. IPFS permet de stocker des fichiers gratuitement sur son reseau tout en réduisant le risque de crash d'un serveur par son aspect décentralisé.

Le protocole fonctionne en associant un fichier (ou un repertoire de fichier) au hash de son contenu c'est ce que l'on appelle les CIDs (Content Identifiers), c'est ce CID qui sera utilisé pour retrouver le fichier sur le reseau (par analogie avec le web traditionnel on pourrait comparer le CID à l'url d'un site internet). Quand un client va faire une requete à un noeud IPFS ce noead va voir s'il possede le fichier associé au CID, si il l'a il va le retourner au client et s'il ne l'a pas il va renvoyer la requetes aux nodes voisines qui executerons le même protocole jusqu'a trouver une node possedant le fichier.

Pour enregistrer des fichiers sur le réseau vous devez avoir une node IPFS pour ça vous avez plusieurs options vous pouvez télécharger (IPFS Desktop)[https://docs.ipfs.io/install/ipfs-desktop/] qui permet d'avoir une node sur propre ordinateur mais vous ne pourrez acceder a vos fichier que quand il sera allumé, vous pouvez également faire tourner une node IPFS sur un vps, un serveur dédié, ou votre NAS mais au prix d'un abonnement au mois. Finalement vous pouvez utilisé un service comme Pinata qui va s'occuper de pin vos fichier pour vous gratuitement.

Pour heberger son site internet sur IPFS il va falloir que tout les liens sur le site soient relatifs car vous ne pouvez pas vraiment connaire le CID avant de l'envoyer sur IPFS. Il faudra ensuite mettre tout votre site dans un dossier et avoir un fichier index.html à la racine de votre dossier, une fois que c'est fait publiez le dossier de votre site sur ipfs (le dossier lui même pas chaque fichier un par un) et vous pourrez y acceder à travers les clients ipfs par exemple [https://ipfs.io/ipfs/WEBSITE_FOLDER_HASH/index.html]

#### IPNS

Maintenant on se retrouve avec un hash IPFS, sauf que le hash de votre site est immutable donc a chaque changement de votre site vous avez un nouveau hash, pas très pratique vous en conviendrez.

Pour cela l'équipe derriere IPFS à introduit en XXXX IPNS (interplanetary Name System), il permet de generer un hash IPNS pointant vers un autre hash ipfs classique, la subtilité est que le hash IPNS peut changer de cible à la volonter du créateur du hash et donc peut changer à chaque fois que votre site se mets à jour. On a donc un hash unique que l'on mets à jour a chaque deploiment du site, beaucoup plus simple pour l'utilisateur.

#### Swarm

Swarm nous introduit le DSaaS (Decentralized storage as a service) c'est un système de stockage et de communication décentralisé et pair-à-pair, contrairement à ipfs il est payant et marche par abonnement. Son mode de fonctionnement est proche d'une blockchain sans tout à fait l'être,

#### Ethereum Name Service

Si vous êtes sur Twitter vous avez probablement déja vu passer des pseudos d'utilisateur finissant par .eth, il s'agit en général d'Ethereum Name Service, un service permettant de lier un nom de domaine enregistré sur la blockchain ethereum à differentes "choses".

Le service est utilisé majoritairement pour associé une addresse ethereum (e.g. 0x2AAB340134e5a146Dc9670F09b63E596a2c1b4d2) à une addresse simple (e.g. ekazuki.eth) plus simple à retenir pour effectuer des transactions. Mais au fur et a mesure du temps il s'est developpé pour proposer une veritable identité numérique du web3.0, il permet désormais d'associé à son nom de domaine un NFT, un compte twitter et évidemment une URL. /!\Recherche comment marche le resolveur ENS/!\

C'est cette fonctionalité que l'on va utilisé pour associé notre nom de domaine .eth à un hash ipfs (ou ipns).

#### Unstoppable Domains

Néanmoins ENS n'est pas le seul DNS sur la blockchain, son "concurrent" majeur se nomme Unstoppable Domains, sauf qu'ils ont une philosophie assez differente, en effet la ou ENS cherche plutôt a faire un annuiare de la blockchain à travers un unique tld le fameux .eth, Unstoppable Domains se focalise sur la résolution de site internet avec plusieurs nouveaux nom de domaine (.nft .dao et bien d'autre) et une ingération aux navigateur (natif pour brave et opera et avec des extensions pour les autres).

Egalement Unstoppable Domains offre la possibilité de mint ses domaines sur la blockchain polygon pour presque aucun frais de transactions sur le mint et les changement de records.

Il est bien plus récent que ENS (entendez donc, bien moins audité) mais il n'en reste pas moins une alternative serieuse, d'autant plus que les domaines achetés sur Unstoppable Domains sont permanant, vous n'avez qu'un achat à faire et vous le garderez à vie.
