Commenter dessiner un éléphant cool ?

Voici un éléphant banal :

	elephant = pictures
		[ rotate (-20) (translate 30 40 body)
		, translate 150 (-20) trunk
		, translate (-10) 40 head
		, translate 50 (-50) leg
		, translate (-60) (-50) leg
		, translate (-140) 50 (rotate 100 tail)
		]
		where
			head = scale 1.5 1 (circleSolid 80)
			trunk = rectangleSolid 40 80
			body = scale 3 2 (circleSolid 25)
			leg = rectangleSolid 40 70
			tail = rectangleSolid 10 40

![Eléphant banal](http://cdsmith.files.wordpress.com/2011/09/elephant.png?w=300&h=300)

## La fonction `awesome`

Être cool signifie être deux fois plus grand et de couleur chartreuse.

awesomeElephant = scale 2 2 (color chartreuse elephant)

![Eléphant cool](http://cdsmith.files.wordpress.com/2011/09/aelephant.png)

Si on veut afficher une étoile cool...

star = polygon
	[ (-75, -100)
	, (0, 87)
	, (75, -100)
	, (-100, 25)
	, (100, 25)
	, (-75, -100)
	]

awesomeStar = scale 2 2 (color chartreuse star)

On sait donc dessiner une version cool de n'importe-quelle image. Des vaisseaux spaciaux cool, des vélos cool, des chaussures cool...
Au lieu de se répéter, créons une fonction `awesome` qui rend cool tout ce qu'on lui donne.
awesome x = scale 2 2 (color chartreuse x)
Maintenant, plus besoin de définir des fonctions comme `awesomeStar` ou `awesomeElephant`, il suffit d'écrire `awesome star` et `awesome elephant`.

cute x = scale 0.5 0.5 (color purple x)

## Une queue adaptable

La queue d'un éléphant est très caractéristique de son état. Il serait bien de pouvoir dessiner des éléphants avec des queues différentes. Créons une fonction avec un paramètre `angle` qui est l'angle de rotation de la queue. Celui-ci remplace le `100`.

	elephant angle = pictures
		[ rotate (-20) (translate 30 40 body)
		, translate 150 (-20) trunk
		, translate (-10) 40 head
		, translate 50 (-50) leg
		, translate (-60) (-50) leg
		, translate (-140) 50 (rotate angle tail)
		]
		where
			head = scale 1.5 1 (circleSolid 80)
			trunk = rectangleSolid 40 80
			body = scale 3 2 (circleSolid 25)
			leg = rectangleSolid 40 70
			tail = rectangleSolid 10 40

Elephant normal :

	elephant 160

![Elephant normal](http://cdsmith.files.wordpress.com/2011/09/elephant160.png)

Elephant endormi :

	elephant 180

![Elephant endormi]() {ajouter}

Elephant effrayé :

	elephant 0

![Elephant effrayé](http://cdsmith.files.wordpress.com/2011/09/elephant0.png)

## Listes

Une liste contient des éléments du type de notre choix séparés par des crochets et des virgules.
On a travaillé avec des listes d'images :

	pictures [ circle 10, rectangleSolid 10 20, text "Hello" ]

et des listes de points :

	polygon [ (0, 0), (0, 10), (50, 0) ]

Il existe aussi des listes de nombres, comme :

	[ 1, 2, 3, 4, 5 ]

Nous allons apprendre à utiliser quelques fonctionnalités des listes.

Une autre manière d'écrire `[ 1, 2, 3, 4, 5 ]` est :

	[ 1 .. 5 ]

Et `[ 2 .. 10 ]` équivaut à :

	[ 2, 3, 4, 5, 6, 7, 8, 9, 10 ]

On écrit entre crochets le nombre de départ, suivi de `..`, suivi du nombre d'arrivée, qui produit une liste comprenant ces deux limites et les nombres intermédiaires.

Cette notation est très utile parce-qu'elle automatise la tâche de générer les nombres intermédiaires, mais elle est parfois limitée. En effet, elle crée tous les nombres entiers intermédiaires, mais comment peut-on faire quand on veut avancer de 2 en 2, par exemple ?

	[ 2, 4 .. 10 ]

Cette notation équivaut à :

	[ 2, 4, 6, 8, 10 ]

Elle ressemble beaucoup à la précédente, mis à part qu'on renseigne les deux premiers éléments de la liste, et leur différence est utilisée pour chaque nouvel élément ajouté jusqu'au dernier.

	[ 1 .. 5]

équivaut à :

	[ 1, 2 .. 5 ]

Enfin, voyons la dernière notation qui est la plus intéressante : la compréhension de liste.

	[ x + 7 | x <- [ 1 .. 5 ] ]

Analysons précisément cette expression.
 * Elle commence par un crochet ouvrant, comme dans une liste.
 * Ensuite, il y a une expression : `x + 7`, qui contient une fonction `x` que l'on a pas encore définie.
 * Au milieu, on trouve une barre verticale `|`, qui semble séparer l'expression précédente de ce qui suit.
 * On est face à une sorte de définition de x : `x <- [ 1 .. 5 ]`, mis à part que le `=` est remplacé par `<-` et que ce qui se trouve à droite est une liste : `[ 1 .. 5 ]`.
 * Elle se termine avec un crochet fermant, comme dans une liste.

Cet exemple peut être traduit par : "Donne-moi une liste de toutes les valeurs de `x + 7`, ou {accent} `x` contient successivement les valeurs de la liste `[ 1 .. 5 ]`.

	[ x + 7 | x <- [ 1 .. 5 ] ]
	[ x + 7 | x <- [ 1, 2, 3, 4, 5 ] ]
	[ 1 + 7, 2 + 7, 3 + 7, 4 + 7, 5 + 7 ]
	[ 8, 9, 10, 11, 12 ]

Avec quelques exemples supplémentaires, cette notation vous paraitra simple.

	[ x | x <- [ 1, 2, 3 ] ]
	[ 1, 2, 3]

	[ x * 2 | x <- [ 4 .. 6 ]
	[ 16, 25, 36 ]

	[ (x, -x) | x <- [ -3 .. 1 ] ]
	[ (-3, 3), (-2, 2), (-1, 1), (0, 0), (1, -1) ]

	[ [ 0 .. x ] | x <- [ 1 .. 5 ]
	[ [ 0, 1 ], [ 0, 1, 2 ], [ 0, 1, 2, 3 ], [ 0, 1, 2, 3, 4 ], [ 0, 1, 2, 3, 4, 5 ] ]

Vous remarquez qu'on peut obtenir des listes de nombres, ou des listes de listes.
On pourrait également utiliser les compréhensions de listes avec des images...

## Combiner compréhensions de listes et images

Avant de regarder les images que ces codes produisent, essayez de les deviner et de les esquisser sur un papier.

{ Ajouter les images. }

picture = pictures
	[ circle x
	| x <- [ 10, 20 .. 100 ]
	]

picture = pictures
	[ translate x 0 (circle 20)
	| x <- [ 0, 10 .. 100 ]
	]

picture = pictures
	[ translate x 0 (circle x)
	| x <- [ 10, 20 .. 100 ]
	]

picture = pictures
	[ rotate x (rectangleWire 300 300)
	| x <- [ 0, 10 .. 90 ]
	]

picture = pictures
	[ translate x 0 (rotate x (rectangleWire 300 300))
	| x <- [ -45, -35 .. 45 ]

picture = pictures
	[ translate x y (circle 10)
	| x <- [ -200, -100 .. 200 ]
	, y <- [ -200, -100 .. 200 ]
	]

picture = pictures
	[ rotate angle (translate x 0 (circle 10))
	| x <- [50, 100 .. 200 ]
	, angle <- [ 0,  45 .. 360 ]
	]

picture = pictures
	[
	rotate angle (translate (5*x) 0 (circle x))
	| x <- [10, 20 ..  40 ]
	, angle <- [ 0, 45 .. 360 ]
	]

atom = pictures
	[ rotate t (scale 2 1 (circle 50))
	| t <- [0, 30 .. 360 ]
	]


Les listes de compréhension automatisent donc des tâches très laborieuses.

## Pratique

Cherchez une image très répétitive qui peut être dessinée avec des compréhensions de listes.
Par exemple, le drapeau des Etats-Unis :
![Drapeau des Etats-Unis](http://cdsmith.files.wordpress.com/2011/09/flag.png)
ou un clavier de téléphone :
{ Image. }
un clavier de piano :
![](http://cdsmith.files.wordpress.com/2011/09/keyboard.png)

## Transformer une image en code

![](http://cdsmith.files.wordpress.com/2011/09/picture1.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture2.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture3.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture4.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture5.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture6.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture7.png)

![](http://cdsmith.files.wordpress.com/2011/09/picture8.png)
