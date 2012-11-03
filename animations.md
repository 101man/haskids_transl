Maintenant, nous allons passer des images fixes aux images animées.
Comment peut-on animer des images ? Il faut qu'elle change en fonction du temps. Autrement dit, on introduit un paramètre dans notre expression.

Au lieu de :

	picture = {expression}

nous avons :

	animation t = {expression}

Nous utilisons une fonction différente. Dans nos précédents programmes, le programme affichait `picture`, maintenant il affichera `animation t`.

`t` est le paramètre de temps. Il représente le nombre de secondes écoulées depuis que le programme s'est lancé.
Le programme affiche plusieurs fois par secondes l'image résultante de `animation t`.
Par exemple, à son lancement, il va afficher `animation 0`, puis il va vite afficher `animation 0.05`, puis `animation 0.10`, etc. Il va appeler `animation` environ 60 fois par seconde, pour ne pas obtenir une animation saccadée.

Il faut utiliser le paramètre `t` dans l'expression pour que l'image soit animée, parce-que `t` est la seule valeur qui est variable, qui change, dans l'expression.

C'est, en pratique, très simple.

	animation t = scale 5 5 (text (read t))

`read` transforme un nombre en texte.
Vous voyez un nombre très souvent actualisé, qui augmente de 1 toutes les secondes.
Au tout début de l'animation, il s'est passé 0 seconde depuis que l'on a exécuté le programme. Dans le programme, `animation` avec l'argument `0` est appelé : `animation 0`. Dans cette fonction, `t` est remplacé par `0`, et l'expression devient : `scale 5 5 (text (read 0))`, ce qui veut dire `scale 5 5 (text "0")`. Cette image est immédiatement affichée à l'écran.
Un certain temps passe. Après une demi-seconde, animation est appelée avec la valeur `0.5` pour `t` : `animation 0.5`. L'image `scale 5 5 (text (read 0.5))` est affichée.
Une seconde plus tard, le programme fait `animation 1`, ce qui donne `scale 5 5 (text (read 1)), qui est affiché.
Etc.

	animation t = circle t

Vous voyez un cercle dont le rayon grandit de 1px toutes les secondes.

	animation t = circle (5 * t)

Même exemple que précédemment, sauf que le rayon du cercle grandit 5 fois plus vite.

	animation t = rotate (60 * t) (rectangleSolid 100 100)

Un carré qui tourne de 60 degrés par seconde.

	animation t = rotate (100 * t) (rectangleWire 100 100)

Le carré tourne maintenant plus vite. Au début, il n'est pas tourné, parce-que 100 * 0 = 0, mais ensuite, il mettra trois secondes et demi pour faire un tour complet.

	animation t = rotate (100 * t + 45) (rectangleWire 100 100)

Maintenant, le carré commence sa rotation à un angle différent, à 45°, ce qui lui donne une allure de losange. En effet, quand `animation` est appelée avec `0` pour valeur de `t`, l'expression est `rotate 45 (rectangleWire 100 100)`. Ensuite, il tourne à la même vitesse que précédemment.

	animation t = rotate (60 * t) (translate 100 0 (circleSolid 25))

Comme on l'avait vu il y a un certain temps, une rotation autour d'un point autre que le milieu du cercle.

	animation t = translate (50 * t) 0 (circle 25)

Au début, quant `t` vaut `0`, l'image est `translate 0 0 (circle 25)`. Une seconde plus tard, elle est `translate 50 0 (circle 25)`. Au bout d'un certain temps, elle sort de l'écran, bien qu'elle continue à se déplacer.

	animation t = pictures
		[ translate t 0 (rotate (60 * t) (rectangleSolid 36 36)
		, translate 0 t (circle (5 * t))
		]

Une animation un peu longue à décrire qui rassemble plusieurs animations précédentes.

	animation t = if (round t) % 2 == 0
		then color orange
			(translate (-100) (-100)
				(circle 15))
		else color blue
			(translate 100 100
				(rectangleSolid 15 15))

Une animation qui alterne entre un cercle et un rectangle toutes les secondes.
Dans une animation, on n'est donc pas obligé d'effectuer des transitions, c'est-à-dire de faire progressivement évoluer la position ou la taille d'une figure, mais on peut afficher des images totalement différentes.
Ce simple paramètre de temps permet d'obtenir des animations complexes très rapidement avec beaucoup de libertés.

	animation t = pictures
		[ translate (15 * x) 0 (circle 10)
		| x <- [ 0 .. (round t) ]
		]

Une compréhension de liste qui crée une liste d'images dont la longueur est le nombre de secondes écoulées et espacées de 5px.

## Le top-down design

Pour des animations plus complexes, nous allons avoir besoin du  top-down design. Pour rappel, celui-ci consiste à séparer logiquement le code en fonctions dont on écrit les définitions après.
Pour les animations, certaines fonctions auront besoin que le paramètre `t` leur soit donné, alors que d'autres, des images fixes, non.

Voici un programme qui affiche une horloge en respectant ce design.

	import Graphics.Gloss

	animation t = clock t

	clock t = pictures
		[ backPlate
		, minuteHand t
		, secondHand t
		]

	backPlate = color (dark white) (circleSolid 250)

	minuteHand t = rotate (-0.1 * t) hand
	secondHand t = rotate (-6 * t) hand
	hand = line [ (0, 0), (0, 250) ]

 * On définit la fonction `animation` comme une horloge `clock`. On donne le paramètre `t` à `clock`, pour qu'elle puisse s'animer.
 * Dans la définition de `clock`, on trouve donc un paramètre `t`, comme pour animation. L'expression dit qu'une horloge est composée d'un plateau, d'une aiguille des secondes et d'une des minutes. Comme le plateau ne bouge pas, on ne lui passe pas l'argument `t`, mais les aiguilles bougent, donc on le leur passe.
 * Le plateau, `backPlate`, est défini comme une image fixe.
 * `minuteHand` a besoin d'un paramètre `t` pour tourner en fonction de l'heure. On veut que l'aiguille tourne de 360° chaque heure, et une heure = 3600 secondes. La seconde est l'unité de `t`. 3600 / 360 = 10, donc on doit tourner d'un dixième de degré l'aiguille chaque seconde. `rotate` fait tourner les images dans le sens inverse des aiguilles d'une montre, donc il faut utiliser un nombre négatif pour corriger ça.
 * De même pour `secondHand`, sauf que cette fois, on veut que l'aiguille tourne de 360° chaque minute, et une minute = 60 secondes. 360 / 60 = 6, donc l'aiguille doit tourner de 6 degrés par seconde. Le nombre est là aussi négatif.

Remarquez aussi qu'on peut remplacer `animation t = clock t` par :

	animation t = secondHand t

afin de tester individuellement les fonctions, comme le permet le top-down design.

## Pratique

Reproduire un système solaire simplifié, avec la rotation des planètes.

Le réalisme n'est pas très important :
 * Leur taille relative doit être respectée, c'est-à-dire que Jupiter est un peu plus grande que Saturne, que Mercure est un peu plus petite que la Terre, etc., mais le soleil peut être plus petit qu'en taille réelle, par exemple 2 fois plus grand que Jupiter.
 * Les distances n'ont pas à être conservées, parce-que les planètes sont très éloignées, donc vous pouvez les rapprocher.
 * Utilisez des rotations sont circulaires, parce-qu'on ne sait pas en faire d'autre, alors que les rotations réelles des planètes sont ellipsoidales. Conservez l'ordre des astres : {...}.

Vous pouvez donner des couleurs aux planètes, par exemple : 
Vous pouvez ajouter des satellites, comme la Lune, et donner des anneaux à Saturne.

Si vous le souhaitez, vous pouvez même créer votre propre système solaire avec vos noms et vos astres.


##

Le problème est que dans la plupart de nos animations, après un certain temps passé, le nombre `t` devient trop grand et les objets deviennent trop gros, sortent de l'écran, etc., sauf avec `rotate`.

Afin de régler ce problème, nous allons créer des fonctions qui vont prendre `t` en paramètre et renvoyer un autre nombre à utiliser à la place.

Exemple 1

	animation t = translate (15 * t) 0 (circle 15)

Au bout de quelques secondes, le cercle sort de l'écran, parce-que sa position dépend de `t` et que `t` est trop grand.
On peut donc, par exemple, établir une limite à `t`.

	animation t = translate (15 * (limite t)) 0 (circle 15)

	limite x = if x > 5
		then 5
		else x

## Dessiner des fonctions !

Aux alentours du début du lycée, le dessin de fonction est pris en charge dans les cours de math.

Les fonctions Gloss que l'on utilise prennent à 99% des nombres en paramètre. `circle`, `translate`, etc.
Se familiariser avec des fonctions qui retournent des nombres nous sera donc très utile.

![](http://cdsmith.files.wordpress.com/2011/10/graph.png)
![](http://cdsmith.files.wordpress.com/2011/10/graph1.png)

Code :

	f t = t / 2 - 2

Etapes :

	f  0 =  0/2 - 2 = 0.0 - 2 = -2.0
	f  1 =  1/2 - 2 = 0.5 - 2 = -1.5
	f  2 =  2/2 - 2 = 1.0 - 2 = -1.0
	f  3 =  3/2 - 2 = 1.5 - 2 = -0.5
	f  4 =  4/2 - 2 = 2.0 - 2 =  0.0
	f  5 =  5/2 - 2 = 2.5 - 2 =  0.5
	f  6 =  6/2 - 2 = 3.0 - 2 =  1.0
	f  7 =  7/2 - 2 = 3.5 - 2 =  1.5
	f  8 =  8/2 - 2 = 4.0 - 2 =  2.0
	f  9 =  9/2 - 2 = 4.5 - 2 =  2.5
	f 10 = 10/2 - 2 = 5.0 - 2 =  3.0
	f 11 = 11/2 - 2 = 5.5 - 2 =  3.5
	f 12 = 12/2 - 2 = 6.0 - 2 =  4.0
	f 13 = 13/2 - 2 = 6.5 - 2 =  4.5

![](http://cdsmith.files.wordpress.com/2011/10/graph2.png)

	import Graphics.Gloss

	animation t = drawFunction f t

	drawFunction f x = translate x (f x) point

	point = rectangleSolid 1 1

Il existe une manière très pratique de dessiner une fonction qui prend un nombre en paramètre et en retourne un autre. En fait, c'est une courbe qui est dessinée. {Inventée par Descartes ?}

C'est un repère. L'axe des abscisses représente l'entrée de sa fonction, son argument, et l'axe des ordonnées représente sa sortie, son résultat, son retour.

On numérote les axes en inscrivant des nombres sur leurs traits. L'origine du repère est le point qui a pour coordonnées `(0;0)`. Les nombres peuven être négatifs pour `y`, et pourraient l'être pour `x`, mais dans notre cas, `x = t` et `t` représente le temps, donc pas possible qu'il soit négatif.

Pour chaque couple entrée/sortie, on place un point qui a une abscisse perpendiculaire blabla.
Autrement dit, si on affiche la courbe de `f` et que

	f 1 = 2

alors on peut positionner notre premier point :

	f 0 = ...

{Utiliser des couleurs sur le graphe et le code pour reconnaître les x et y.}

`f x = y`

Comment lire le graphique.

En observant le graphique, on peut bien comprendre ce que fait la fonction. Dans le cas de {la fonction affine}, on sait que plus l'entrée sera grande, plus la sortie le sera, donc plus on avancera en temps dans l'animation, plus le nombre renvoyé par `t` sera grand.

So what does that mean?
If you say rotate (t/2-2), that means start out rotated a little bit clockwise, and then turn counter-clockwise at a constant speed forever.
If you say translate (t/2-2) 0, that means start out a little to the left, then move right at a constant speed forever.
and so on…


D'abord, on fait à la main. Ensuite on affiche la fonction à l'ordi sans repère. Puis on crée un repère. Puis on met les deux ensemble.

Dessiner un repère avec Gloss :



Fonction d'ordre supérieur.

Voir la progression d'une animation au fur et à mesure et ses étapes précédentes :
Voir l'animation proprement, centrée, et les autres étapes

Voir tout d'un coup avec une compréhension de liste et picture.

Ces fonctions peuvent être utilisées sur n'importe-quelle animation !
Ce sont des outils, en quelque sorte.

Quelques fonctions mathématiques utiles :
 - min et max : elles prennent deux nombres en paramètre, et retourne un des deux nombres que vous leur avez donné. `min` retourne le plus petit des deux et `max` le plus grand des deux.
 - sin : Des courbes symétriques qui vont de 1 à -1. ![](http://cdsmith.files.wordpress.com/2011/10/sine.png?w=595). Trigonométrie.

## Pratique

Dessinez à la main avant de tester le programme :

	f t = 0

	f t = 5

	f t = t

	f t = 3 * t

	f t = t + 5

	f t = 3 * t + 5

	f t = t * t

	f t = (-4) * t * t + 2 * t + 8

	f t = min t 20

	f t = max t 0

	f t = sin t

	f t = 200 * sin t

	f t = 200 * sin (0.1 * t)

	Un exemple plus complexe :

	f t = max 0 (100 * sin (0.1 * t – 15)) + 25

## Pratique

Un parc d'attraction animé.
Cherchez des mouvements variés : des rotations, un mouvement d'avant en arrière, etc.

 * Les montagnes russes
 * Le saut saut à l'élastique
 * La grande roue 
 * Manège
 * Le vaisseau pirate

Le top-down design permet de travailler sur chacun de ces éléments et de les regrouper ensuite dans une grande fonction `amusementParc`.
Vous pouvez même vous répartir les tâches entre vous et réunir vos codes en un seul fichier à la fin.

##

	import Graphics.Gloss

	animation t = pictures [
		jumprope t,
		translate (-210) 0 stickFigure,
		translate ( 210) 0 stickFigure,
		jumper t
		]

	jumprope t = scale 1.75 (1.1 * sin t) rope

	rope = line [ (x, 100 * cos (0.015 * x)) | x <- [-100, -75 .. 100 ] ]

	stickFigure = pictures [
		line [ (-35, -100), (0, -25) ],
		line [ ( 35, -100), (0, -25) ],
		line [ (  0,  -25), (0,  50) ],
		line [ (-35,    0), (0,  40) ],
		line [ ( 35,    0), (0,  40) ],
		translate 0 75 (circle 25)
		]

	jumper t = translate 0 (max 0 (-50 * sin t)) stickFigure

## Des images

Faire suivre des trajectoires :

##

Intérêt de savoir dessiner des images, puis des animations ? Peut servir pour des présentations, pour tester des formules mathématiques ou physiques, etc. Mais pour vrais figures complexes : Inkscape, et pour films d'animation : ...
Surtout se préparer à la création de jeux.
