---
layout: post
title:  "Practical VIM"
description: "Lecture du livre Practical VIM"
date:   2020-02-01 11:00:00
tags: livre
---

TIP 65 
### Bouger et Editer

`:12` ou `12G` -> va a la ligne 12
`h,j,k,l` -> h a gauche, j en bas, k en haut, l a droite 
`0`pour zero permet d'aller au début d'une ligne, `$` pour aller a la fin
`w`pour aller au début du mot suivant, `b`pour aller a début du mot précédent.

`u` pour Undo last action, on peut faire `3u` pour undo 3 last action bien sur. `Ctrl+maj+r` pour faire un redo


`a` Permet de rentrer en *insert mode* mais un charactère après le curseur. En gros, `$`pour aller a la fin + `a` pour insérer.
* `A` éditer la ligne en cours directement a la fin, 
* `I` edite la ligne directement au début,
* `o` ajoute une ligne en insert mode après la ligne courante
* `O` eau majuscule, ligne au dessus en édit mode 



`c` Enter in change mode, rentre en insert mode mais en appliquant la commande de change avant, par exemple `c0` supprime tout du curseur au début de la ligne + entre en insert mode.i
exemple marrant : `c2b`supprime les 2 derniers mots et insert mode, `cw`, `c$`, `c0`...



`dd`supprime la ligne, `dw` supprime le mot après le curseur, `d4w` supprime 4 mots après le curseur, `d3b` supprime 3 mots avant le curseur.
Pour supprimer plusieurs ligne on peut se mettre ou on veut et faire `4dd` pour supprimer 4 lignes ou bien faire `:45,49d` pour supp les lignes de 45 a 49

#### Copier/Coller

`y`= yank pour copier, `yy` copie la ligne entière, `p`copie au dessus du curseur, `P`copie en dessous du curseur
`y3w`copie 3 mots après le curseur, `y5b`

Pour copier les lignes 55 a 59 on peut faire `:55,59y` puis se positioner ou on veut pour copier


`:set nu` pour afficher le numéro des lignes, pour retirer c'est `:set nonu`, on préfixe le nom de l'option par `no`

`.` = redo last action, exemple, `3dw`pour delete 3 word + `.` = redelete 3 words.

### Se déplacer sur l'écran
`Enter` permet d'aller au début de la ligne suivant, `3Enter`-> go to start 3 lignes en dessous

`Ctrl+f` ou `Ctrl+b` un écran en dessous ou au dessus, forward, before

`e` amène a la fin du mot suivant
`zz` met le curseur au milieu de l'écran
`zt` pour le mettre en haut du screen
`zb` pour le mettre en bas

#### Pour faire des recherches : 
`/toto` cherche les occurences de toto après le curseur, tape `n` to seach next occurence
`?toto` cherche occurence toto avant le curseur, `n`= next dans la meme direction, `N` next dans la direction opposées


`f,` aller a la prochaine occurence du character ',' `F,` aller a la précédente occurence du charactére ',', on peut faire `2f_` pour placer au deuxième _ suivant
Pour faire l'équivalent de next `n`on peut utiliser `,` ou `;` pour bouger dans la recherche sur la phrase

Pour savoir a quelle ligne on se trouve : `Ctrl+G` mais on peut aussi faire `:set nu` pour afficher les numéros de lignes

Pour TOTOchanger un pattern sur une ligne courante : `:s/tata/tata/` on peut ajouter un g pour le faire partout dans la ligne
Pour changer dans tout le fichier un pattern par un autre : `:%s/toto/tata/g` 
On peut aussi ajouter un `c` pour dire confirmer a la fin histoire de valider chaque changement : `:%s/tata/toto/gc`

Pour changer plein d'occurence on peut aussi combiner le search `/toto` avec `cw` + insert ESC + `n` pour aller a la prochaine occurence

