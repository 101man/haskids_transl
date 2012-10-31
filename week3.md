### Lignes et polygônes

Jusqu'à présent, nous avons essentiellement dessiné des rectangles et des cercles. Or, il existe bien d'autres formes que nous ne pouvons représenter actuellement, même en utilisant des `scale` et des `Pictures`, comme une étoile.

### Les lignes

Nous allons commencer par dessiner des lignes.
Dans Gloss, une ligne est une suite de segments qui se succèdent. Ce n'est pas une droite ou un segment.

Voici quelques exemples de lignes et le code qui leur correspond.
Essayez de deviner les images que les codes produisent avant de les exécuter.

	import Graphics.Gloss
	picture = line [ (-100, -100), (-200, 50) ]

{ici image}

	picture = line
		[ (-200, -200)
		, ( 200, -200)
		, ( 200,  200)
		, (-200,  200)
		, (-200, -200)
		]

{trouver lignes sympa, comme un électrocardiogramme}

{Quand on met 0 élément ? 1 ?}

Voici une image, tirée de [Berkeley Science](http://www.berkeleyscience.com/relativity.htm), qui présente un repère avec des points et leurs coordonnées.
![Coordonnées cartésiennes](http://cdsmith.files.wordpress.com/2011/08/coordinates.jpg)
Prenons par exemple le point rouge. Il a pour coordonnées (-3,1). -3 est sa position horizontalle et 1 est sa position verticale. La position horizontalle s'appelle la coordonnée x et la position verticale la coordonnée y, parce-que ces lettres sont plus courtes.
Ces coordonnées sont appelées cartésiennes, parce-que Descartes est à l'origine de la géométrie avec repères {développer}

### Les polygônes

#### Une figure fermée

Les polygônes ressemblent beaucoup aux lignes.
Dans une ligne, des segments sont tracés entre les points se succédant dans la liste.
Dans un polygône, un segment supplémentaire est tracé entre le premier et le dernier point de la liste afin de fermer la figure.
Le mot "Polygône" est composée de "poly", qui signifie"plusieurs" en grec, et "gône", qui signifie "côté".

Voici comment dessiner un triangle avec `line` :
{code}
et avec `polygon` :
{code}
Dans la liste de `line`, le premier et le dernier point sont les mêmes : `(`, alors que dans `polygon`, le premier point n'apparaît qu'au début. {reprendre le code en coloriant les tuples des points pour les reconnaître}

	import Graphics.Gloss
	picture = polygon
		[ (-200, -200)
		, ( 200, -200)
		, ( 200,  200)
		, (-200,  200)
		]

Voici la figure annoncée dans l'introduction de cette partie.

	import Graphics.Gloss

	picture = stars

	stars = pictures [
		translate 100 100 (color blue star),
		translate (-100) (-100) (color yellow star),
		translate 0 0 (color red star)
		]

	star = polygon
		[ (0, 50)
		, (10, 20)
		, (40, 20)
		, (20, 0)
		, (30, -30)
		, (0, -10)
		, (-30, -30)
		, (-20, 0)
		, (-40, 20)
		, (-10, 20)
		, (0, 50)
		]

![Image du programme : étoiles](http://cdsmith.files.wordpress.com/2011/08/stars.png)

Les polygônes sont donc en quelque sorte des lignes spécialisées.

{Quand on met 0 élément ? 1 ?}

#### La couleur

Les polygônes diffèrent aussi des lignes par la couleur. Appliquer une couleur sur une ligne coloriera la ligne et appliquer une couleur sur un polygône remplira sa surface et non pas son contour.

Exemple d'un carré colorié en rouge :
 * avec `line` :
 {code + image}
 * avec `polygon` :
 {code + image}

#### Combinaison des deux

Vous pouvez utiliser les deux si vous voulez que votre figure ait une couleur de bordure et une de remplissage.

Cependant, c'est très répétitif, ce qui ne doit pas être le cas en programmation.
Voici une fonction qui automatise cette opération et que vous pouvez coller à la fin de votre fichier pour l'utiliser.

	polygonLine (x:xs:[]) stroke fill = Pictures
		[ color stroke (line (x:xs:x:[]))
		, color fill (polygon (x:xs:[]))
		]

Le premier paramètre est la liste des sommets du polygône, le second la couleur de ses côtés et le dernier la couleur de sa surface.

Remplacez l'ancienne expression, la répétitive, par `polygonLine {} violet red`.

### Les fonctions

`rectangleSolid` utilise la fonction `polygon`.

## Penser comme un ordinateur

Nous allons prétendre être un compilateur. De cette façon, nous pouvons détecter les erreurs et prévoir avec précision comment le programme se comportera.

Nous allons "compiler" le programme suivant :

	import Graphics.Gloss

	picture = stars

	stars = pictures [
		translate 100 100 (color blue star),
		translate (-100) (-100) (color yellow star),
		translate 0 0 (color red star)
		]

	star = polygon
		[ (0, 50)
		, (10, 20)
		, (40, 20)
		, (20, 0)
		, (30, -30)
		, (0, -10)
		, (-30, -30)
		, (-20, 0)
		, (-40, 20)
		, (-10, 20)
		, (0, 50)
		]

 * Je commence à lire le programme du haut.
 Je vois un import, et je retiens que ce programme utiliser le module `Graphics.Gloss`. Je vais voir les définitions que ce module contient, comme `circle`, `polygon`, `rotate`, `green`, et je sais maintenant ce que ces mots veulent dire.
 * Comme mon but dans la vie est de dessiner la fonction `picture`, je cherche sa définition et j'essaie de la comprendre
 * Oups, je ne sais pas ce que `myFavoriteColor` veut dire ; cette fonction n'est pas dans mon dictionnaire ! Ce n'est pas un problème : je cherche dans le programme sa définition, et je vois qu'elle veut dire `blue`.
 * Maintenant, je connais tout ce dont j'ai besoin pour définir `picture` ; j'ai donc fini de lire le programme. J'ouvre une fenêtre et je dessine un cercle bleu.

Vous vous demandez peut-être ce qu'il advient de `stars` et de `star`. Elles sont bien définies dans le fichier, mais elles ne sont pas utilisées pour définir `picture`. La simple existence d'un mot dans un dictionnaire ne signifie pas que vous en avez besoin. Le compilateur se préoccupe seulement de `picture`, et s'il ne voit pas mention de `stars` ou `star` dans sa définition ou dans la définition de `myFavoriteColor`, il ne lit pas la leur, parce-que c'est inutile.

Ce comportement fait partie de l'évaluation paresseuse, ou "lazy evaluation", spécifique à Haskell.

## Exercice

Vous allez essayer de prédire le résultat de vos programmes.

Sur une feuille de papier millimettré, dessinez un point au centre, qui correspond aux coordonnées (0,0), et interprétez les programmes suivants comme un compilateur puis dessinez-les :

	import Graphics.Gloss
	picture = rotate 45 (rectangleWire 150 150)

	import Graphics.Gloss
	picture = rotate 135 (pictures
		[ circle 50
		, rectangleWire 10 100
		])

	import Graphics.Gloss
	picture = translate 50 0 (rotate 45 triangle)
	triangle = line [ (0,0), (100,200), (-100,200), (0,0) ]

	import Graphics.Gloss
	picture = rotate 90 (translate 100 0 (circle 100))
