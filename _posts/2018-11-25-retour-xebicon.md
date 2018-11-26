---
layout: post
title:  "Retour sur la Xebicon 2018"
description: "Xebicon 2018 du 20 novembre, ce que j'ai aime et retenu"
date:   2018-11-25 11:00:00
tags: conference
---

Le 20 novembre dernier avait lieu la Xebicon, la conférence annuel de la SS2I Xebia. La conf avait lieu dans le magnifique palais Brongnart ancienne bourse de Paris. Voici mon retour sur les confs que j'ai trouvé particulièrement intéressante.

![alt text](/images/palais-brongniart-lol.jpg "Palais Brongniart de Paris")

Au programme :

* [Optimisation d'enchère adWords par Oui.sncf](#adwords)
* [MlFlow, la datascience de façon un peu plus pro](#mlflow)
* [Une vulgarisation du deepLearning distribué](#dl-distributed)
* [Zeppelin comme outil de reporting](#zeppelin)
* [sparkNLP : traitement diu langage naturel avec Spark](#spark)
* [comment tricher avec vos DataViz](#dataviz)
* [Deno, le "peut-être" futur node.js](#deno)

### Optimisation d'enchère adWords par Oui.sncf <a class="anchor" id="adwords"></a>

La feature team Astro de chez Oui.sncf est venue nous montrer le projet OuiBoost qu'ils ont réalisé en partenariat avec Xebia. Ce produit permet aux webmarketers d'optimiser le ROI de leurs campagnes adWords en leur permettant de choisir les meilleurs mots clés et la meilleure enchère. En gros avoir une enchère minimale pour un maximum de clic.

Ils utilisent l'APIs google d'adWords pour obtenir leur données d'entrainement et leur pipeline est composé de 2 prédicteurs et d'un moteur d'optimisation.

* Un premier modèle permet de prédire le coût pour un mot clé donné. Si je mets 1€ sur "train lille paris" combien vais-je payer.
* Un second modèle permet de prédire le revenu générer pour un mot clé. Si j'achète "train lille paris", combien vais-je gagner.
* Un moteur d'optimisation de type [glouton](https://fr.wikipedia.org/wiki/Algorithme_glouton) permet ensuite de trouver la combinaison mot-clé/enchère qui maximise le revenu.

Ils ne sont pas entré réellement dans la technique comme je l'espérais mais en fouinant sur le web on devrait pouvoir trouver de l'info. Par exemple, OVH semble s'y être intéréssé également : [https://remibacha.com/data-science-seo/](https://remibacha.com/data-science-seo/)

### MlFlow, la datascience de façon un peu plus pro <a class="anchor" id="mlflow"></a>

3 modules dans ce framework open source pour bosser plus proprement en datascience :

* Tracking :

Garder une trace des paramètres d'un modéle entrainé et de ces résultats

* Projects :

Packaging d'une expérimentation pour la rendre reproduisible : data d'entrainement de validation, model et paramètres dans un fichier yml

* Models :

Build et pousse un livrable permettant de pousser en production un modèle, par exemple un fichier pkl à destination d'une webapp en prod permettant de faire de prédiction avec la version n - 1.

Très prometteur, je m'en vais approfondir ça de ce pas !

### Une vulgarisation du deepLearning distribué <a class="anchor" id="dl-distributed"></a>

Une petite prez d'un quart d'heure pour expliquer comment se passe le ml distribué pour pouvoir entrainer des réseaux de neuronnes très profond avec des millions de données d'entrainement.

Duplicate et synchronize VS Split le réseaux
Le choix en fonction du type de réseau et des calculs qu'il implique et de la volumétrie des données que l'on a.

Horovod : Uber distributed framework
Keras : Distributed Keras ou Elephas = librairie pour faire du distribué

### Zeppelin comme outil de reporting <a class="anchor" id="zeppelin"></a>

Romain Sagean est venue nous vendre les mérite de [Apache Zeppelin](https://zeppelin.apache.org/) face à son concurrent Jupyter.

Romain a mit en place Zeppelin comme outil de monitoring pour le projet d'un de ses clients. Etant donné que le monitoring ne se faisait qu'une fois par jour, un ELK leurs paraissaient trop lourd à mettre en place pour leur besoin. Avec les capacités de Zeppelin listées ci-dessous, l'outil a tout son sens.

Voici ce que Zeppelin peut faire :

* cacher le code des cellules du notebook pour le passer en mode reporting
* gestion des droits utilisateurs fines
* beaucoup de langages et sources de données supportés : cassandra, spark, java, scala, python, jdbc, shell...
* on peut mélanger tout ça à l'intérieur d'un meme notebook !
* interraction avec un notebook ou Zeppelin via API : mise en prod via CI/CD du notebook par exemple
* planification de l'execution d'un notebook via un CRON.
* l'interpreteur angular coté navigateur permet de faire de belle dataviz

J'en oublie surement encore beaucoup, je vous laisse voir la doc si ça vous intéresse.

### sparkNLP : traitement du langage naturel avec Spark <a class="anchor" id="spark"></a>

Je ne connais que les grandes lignes de Spark et je n'ai jamais eu l'occasion de jouer avec. Dans cette conf, @auroredea nous présente la trés récente API spark-NLP qui vient juste au dessus de spark-ML.

J'imagine qu'avec la puissance d'un cluster Spark, on pourrait entrainer des models permettant de faire de la NER assez rapidement et sur de gros volumes de données (dont il faut déjà être assez chanceux pour les avoir à disposition).

Par contre, en apprennant dans ce talk que les models pré-entrainé de spark-ML ne sont dispo que pour l'anglais et qu'on ne peut entrainer des models que sur des jeux de moins de 2Mo, je me dis qu'il reste encore de la route pour rivaliser avec des librairies comme Spacy et qui ont le bon goût d'etre en python :-)

### comment tricher avec vos DataViz <a class="anchor" id="dataviz"></a>

Sous un titre volontairement provocateur @Romain Sagean nous montre quelques ficelles pour tricher dans nos graphiques.

Il nous montre comment on peut diminuer l'effet visuel d'une baisse dans un line chart en augmentant l'épaisseur du trait ou même en le passant en 3D.

En manipulant un peu les échelles, on peut facilement augmenter l'effet visuel d'une augmentation ou diminuer son effet avec des bars charts.

De manière général, toujours utiliser les line chart pour montrer une évolution dans le temps et les bar charts pour faire de la comparaison.

Concernant les fameux camembert, autrement appeler pie chart, les découper en leurs centres pour leur donner un effet Donuts TM est une mauvaise idée meme si ca donne un vrai style et une patte de designer. En effet, selon Romain, 25% des gens regardent l'angle du camembert pour visualiser la part d'une catégorie quand les autres regardent la périphérie.

La 3D est toujours à prescrire, en mettant volontairement une catégorie sous représentées en haut du camembert, on augmente l'impression de son importance artificiellement.

Les couleurs ont leurs importance, un bar chart qui compare des pays avec la France en rose et le Brésil en violet, c'est du grand n'importe quoi.

Et pour finir quelque chose de vraiment basique. Quand on présente un tableau tout bête, le trier sur les labels (ex: le nom des pays) est souvent une mauvaise idée. Il vaut mieux trier sur le chiffre d'affaire du pays dans l'ordre que l'on veut par exemple.

### Deno, le "peut-être" futur node.js <a class="anchor" id="deno"></a>

Rohald Dahl, le créateur de node.js en 2009 n'est apparemment pas très satisfait de son travail initial et de ce qu'est devenu node.js et son ecosystème.

Le projet a démarré il y a 6 mois seulement mais est apparemment très actif.

pour l'instant bien à l'arrache : lister ce que j'ai compris pas mettre en 1er.
