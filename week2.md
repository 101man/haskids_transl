# Séance 2

## Cours

{séparer la syntaxe et les bonnes habitudes ou les réunir ?}

## La syntaxe Haskell

{ramener module ici et how to think like a compiler}

Nous avons appris à utiliser quelques fonctionnalités de Gloss, mais nous n'avons nommé que quelques termes informatiques, comme "expression". Maintenant que vous savez un peu écrire, nous allons apprendre un peu de syntaxe du langage Haskell.

### Fonction et argument

Prenons l'exemple de :

	picture = translate 10 20 (circleSolid 50)

et analysons ce qui se trouve à droite du "=".

Comment s'appellent tous les éléments qui composent cette expression ?
 * `translate` est une fonction
 * `10` est un argument
 * `20` est un argument
 * `(circleSolid 50)` est un argument
 
Les fonctions sont essentielles en information, parce-que ce sont elles qui font les calculs.
Elle prend donc des arguments et donne un résultat qui varie en fonction des arguments.
Dans `1 + 4`, `+` est une fonction, `1` et `4` sont ces arguments et `5` est son résultat, ou retour.

Les fonctions possèdent un certain nombre de paramètres qui ont une certaine nature qu'il faut respecter.
`translate` en attend trois : un nombre, un autre nombre, et une image. Elle utilise les deux premiers pour donner une position au troisième.

Ce troisième argument est, pour rappel, `(circleSolid 50)`. On sait qu'il est de type image. En fait, `circleSolid` est une fonction qui prend un argument de type nombre, ici `50` et qui retourne une image.

polygon, fonction avec arguments

{Paramètre et arguments}

Listes, points, nombres. Listes de caractères.
You might have noticed that not all the forms of expressions give you pictures!  Some of them give you numbers, or points, or lists.  It’s up to you to make sure that you’re putting the right type of expression in the right places.  If you try to define picture to be the number 7, for example, it won’t work!  That’s because 7 isn’t a picture… it’s just a number.

Séparer fonctions et arguments avec des espaces.

substitution

On utilise beaucoup de fonctions qui retournent des images, mais pas seulement.
`light` et `dark` prennent et retournent des couleurs.