Pour inserer du text au début d'une selection de ligne : `Ctrl+v`pour entrer en VISUAL BLOCK MODE, avec `hjkl`selectionner le text, hit `Shift+i` pour inserer au début ou `Shift+a`pour la fin, add text + ESC et ça se réplique partout.
On peut aussi faire `:56,60normal Iceci est un commentaire : `

Sur un mot `*` fait un search sur le mot vers le bas, `:set hls` pour hightlight search match, permet de voir tous les matchs d'une recherche
Une fois sur un mot on peut faire `cw` pour changer le mot, puis `n`pour aller a la recherche suivante, puis `.` pour repeter le `cw` 

On peut incrementer un nombre sur une ligne avec `Ctrl+a` et décrémenter avec `Ctrl+x`, pas besoin d'etre sur le digit, il incrémente le premier.
On peut aussi faire `10Ctrl+a` pour augmenter de 10 par exemple :) 

En etant en InsertMode, `Ctrl+h` supprime le caratère précédent, `Ctrl+w` supprime le mot précédent, `Ctrl+u` supprime depuis le début de la ligne
Bonus, ça marche aussi dans le shell !!!!

En étant en insert mode, `Ctrl+o` permet de lancer une seule commande en Normal Mode puis revenir directement en  Insert Mode. Par exemple `Ctrl+o` + `d3b` supprime les 3 derniers mots tout en restant en INsert Mode.
En Normal Mode, `Ctrl+o` permet de revenir a la localisation précédente, 12G (go l 12) puis `Ctrl+o` retour l33 par exemple. 

Avec `R` on rentre en replace mode, ça permet de remplacer du texte. on peut aussi faire `ra` pour remplacer le caractère actuel par un `a`

### Ex commands
Voila le pattern : range(12,11 ou /Mot/,/autreMot/)action[line] par exemple :12,14m10 -> move contenu de ligne 12 a 14 a la ligne 10
`:12,$d` Delete les lignes de la 12 a la fin
`:%d` delete toute les lignes : % = toutes les lignes -> `:%s/toto/tata/g` change toto en tata globaly toute les lignes
`:,$d` delete du curseur a la fin

### Copy or Move with Ex command
`range t location` -> `12,14t32` copy lines from 12 to 14 under line 32, t est un shortcut de copy
On peut faire pareil avec m shortcut de move: `12m.` déplace la ligne 12 au curseur

`:'<,'>m$` -> move la selection fait precedement avec un Visual Mode a la fin

La Ex command `normal` permet de faire une Ex command avec les command du Normal Mode, par exemple pour insérer au début de plusieurs ligne : `:12,16normal I#` va commenter les lignes 12 a 16
`:83,89normal d3w` supprime les 3 premiers mots de ces lignes
On peut enchainer, par exemple : `12,15normal 3w3w` avance de 3 mots et supprime 3 mots

Command Line Window !!! En tapant `q:` on rentre dans un fichier contenant l'historique de ses commandes, on peut bouger dedans comme dans un fichier vim.
En faisant `CR` puis Enter on execute la commande sur laquelle on est. On peut faire des recherches et meme modifier l'historique.

### Jouer des commandes Shell
En faisant `:shell` on ouvre un shell, on tape ses commandes puis `exit` et on retourne dans l'édition du document
On peut jouer une commande shell depuis le Command Line Mode en prefixant par un ! : `:!ls opt*` 
Si on fait `read !ls` on ecrit en dessout du curseur le résultat de la commande ls.

### Splitter l'écran et editer plusieurs fichier
`Ctrl+w s` splitte horizontalement, puis `:ls :b3` pour ouvrir le fichier 3 du buffer list.
Ou bien `:edit filename` 

`Ctrl+w v` splitt l'écran verticalement

Pour se balader entre les écrans : `Ctrl+w h,j,k,l`, on peut ausi faire `Ctrl+w w` en boucle pour tourner de fichier en fichier
`Ctrl+w c` pour fermer une fenetre, `Ctrl+w o` pour fermer toute les fenetres sauf celle sur laquelle on est.

