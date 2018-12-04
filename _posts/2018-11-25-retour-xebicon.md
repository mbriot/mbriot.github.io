---
layout: post
title:  "Retour sur la Xebicon 2018"
description: "mon retour sur la Xebicon 2018 du 20 novembre"
date:   2018-11-25 11:00:00
header-img: /images/xebicon2018/xebicon-2018.jpg
tags: conference
---

Le 20 novembre dernier avait lieu la Xebicon, la conférence annuelle de la SS2I Xebia. La conf avait lieu dans le magnifique palais Brongniart ancienne bourse de Paris. Voici mon retour sur les confs que j'ai trouvé particulièrement intéressantes.

![alt text](/images/xebicon2018/palais-brongniart.jpg "Palais Brongniart de Paris")

Au programme :

* [Optimisation d'enchère adWords par Oui.sncf](#adwords)
* [MlFlow, la datascience de façon un peu plus pro](#mlflow)
* [Une vulgarisation du deepLearning distribué](#dl-distributed)
* [Zeppelin comme outil de reporting](#zeppelin)
* [sparkNLP : traitement diu langage naturel avec Spark](#spark)
* [comment tricher avec vos DataViz](#dataviz)

### Optimisation d'enchère adWords par Oui.sncf <a class="anchor" id="adwords"></a>

La feature team Astro de chez Oui.sncf est venue nous montrer le projet OuiBoost qu'ils ont réalisé en partenariat avec Xebia. Ce produit permet aux webmarketers d'optimiser le ROI de leurs campagnes adWords en leur permettant de choisir les meilleurs mots clés pour la meilleure enchère. En gros avoir une enchère minimale pour un maximum de clics tout en maximisant les revenus.

Ils utilisent l'API google AdWords pour obtenir leurs données d'entrainement et leur pipeline est composé de 2 prédicteurs et d'un moteur d'optimisation.

* Un premier modèle permet de prédire le coût pour un mot clé donné. Si je mets 1€ sur "train Lille Paris" combien vais-je payer ?
* Un second modèle permet de prédire le revenu généré pour un mot clé. Si j'achète "train Lille Paris", combien vais-je gagner ?
* Un moteur d'optimisation de type [glouton](https://fr.wikipedia.org/wiki/Algorithme_glouton) permet ensuite de trouver la combinaison mot-clé/enchère qui maximise le revenu.

Ils ne sont pas entrés réellement dans la technique mais en fouinant sur le web on devrait pouvoir trouver de l'info. Par exemple, OVH semble s'y être intéressé également : [https://remibacha.com/data-science-seo/](https://remibacha.com/data-science-seo/)

### MlFlow, la datascience de façon un peu plus pro <a class="anchor" id="mlflow"></a>

Un projet opensource créé par Databricks qui permet d'industrialiser les différentes étapes d'un projet de machine learning, de la phase de recherche à la mise en production.

Le projet se compose de 3 modules :

* Tracking :

Garder une trace des paramètres d'un modèle entraîné et de ses résultats.
On peut ainsi voir dans une belle interface front, les algos choisis, leurs paramètres et les résultats obtenus.
Les données sont sauvegardées via fichiers plats en local ou via API Rest avec un serveur MlFlow dédié.

* Projects :

Ce module permet de packager une expérimentation (un algo, ses paramètres et ses données d'entraînement et de validation). On peut ainsi la partager et rendre notre travail reproductible.
La sortie du packaging est un fichier yaml contenant l'ensemble des infos permettant de reproduire l'expèrimentation.

* Models :

Permet de builder une expérimentation donnée et de la pousser en production. Que ce soit à destination d'une webapp on-premise (flask + scikit-learn) ou d'un service managé type Amazon Sagemaker par exemple.

### Une vulgarisation du deepLearning distribué <a class="anchor" id="dl-distributed"></a>

Une petite prez d'un quart d'heure pour expliquer comment se passe le machine learning distribué avec Keras pour pouvoir entrainer des réseaux de neurones profonds avec des millions de données d'entrainement.

Deux stratégies différentes à choisir en fonction du nombre de données d'entrainement et de la complexité des calculs que le réseau de neurones va devoir faire.

* Duplicate & Synchronize :
  On va ici dupliquer notre réseau de neurones tel quel. On aura donc besoin d'un serveur de paramètres permettant de stocker la valeur des poids des neurones en fonction de leur mise à jour dans les différents clones.
  Cette stratégie est utile dans le cas de réseaux simples mais dont les calculs sont longs (réseaux convolutionnels par exemple). La contrepartie, c'est qu'on va perdre pas mal de temps à synchroniser nos millions de poids sur le réseau. Pour lutter contre ça, il est possible d'augmenter la taille des batchs d'entrainement.

* Split :
  Ici il s'agit de découper le réseau de neurones pour le faire tenir sur plusieurs machines. C'est utile quand l'architecture du réseau de neurones est si complexe qu'il ne tient pas en mémoire sur une seule machine.

Keras dispose de deux librairies pour faire du machine learning distribué : [Distributed Keras](https://github.com/cerndb/dist-keras) et [Elephas](http://maxpumperla.com/elephas/)
La librairie [Horovod](https://eng.uber.com/horovod/) d'Uber existe également.

### Zeppelin comme outil de reporting <a class="anchor" id="zeppelin"></a>

Romain Sagean est venu nous vendre les mérites de [Apache Zeppelin](https://zeppelin.apache.org/) face à son concurrent Jupyter.

Romain a mis en place Zeppelin comme outil de monitoring pour le projet d'un de ses clients. Etant donné que le monitoring ne se faisait qu'une fois par jour, un ELK leur paraissait trop lourd à mettre en place pour leur besoin. Avec les capacités de Zeppelin listées ci-dessous, l'outil a tout son sens.

Voici ce que Zeppelin peut faire :

* cacher le code des cellules du notebook pour le passer en mode reporting
* gérer les droits utilisateurs finement
* supporter beaucoup de languages et sources de données : cassandra, spark, java, scala, python, jdbc, shell...
* mélanger tout ça à l'intérieur d'un même notebook !
* interagir avec un notebook ou Zeppelin via API : mise en prod via CI/CD du notebook par exemple
* planifier l'exécution d'un notebook via un CRON.
* l'interpréteur angular côté navigateur permet de faire de belles dataviz

J'en oublie surement encore beaucoup, je vous laisse voir la doc si ça vous intéresse.

### sparkNLP : traitement du language naturel avec Spark <a class="anchor" id="spark"></a>

Dans cette conf, Aurore de Amaral nous présente la récente API spark-NLP qui vient juste au dessus de spark-ML.

J'imagine qu'avec la puissance d'un cluster Spark, on pourrait entrainer des modèles permettant de faire de la NER assez rapidement et sur de gros volumes de données (il faut déjà être assez chanceux pour en disposer...).

Par contre, en apprenant dans ce talk que les modèles pré-entraînés de spark-ML ne sont disponibles que pour l'anglais et qu'on ne peut en entraîner que sur des jeux de moins de 2Mo, je me dis qu'il reste encore de la route pour rivaliser avec des librairies comme Spacy qui ont le bon goût d'être en python :-)

### comment tricher avec vos DataViz <a class="anchor" id="dataviz"></a>

Sous un titre volontairement provocateur Romain Sagean nous montre quelques ficelles pour tricher dans nos graphiques.

Il nous montre comment on peut diminuer l'effet visuel d'une baisse dans un line chart en augmentant l'épaisseur du trait ou même en le passant en 3D.

En manipulant un peu les échelles, on peut facilement augmenter l'effet visuel d'une augmentation ou diminuer son effet avec des bars charts.

De manière générale, toujours utiliser les line charts pour montrer une évolution dans le temps et les bar charts pour faire de la comparaison.

Concernant les fameux camemberts, autrement appelés pie charts, les découper en leur centre pour leur donner un effet Donuts est une mauvaise idée même si ça donne un vrai style et une patte de designer. En effet, selon Romain, 25% des gens regardent l'angle du camembert pour visualiser la part d'une catégorie quand les autres regardent la périphérie.

La 3D est toujours à proscrire ! En mettant volontairement une catégorie sous-représentée en haut du camembert, on augmente l'impression de son importance artificiellement.

Les couleurs ont leur importance, un bar chart qui compare des pays avec la France en rose et le Brésil en violet, c'est du grand n'importe quoi.

Et pour finir quelque chose de vraiment basique. Quand on présente un tableau tout bête, le trier sur les labels (ex: le nom des pays) est souvent une mauvaise idée. Il vaut mieux trier sur le chiffre d'affaire du pays dans l'ordre que l'on veut par exemple.