![Schéma d'une fonction](http://cdsmith.files.wordpress.com/2011/10/function.png?w=595)

### Expression

Une expression est une suite d'applications de fonctions. Les expressions composent les programmes.

Comment Haskell identifie et traite-t-il les expressions ?

Prenons un exemple simple :

	1 + (5 * (10 / 2))

Voyons les redex, c'est-à-dire les étapes de réduction d'une expression

	1 + (5 * (10 / 2)) -- une expression complexe avec une sous-expression qui contient une sous-expression
	1 + (5 * 5) -- une expression avec une sous-expression
	1 + 25 -- une expression
	26 -- une expression totalement réduite

Dans l'exemple de tout-à-l'heure :

	picture = translate 10 20 (circleSolid 50)

`circleSolid 50` est une expression et la juxtaposition de `translate 10 20` et de cette expression réduite est une expression.

That’s normally how things go: you do a lot of building up expressions in computer programming.

### Définition de fonction

Une définition attribue un nom à une expression. Ainsi, l'utilisation du nom et de l'expression sont équivalentes.
A definition tells the computer what a word, called a function, means.
C'est exactement le même principe que les définitions de dictionnaire de langues parlées.
Voyons un exemple.

	ring = color yellow (thickCircle 3 15)

Nous avons indiqué à Haskell que `ring` et que `color yellow (thickCircle 3 15)` étaient les mêmes expressions. Ainsi, nous pouvons utiliser les deux de la même façon dans une autre expression, elles sont interchangeables. Créons une image qui représente deux anneaux côte à côte.

	picture = Pictures
		[ translate (-15) 0 ring
		, translate 15 0 ring ]

est la même chose que

	picture = Pictures
		[ translate (-15) 0 (color yellow (thickCircle 3 15))
		, translate 15 0 (color yellow (thickCircle 3 15)) ]

Mais le dernier exemple est moins pratique à lire et à écrire : les lignes de codes sont plus longues, il commence à y avoir beaucoup de parenthèses, le mot "ring" est plus explicite que son expression, que l'on doit lire en intégralité pour comprendre ce qu'elle décrit, etc.

La définition permet donc de réutiliser du code. C'est une forme d'automatisation.

Il ne peut y avoir plusieurs définitions pour un même mot.

	ring = color yellow (thickCircle 3 15)
	ring = circle 15

n'est pas un code Haskell valide. `ring` ne peut être associé qu'à une seule expression, et ceci dans tout votre code. Cette restriction permet d'éviter les bogues, parce-qu'on est sûr de l'expression qu'on utilise.
Par contre, il peut y avoir plusieurs noms associés à une expression, mais c'est stupide.

	ring = color yellow (thickCircle 3 15)
	angelAura = color yellow (thickCircle 3 15)

A la limite, on peut écrire

	ring = color yellow (thickCircle 3 15)
	angelAura = ring

Si vous n'êtes pas satisfait de l'appelation des fonctions :

	move = translate

ou que vous êtes un adepte de la françisation de la programmation

	deplacer = translate

Defining a new word for it might help you in the short term to write your program, but it won’t help at all when you need to read someone else’s program!

Un exemple encore plus parlant : les anneaux olympiques (simplifiés).

	ring = thickcircle 5 30
	upperRing x = translate x 30 ring
	lowerRing x = translate x 0 ring

	olympicGames = Pictures
		[ color blue (upperRing (-66))
		, color yellow (lowerRing (-33))
		, ring
		, color green (upperRing 66)
		, color red (lowerRing 33) ]

est bien mieux que :

	olympicGames = Pictures
		[ color blue (translate (-66) 30 (thickcircle 5 30))
		, color yellow (translate (-33) 0 (thickcircle 5 30))
		, thickcircle 5 30
		, color green (translate 66 30 (thickcircle 5 30))
		, color red (translate 33 0 (thickcircle 5 30)) ]

La syntaxe d'une définition est la suivante :

	fonction = expression

On voit que le mot à qui est assigné l'expression s'appelle en fait une fonction. Ainsi, `ring` et `picture` sont des fonctions sans argument.

Capitalization:  Whether words are capitalized or not matters!  So, for example, red works fine as a color, but Red doesn’t work! 

There, the function is cute and the argument is elephant, so to figure out what that means you just replace x with elephant every time it shows up in the function definition for cute!

{exemple : awesome et cute}

### L'import de module

Un import permet d'utiliser un module dans notre code en indiquant au compilateur Haskell quel est son nom afin qu'il nous donne accès à ses fonctions.

Un module est un fichier Haskell ...
Simple structuration logique.
Modules have two kinds of things in them that we’ve seen so far:
Imports
Definitions

import Graphics.Gloss
circle 80

If you’ve tried that, you know it doesn’t work!  You can’t throw in a description of the circle just anywhere in your program.  The error you’d get for that would say “Naked expression at top level”.  When you see that, it means you forgot to say what variable you are defining!


So far, you can think of module as a word that means the entire program.  (We talked a little bit about how programs can have more than one module, too.  In fact, most programs in the world do!  But it will be a while before we’ll really need to separate our programs into more than one module.)

Les bibliothèques contiennent des modules.

Les bibliothèques sont rangés dans des catégories pour qu'on puisse les découvrir facilement selon notre besoin. Il existe par exemple la catégorie "Graphics", regroupant les bibliothèques offrant des capacités graphiques, ou la catégorie "Data", regroupant les structures de données comme les nombres, les listes, les caractères.
Beaucoup de bibliothèques sont importées par défaut, comme `Data.List`, qui vous permet d'utiliser des listes.

La syntaxe de base est `import categorie.bibliothèque`, comme dans :

	import Graphics.Gloss

Séparation en fichiers puis en bibliothèques.

## Organiser son code

Vous devez être très perfectionnistes et attentionnés vis-à-vis de votre code. Sinon, vous ne pourrez pas vous y retrouver et ne pourrez pas écrire de programmes intéressants.

Voici un exemple de ce que vous ne devez pas écrire :

	import Graphics.Gloss
	picture=Pictures[translate 71 123 (rectangleSolid 30 200), rotate 45 (translate 10 1 (color green (circle 50))), translate 100 100 (scale 2 2 (rectangleWire 10 10))]

Prenons l'exemple d'un bonhomme de neige.

	import Graphics.Gloss

	picture = snowman

	snowman = pictures [
		translate 0 ( 50) top,
		translate 0 (  0) middle,
		translate 0 (-75) bottom
		]

	top    = circleSolid 50
	middle = circleSolid 70
	bottom = circleSolid 90

Pour tester quelques fonctions ou s'amuser, ce n'est pas un problème.

Nous vous conseillons fortement de suivre les principes d'organisation suivants.

{Traduire les citations}

Martin Fowler, {qui}, a dit :
> Any fool can write programs that a computer can understand.  Good programmers write code that humans can understand.

Paul Graham, {qui}, a dit :
> Great software, likewise, requires a fanatical devotion to beauty.  If you look inside good software, you find that the parts no one is ever supposed to see are beautiful, too.  I’m not claiming I write great software, but I know that when it comes to code, I behave in a way that would make me eligible for prescription drugs if I approached everyday life the same way.  It drives me crazy to see code that’s badly indented, or that uses ugly variable names.

C.A.R. Hoare a dit :
> There are two ways of writing computer programs: One way is to make it so simple that there are obviously no mistakes.  The other way is to make it so complicated that there are no obvious mistakes.

Cette dernière citation n'est pas l'exacte traduction de l'originale qui utilisait des termes un peu techniques.

### Une syntaxe lisible

 * Séparez les fonctions et les arguments par des espaces,
 * ne dépassez pas 80 caractères sur chaque ligne,
 * indentez d'au moins 4 caractères,
 * alignez les expressions lorsque vous les mettez à la ligne

Espaces obligatoires ou pas selon situations.

You are not allowed to indent the first line of a definition.  You must indent every line after that.  These are rules in the Haskell programming language; if you don’t follow them, your programs won’t work.  Also, if you use let and where, then you have to indent all the definitions there by the same amount.
Pour aligner, plusieurs espaces autorisés.

You have to match up your opening and closing parentheses, or your program just won’t work.We talked about a few things that help.  Some programming tools, including the web site we’re using right now, will help you match up parentheses by pointing them out.  Using the web site, try putting your cursor right after a parenthesis: the matching one will have a gray box drawn around it!  This can help a lot… but it’s only there if the tool you’re using happens to do it.We also talked about counting parentheses.  There’s a trick most programmers figure out for checking their parentheses: start out with the number 0 in your head, and then read through part of your program, and add one every time you see an open parenthesis, and subtract one every time you see a close parenthesis.  If everything matches, you’ll end up back at zero at the end!  If not, you may need to figure out where you’re missing a parenthesis.

Putting one expression a line and indenting can help see where the parentheses belong to.

We talked about the convention of capitalizing new words inside a variable name, even through the variable name starts in lower case, like in circleSolid.  That has a name: “camel case”… because it’s sort of like upper-case or lower-case, except it has a hump in the middle.

Le code précédent devient ainsi :

	import Graphics.Gloss

	pictures = Pictures
		[ translate 71 123
			(rectangleSolid 30 200)
		, rotate 45
			(translate 10 1
				(color green
					(circle 50)))
		, translate 100 100
			(scale 2 2
				(rectangleWire 10 10)) ]

C'est déjà mieux : on discerne la structure du code, l'imbrication des expressions.

### La factorisation du code

Logique.
Tests unitaires.

Dans un sens ou dans l'autre, top-down design et refactorisation.

Ordre de définition : top-down préféré.
On s'occupe des détails si on veut en les lisant ou écrivant après.
Prendre garde à analysis-paralysis. Imaginer avant et faire après, mais parfois améliorer après avec expérience.

#### Le top-down design

##### Un exemple

Nous allons, à travers un exemple concret, mettre au point une méthode qui permet de gérer facilement les programmes complexes, c'est-à-dire qui contiennent beaucoup d'images.
Elle se nomme le top-down design, ce qui signifie "design de haut en bas", ce que vous allez comprendre.

Notre but est de dessiner une assiette de légumes, qui ressemblera à :
![Schéma de l'assiette de légumes](http://cdsmith.files.wordpress.com/2011/09/dinner.jpg)

Commençons par dessiner une image vide.

	import Graphics.Gloss

	picture = blank

`blank` est une image vide.
Pour l'instant, le résultat est très sobre :
![Image vide](http://cdsmith.files.wordpress.com/2011/09/blank.jpg)

Ce que vous venons d'écrire s'appelle un stub {bon terme ?}. Nous avons donné une fausse définition à `picture` pour que notre programme puisse se lancer, bien qu'il ne soit pas terminé. Pour se souvenir des stubs dans notre programme, on peut les marquer d'un commentaire.
Si on veut éviter que notre programme compile, on peut donner `undefined` comme définition à une fonction.

Définissons `diner`, qui est le nom de notre programme.

	import Graphics.Gloss

	picture = dinner
	dinner = blank -- stub

Le résultat est le même pour l'instant, et ces étapes ont l'air inutile, mais ne sous-estimez pas la chance de nommer les éléments !

Complétons `diner` avec des stubs des éléments principaux que l'on souhaite dessiner. Si vous observez le schéma, vous voyez qu'il est grossièrement divisé en une assiette, de la nourriture, un couteau et une fourchette. Ajoutons aussi une table, qui sert d'arrière-plan à l'image entière.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = blank -- stub
	plate = blank -- stub
	food = blank -- stub
	fork = blank -- stub
	knife = blank -- stub

Il est inutile d'exécuter le programme parce-que le résultat est toujours le même ! Assez de `blank`, donc ; commençons à définir quelques éléments simples. Premièrement, la table qui est juste un rectangle marron de 500 x 500, autrement dit l'écran entier.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)
	plate = blank -- stub
	food = blank -- stub
	fork = blank -- stub
	knife = blank -- stub

Compilons-ça, et...

	Not in scope: `brown'

Nous avons utilisé la couleur `brown`, mais elle est n'est pas définie dans Gloss, mais nous pouvons le faire. Marron est plus ou moins de l'orange foncé.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)
	plate = blank  -- stub
	food  = blank  -- stub
	fork  = blank  -- stub
	knife = blank  -- stub

	brown = dark orange

Le résultat :
![Table](http://cdsmith.files.wordpress.com/2011/09/dinner1.jpg)

Ajoutons l'assiette, un grand cercle pour le contour et un plus petit au-dessus pour la jante, de deux nouvelles couleurs : `gray` et `lightGray`.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	food = blank -- stub
	fork = blank -- stub
	knife = blank -- stub

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

![Assiette ajoutée](http://cdsmith.files.wordpress.com/2011/09/dinner2.jpg)

Ajoutons, par exemple, les couverts. Que sait-on d'eux ?
 * ce sont un couteau et une fourchette
 * on veut qu'ils soient au milieu de l'axe y, mais opposés sur l'axe x, à peu près de 200 pixels de chaque côté.
 * Ils sont plus hauts qu'ils ne sont larges, peut-être 300px et 50px.
Au lieu s'occuper de tous les détails d'un coup, arrêtons-nous là. Pour l"instant, ignorons les formes différentes de la fourchette et du couteau et utilisons des rectangles. D'autre part, les positions sur la table des couverts ne font pas partie de ce qu'ils sont, donc la translation va dans la définition de `diner`, et pas celle de `fork` et `knife`.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	food = blank -- stub
	fork = rectangleWire 50 300 -- stub
	knife = rectangleWire 50 300 -- stub

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

Notez que `fork` et `knife` sont toujours des stubs. Ils ont une définition autre que `blank`, mais leur apparence n'est pas encore celle du résultat.

![Assiette et couverts](http://cdsmith.files.wordpress.com/2011/09/dinner3.jpg)

Préparons la nourriture. Nous allons la diviser en trois éléments principaux : une carotte et deux broccolis que nous allons définir comme des stubs pour l'instant.

	import Graphics.Gloss

	picture = dinner

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	fork = rectangleWire 50 300 -- stub
	knife = rectangleWire 50 300 -- stub

	food = pictures
		[ translate (-50) 50 (rotate 45 carrot)
		, translate (-20) (-40) (rotate 20 broccoli)
		, translate 60 (-30) (rotate (-10) broccoli)
		]

	carrot = rectangleWire 40 80 -- stub
	broccoli = rectangleWire 80 80 -- stub

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

Ne vous inquiêtez pas si vous n'avez pas deviné les nombres des `translate` et des `rotate` du premier coup.

![Dîner avec des stubs](http://cdsmith.files.wordpress.com/2011/09/dinner4.jpg)

Maintenant, nous avons utiliser une astuce. On veut se concentrer sur le dessin de la carotte, mais comme elle est déplacée et tournée, c'est difficile. C'est là que `picture = diner` va devenir utile. Nous allons, temporairement, changer cette définition par `picture = carrot`.

	import Graphics.Gloss

	picture = carrot

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	fork = rectangleWire 50 300 -- stub
	knife = rectangleWire 50 300 -- stub

	food = pictures
		[ translate (-50) 50 (rotate 45 carrot)
		, translate (-20) (-40) (rotate 20 broccoli)
		, translate 60 (-30) (rotate (-10) broccoli)
		]

	carrot = rectangleWire 40 80 -- stub
	broccoli = rectangleWire 80 80 -- stub

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

Le compilateur ne se préoccupe pas de toutes les autres définitions, parce-que nous lui avons dit que l'image était uniquement la carotte.

![](http://cdsmith.files.wordpress.com/2011/09/dinner5.jpg)

Sans toutes les distractions alentour, on devine facilement la définition de la carotte. C'est un trapèze, donc un polygône, orange qui possède un trapèze vert en guise de tige. Après un peu d'expérimentation, vous devriez obtenir quelque-chose comme :

	import Graphics.Gloss

	picture = carrot

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	fork = rectangleWire 50 300 -- stub
	knife = rectangleWire 50 300 -- stub

	food = pictures
		[ translate (-50) 50 (rotate 45 carrot)
		, translate (-20) (-40) (rotate 20 broccoli)
		, translate 60 (-30) (rotate (-10) broccoli)
		]

	carrot = pictures
		[ color green (polygon
			[(-5, 40), (-10, 55), (10, 55), (5, 40)])
		, color orange (polygon
			[(-5, -40), (-20, 40), (20, 40), (5, -40)])
	broccoli = rectangleWire 80 80 -- stub

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

Maintenant, nous pouvons vérifier que notre carotte s'inscrit bien dans `diner` en remettant `picture = carrot`. Nous allons faire de même avec tous les éléments restants : le broccoli, la fourchette et le couteau.
Voici donc le code final :

	import Graphics.Gloss


	{-
		This program draw a silver plate, with several food items and silverware
	-}

	picture = carrot

	dinner = pictures
		[ table
		, plate
		, food
		, fork
		, knife
		]


	-- Equipment.

	table = color brown (rectangleSolid 500 500)

	plate = pictures
		[ color gray (circleSolid 175)
		, color lightGray (circleSolid 150)
		]

	fork = color lightGray (pictures
		[ handle
		, translate 0 80 base
		, translate (-15) 100 prong
		, translate 15 100 prong
		])
    	where
    		handle = rectangleSolid 10 250
    		base = rectangleSolid 40 10
    		prong = rectangleSolid 10 45

	knife = color lightGray (pictures
		[ translate 0 (-25) handle
		, blade
		])
		where
			handle = rectangleSolid 30 200
			blade = polygon
				[ (-15,  75)
				, ( -5, 105)
				, ( 15, 125)
				, ( 15,  75)
				]


	-- Food.

	food = pictures
		[ translate (-50) 50 (rotate 45 carrot)
		, translate (-20) (-40) (rotate 20 broccoli)
		, translate 60 (-30) (rotate (-10) broccoli)
		]

	carrot = pictures
		[ color green (polygon
			[(-5, 40), (-10, 55), (10, 55), (5, 40)])
		, color orange (polygon
			[(-5, -40), (-20, 40), (20, 40), (5, -40)])
	broccoli = color (dark green) (pictures
		[ translate 0 (-15) base
		, translate (-15) 0 flower
		, translate 15 0 flower
		, translate 0 15 flower
		])
		where
			base = rectangleSolid 30 50
			flower = circleSolid 25


	-- New colors used in our program.

	brown = dark orange
	lightGray = dark white
	gray = dark lightGray

Nous avons aussi ajoutés des commentaires descriptifs qui divisent le programme en sections.
Voici l'image finale :

![Dîner](http://cdsmith.files.wordpress.com/2011/09/dinner7.jpg)

##### Une citation

Brian Kernighan, un des deux inventeurs dans les années 1970 du langage C, qui est le plus utilisé au monde pour faire des programmes, jeux, systèmes d'exploitations, ... a dit :
> Controlling complexity is the essence of computer programming.
He’s talking here about the important of organization, and of hiding unnecessary details, so that you can build programs without being overwhelmed by how complicated everything gets!  In fact, he says that’s the most important thing about computer programming.

##### Le design

Un design est une manière de programmer.
Le top-down design consiste à peu près en ces étapes :
 * Ecrire les définitions des fonctions appelées en premier par le programme
 * Diviser ces définitions en à peu près trois ou quatre appels de fonctions.
 * Parmi ces fonctions, écrire des stubs de nos fonctions personnelles.
 * Effectuer des tests unitaires : se concentrer sur ces fonctions en complétant leurs définitions sans se préoccuper de leur place dans les autres définitions.
 * Recommencer jusqu'à avoir complété tous les moindres détails.

###### Pratique

Pour la prochaine séance, programmez une image simplifiée de ces images ou d'une image de votre choix, aussi difficile, en utilisant le top-down design.

![Images à programmer](http://cdsmith.files.wordpress.com/2011/09/pictureexamoptions.jpg) {découper en 4 images}

### La portée des fonctions

La portée des fonctions désigne la possibilité de leur utilisation par d'autre fonction.
En anglais : "the function's scope".

Voici une analogie qui permet de mieux comprendre le concept. Si vous êtes en Angleterre, dire "J'ai vu la reine aujourd'hui !" est parfaitement sensé, mais si vous êtes aux Etats-Unis, ce n'est plus du tout le cas, parce-que ce pays ne possède pas de reine.
D'une certaine manière, l'expression "la reine" a différentes significations selon le lieu ou {accent} vous êtes.
Dans le cas des Etats-Unis, si ses habitants parlaient comme le compilateur Haskell, ils répondraient : "Not in scope: la reine".

#### Le premier niveau

Toutes les fonctions que l'on a défini jusqu'à maintenant sont dites au premier niveau {mauvais terme ?}.

Par exemple, dans :

	import Graphics.Gloss
	
	picture = character
	
	character = pictures
		[ face
		, translate (-10) (-15) foot
		, translate 10 15 foot

	face = circle 15
	foot = rectangleSolid 7 4

les fonctions `picture`, `character`, `face` et `foot` sont au premier niveau.
Toutes les fonctions importées des modules sont aussi de premier niveau.
Donc, dans `Graphics.Gloss`, `circle`, `translate`, `red`, etc. sont de premier niveau.

Les fonctions de premier niveau sont accessibles par toutes les autres fonctions.

#### Les niveaux inférieurs

Nous allons voir comment définir des fonctions de niveaux inférieurs et leur utilité.

Certaines fonctions ne sont utiles que dans une seule fonction et on souhaiterait éviter qu'elles dérangent le premier niveau, surtout lorsqu'on écrit des programmes complexes.
On utilise pour cela, au choix, le mot-clé `let` ou `where`.

Prenons l'exemple du bonhomme de neige.

	import Graphics.Gloss

	picture = snowman

	snowman = pictures
			[ translate 0 50 top
			, translate 0  0 middle
			, translate 0 (-75) bottom
			]

	top = circleSolid 50
	middle = circleSolid 70
	bottom = circleSolid 90

On souhaite éviter que `top`, `middle` et `bottom` soient au premier niveau et que leur portée se réduise à la fonction `snowman`.

 * Avec `let` :

	import Graphics.Gloss

	picture = snowman

	snowman =
		let top = circleSolid 50
			middle = circleSolid 70
			bottom = circleSolid 90
		in pictures
			[ translate 0 50 top
			, translate 0 0 middle
			, translate 0 (-75) bottom
			]

	La syntaxe de ce mot-clé est `let {definitions} in {expression}`. L'ensemble `let ... in ...` est une expression.
Après `let`, on peut écrire des définitions, puis le`in`, après quoi on écrit l'expression.
Les trois fonctions `top`, `middle`, `bottom` sont uniquement accessibles entre elles et dans l'expression qui suit le `in`.
On peut écrire tout en une ligne : `let { top = circleSolid 50, middle = circleSolid 70, bottom = circleSolid 90 } in pictures [translate 0 50 top, translate 0 0 middle, translate 0 (-75) bottom]`, mais ce n'est pas recommandé parce-que peu lisible.

 * Avec `where` :

	import Graphics.Gloss

	picture = snowman

	snowman = pictures
		[ translate 0 50 top
		, translate 0 0 middle
		, translate 0 -75 bottom
		]
		where
			top = circleSolid 50
			middle = circleSolid 70
			bottom = circleSolid 90

	La syntaxe de ce mot-clé est `{expression} where {definitions}`.
A la différence de `let`, les définitions de fonctions à portée limitée se trouvent après l'expression. Cette syntaxe, dans l'esprit du top-down design, est généralement préférée à celle du `let`, parce-qu'elle est jugée plus lisible.
On peut aussi tout écrire en une ligne : `pictures [translate 0 50 top, translate 0 0 middle, translate 0 -75 bottom] where { top = circleSolid 50, middle = circleSolid 70, bottom = circleSolid 90}`.

### Le choix des noms et les commentaires

Steve McConnell a dit :
> If you’re about to add a comment, ask yourself, "How can I improve the code so that this comment isn’t needed?"

C'est un excellent concept. Les commentaires permettent d'expliquer le programme, mais il est encore mieux d'écrire le programme de façon tellement claire et évidente que les commentaires ne sont pas utiles. Voyez donc si vous pouvez vous passer de commentaires en respectant le principe KISS {détailler}, mais mettez-en au cas ou.
Cependant, même pour un programme clair, des commentaires sont utiles dans le cadre d'un tutoriel pour les débutants ou d'un partage du code avec des gens susceptibles de ne pas bien connaître la bibliothèque, le langage, etc.

## Pratique

Organisez autant que possible votre code !
