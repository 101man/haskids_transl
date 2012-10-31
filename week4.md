# Les animations

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
