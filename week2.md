# Séance 2

## Cours

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

> "Any fool can write programs that a computer can understand.  Good programmers write code that humans can understand."
Martin Fowler

> "Great software, likewise, requires a fanatical devotion to beauty.  If you look inside good software, you find that the parts no one is ever supposed to see are beautiful, too.  I’m not claiming I write great software, but I know that when it comes to code, I behave in a way that would make me eligible for prescription drugs if I approached everyday life the same way.  It drives me crazy to see code that’s badly indented, or that uses ugly variable names."
Paul Graham

> "There are two ways of writing computer programs: One way is to make it so simple that there are obviously no mistakes.  The other way is to make it so complicated that there are no obvious mistakes."
C.A.R. Hoare

The last one is actually slightly misquoted.  I didn’t correct it because the original version means nearly the same thing but uses more technical language.

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

> "Controlling complexity is the essence of computer programming."
- Brian Kernighan
Brian Kernighan was one of two people who invented C, one of the most popular programming languages in the world.  He’s talking here about the important of organization, and of hiding unnecessary details, so that you can build programs without being overwhelmed by how complicated everything gets!  In fact, he says that’s the most important thing about computer programming.

### La portée des fonctions

`let` et `where`

Avec `let` :

	import Graphics.Gloss

	picture = snowman

	snowman = pictures [
		translate 0 ( 50) top,
		translate 0 (  0) middle,
		translate 0 (-75) bottom
		]
	  where
		top    = circleSolid 50
		middle = circleSolid 70
		bottom = circleSolid 90

Avec `where` :

	import Graphics.Gloss

	picture = snowman

	snowman =
		let top    = circleSolid 50
		    middle = circleSolid 70
		    bottom = circleSolid 90
		in  pictures [
		        translate 0 ( 50) top,
		        translate 0 (  0) middle,
		        translate 0 (-75) bottom
		        ]

The scope of a variable is how much of the program it can be used in.  When you write more involved programs, you don’t want every single variable that you define to be visible everywhere!  As this snowman program gets bigger, maybe we might want the same name, like top, to mean something completely different when we’re drawing a different part of the scene.  That’s okay, because this top is only “in scope” during the definition of snowman.
We talked about an analogy for this: if you’re in England, it makes perfect sense to say “I saw the queen today!”  But, if you’re in the United States, it doesn’t make sense any more, because we don’t have a queen.  The phrase “the queen” means something different depending on where you are!  So if I talked like the Haskell programming language, and you said to me “I saw the queen today!”, I might say in response, “Not in scope: the queen”.

Séparation en fichiers puis en bibliothèques.

### Le choix des noms et les commentaires

> "If you’re about to add a comment, ask yourself, “How can I improve the code so that this comment isn’t needed?”"
Steve McConnell

This is a great concept!  We talked about using comments to explain what’s happening in your programs: but what’s even better than having a comment is organizing your program so that it’s obvious what you’re doing, and you don’t need the comment any more.  This certainly doesn’t mean never write comments: it just means see if you can keep things simple so there’s not as much to explain.

## Pratique

Organisez autant que possible votre code !
