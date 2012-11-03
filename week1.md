## Découverte

Commençons avec des programmes très simples.

### Quelques images de base

Afficher un cercle de rayon 80px :

	import Graphics.Gloss
	picture = circle 80

Afficher un rectangle de longueur 200px et de largeur 300px :

	import Graphics.Gloss

	picture = rectangleSolid 200 300

Afficher des mots :

	import Graphics.Gloss

	picture = text "Hello"

Tous ces programmes ont certaines choses en commun :
 * Leur première ligne est `import Graphics.Gloss`. Elle indique que vous souhaitez utiliser la bibliothèque Gloss dans votre programme. Vous n'avez besoin de le dire qu'une fois, et vous devez l'écrire au début de vos programmes.
 * `picture` est l'image qui doit être affichée. A la droite de `=`, se trouve l'expression qui dit ce qu'est votre image.
 * Cette expression peut être `circle 80`, `text "Hello"`, ou autre chose. Vous pouvez changer les valeurs `80` du cercle, ou `"Hello"` du texte, ou celles du rectangle.

{Ajouter polygones et lignes}
{Ajouter images}

### Afficher plusieurs choses

Cette fois, nous allons afficher un cercle et un rectangle en même temps.

	import Graphics.Gloss
	
	picture = pictures
		[ circle 80
		, rectangleWire 200 100 ]

L'expression qui définit l'image a cette forme :
 * L'utilisation de `pictures`
 * Un crochet ouvrant
 * Une liste des images que l'on veut afficher, séparées par des virgules
 * Un crochet fermant

On aurait pu écrire le programme de cette façon :

	import Graphics.Gloss

	picture = pictures [circle 80, rectangleWire 200 100]

mais il aurait été moins lisible. On peut séparer une expression sur plusieurs lignes, mais il faut indenter les lignes qui continuent les expressions, c'est-à-dire mettre quelques espaces à leur début, comme c'est le cas avant "[ circle" et ", rectangleWire".

## Changer nos images

La librairie Gloss nous permet de modifier certaines propriétés de nos images, comme la couleur, la position, l'angle et l'échelle.

### `color`

Pour changer la couleur d'une image, on utilise `color`.

	import Graphics.Gloss

	picture = color red (circleSolid 80)

L'expression suit cette forme :
 * `color`
 * le nom de la couleur que l'on veut, par exemple : `red`, `blue`, `magenta`, `chartreuse`, `black`
 * l'image sur laquelle la couleur doit être appliquée, entre parenthèses. Ces parenthèses sont importantes, elles ont la même signification qu'on mathématique : la priorité, autrement dit : "Traite tout ce qui est entre parenthèse comme une seule chose."

### `translate`

On peut également déplacer l'image sur la fenêtre.

	import Graphics.Gloss

	picture = translate 100 50 (rectangleSolid 50 50)

"translate" signifie effectuer une translation, autrement dit déplacer l'image.
`translate` permet de déplacer les images vers le haut ou le bas la droite ou la gauche.

Dans `translate 100 50`, le premier nombre indique que l'image doit être déplacée de 100 pixels vers la droite et le second nombre de 50 pixels vers le haut.
Essayez aussi avec `translate 0 (-45)` : l'image ne se déplace pas horizontalement et se déplace de 45 pixels vers le bas. En Haskell, il faut écrire les nombres négatifs, comme `(-45)` entre parenthèses, sinon Haskell ne sait pas s'il s'agit d'une soustraction, comme `0 - 45` ou d'un nombre négatif.
Le premier nombre indique le déplacement horizontal et le second le vertical. Si l'on écrit `0`, il n'y a aucun déplacement, si on écrit un nombre négatif, ce sera un déplacement vers la gauche pour le premier nombre et un vers le bas pour le second.

La position de départ d'une image est `0` sur l'axe horizontal et `0` sur l'axe vertical, ce qui correspond, dans Gloss, au centre de l'écran.

### `rotate`

On peut aussi tourner les objets, leur faire effectuer une rotation, avec `rotate`.

	import Graphics.Gloss

	picture = rotate 45 (rectangleWire 100  100)

Un losange apparait à l'écran. C'est un carré qui a été tourné de 45 degrés.
`rotate` prend un nombre qui indique la rotation en degrés et l'image à tourner.
Les objets tournent dans le sens trigonométrique, c'est-à-dire inverse des aiguilles d'une montre. {Insérer ici une image d'un cercle avec les angles.} Tourner de 360 degrés revient à tourner de 0 degrés, autrement dit à ne pas le tourner. Tourner de 370 degrés revient à le tourner de 10 degrés. On peut aussi utiliser des nombres négatifs. `rotate (-90)` revient à écrire `rotate 270`.

### `scale`

On peut enfin étirer les images, avec `scale`, dont le nom signifie "échelle", parce-qu'on change les échelles de l'image.

	import Graphics.Gloss

	picture = scale 2 1 (circle 100)

Une ellipse sera affichée, plus large que haute.
Pour `scale`, le premier nombre indique l'échelle horizontale et le second l'échelle verticale. `scale 1 1` ne change pas l'image. Les échelles sont multipliées aux dimensions de l'image.
C'est une déformation : les dimensions de l'image de base et de l'image déformée n'ont pas à être proportionnelles.

## Des images complexes

Il est possible d'utiliser plusieurs de ces transformations en même temps.

### Quelques exemples

Une ellipse orange orientée en diagonale.

	import Graphics.Gloss

	picture = color orange (rotate 45 (scale 2 1 (circle 100)))

L'ordre des transformations a une importance.
Par exemple, on applique `scale` sur l'image avant de de la tourner avec `rotate` dans `rotate 45 (scale 2 1 (circle 100))`. Si on effectue une rotation avant un changement d'échelle `scale 2 1 (rotate 45 (circle 100))`, l'ellispe ne se tourne pas ! En effet, faire tourner un cercle ne change rien, parce-que le cercle a une infinité d'axes de symétrie, alors que faire tourner une ellispe est visible. Essayez d'inverse `scale` et `rotate` pour voir le changement. {Vérifier.}
`translate` peut être utilisé sans différence avant ou après `scale`.
`rotate` a aussi d'autres effets selon son ordre d'application avec `translate`. `rotate` tourne l'image autour de son centre. Si on a utilisé `translate` sur l'image avant de la tourner, ce point de rotation sera aux coordonnées indiquées par `translate`.
Par contre, on peut utiliser `color` à n'importe-quel moment.

{Trouver un autre exemple.
Une image abstraite, par exemple.}

Un visage d'une créature issue d'un autre monde.

	import Graphics.Gloss

	picture = Pictures
		[ color blue (circle 100) -- la tête
		, translate 0 (-10) (scale 0.5 2 (Pictures
			[ translate (-30) 0 (circle 30)
			, translate 30 0 (circle 30)])) -- les yeux
		, translate 0 10 (rectangleSolid 35 2) ]-- la bouche

### A vous de jouer

A vous de de dessiner ce qui vous plaît, que ce soit un poisson, un jardin, une station spatiale, une fusée ou un dragon. Choisissez une image qui utilise le plus d'images et de transformations différentes.
Avant d'écrire le code Haskell, dessiner à la main avec des annotations indiquant le code correspondant vous sera très utile. Par exemple, le dessin d'un personnage en bâtons (ou stickman) peut être réalisé d'après ce schéma :
{faire et insérer le schéma}.