On peut aussi faire des tabs: `:tabe` pour ouvrir une tab, puis `edit filenname` 
`gt` pour aller a la tab suivante, on peut ouvrir plusieurs fenetre dans une tab
Pour fermer une tab : `:tabc` pour tabclose, `:tabo` pour fermer toute les tab sauf l'active
Pour bouger une tab et réarranger l'ordre : `:tabmove 1` ou 0 pour mettre la tab au début

`:lcd /users/Manuel` Permet de change la localisation de relative, tout ce qu'on fera comme `edit` par exemple se basera dessus


On peut faire une recherche avec la commande `:find` mais avant on set le path pour que ça puisse aller dans les sous repertoires : `:set path+=**` puis `:find stop_jfr_weblo.yml` 
La commande find ouvre directement le fichier si elle le trouve

Pour ouvrir un explorateur de fichier : `:Explore` : c'est une vue d'un dossier, on peut rechercher comme un document vim. `p` pour remonter au node racine

On peut supprimer du texte du curseur jusqu'a une recherche : `d/toto` va supprimer du curseur jusqua toto 


### Text Object

Voici un morceau de code html : 

var tpl = [
'<a href="{url}">{title}</a>'
]

Pour faire des selections dedans par balise : `vi}` ça va selectionner l'ensemble a l'intérieur de {} 
`a` pour selectionner a l'exterieur des balises : par exemple `va>` et on peut les chainer : `vi}a"i>` 
Pour selectionner toute la ligne d'un coup sauf les ' on peut faire directement `vi'` ensuite y pour yank ou d pour delete
i pour inner text et a pour all: on peut faire `diw` pour delete inner word

i' -> inside ', it -> inside xml tag, i( a) a} i` tous les symboles
Avec le mode visuel v on voit ce que les text objects font, mais on peut les utliser en normal mode aussi
Par exemple : `yi"` copy ce qui est entre ", `da}` delete tout entre {} 

Text objects marche pas seulement sur les symboles mais aussi sur le texte, par exemple `ciw` change inner word `diw` `yiw`
Marche aussi pour une phrase ou un paragraphe avec s ou p `dis` supprime la phrase.

Mettre des marks a un endroit et y revenir: `m[a-z]` et `'a' par exemple. Donc, `:112`, set a mark : `mc` (on pose le marker c), go to mark c : `'c`

### Registers
On peut copier dans un registre et coller depuis celui ci préciséments.
Pour voir ce qu'on a dans les registres : `:registers` , ou `:reg`
Pour copier un mot ou une phrase dans un registre nommé avec une lettre de l'alphabet : `"ayy` -> copie la phrase dans le registre a, `"aP` -> copie la phrase au dessus du curseur du registre a
`"dyiw` copie le mot courant inner dans le registre d

a,b,c,d... sont des named registers, yank possede sont registre de 0 a 9 automatiquement, si je copie un mot il reste accessible dans le yank register 0 : `"0P` meme si je delete entre temps.

On peut aussi utliser le registre avec les Ex commande, par exemple : `:12,14y c` copie les lignes 12 a 14 dans le registre c. Pour copier `:$put c`, pour copier a la fin du document

Le registre + permet de copier/coller en dehors ou vers vim: `"+yy` copie la ligne vim et elle est accessible dans Chrome
On peut copier en dehors et faire `"+p` 

### Les Macros
foo = 1
bar = 'a'
foobar = foo + bar

En tapant `qa` on débute l'enregistrement d'un macro dans le registre a.
ensuite par exemple `Itoto ` + Esc + `A yo c est toto.` Puis Esc+q pour dire qu'on arrete l'enregistrement.
Sur une ligne on peut ensuite faire `@a` pour refaire la meme chose, ou bien en ex command : `:12,15@a` pour l'appliquer a toute ces lignes

foobar = foo + bar + toto + coco + popo (changer tous les o en a : macro => qaforaq (qa enregistre, fo find o, ra substitute o by a, puis 10@a pour faire 10 fois)

