# Cours original

Ce cours est basé sur la traduction de [Haskell for Kids](https://cdsmith.wordpress.com/2011/08/03/haskell-for-kids-introduction/).

# Ce que nous allons utiliser

## La programmation

La programmation permet aux êtres humains de créer des programmes facilement. C'est un langage intermédiaire que les humains et les ordinateurs comprennent.
De leur côté, les programmeurs transforment leur idée de programme en un code informatique, que l'ordinateur transforme en langage machine au moyen d'un programme appelé le compilateur.

## Le langage Haskell

Il existe beaucoup de langages de programmation, comme il existe beaucoup de langues : C++, Java, LISP, Python, JavaScript, Tcl, Forth, ...
Haskell n'est pas le plus connu d'entre eux, pour des raisons historiques, mais ses utilisateurs, souvent des anciens programmeurs C++, Java, etc. avouent le préférer langage pour sa simplicité, ses concepts puissants, sa sécurité, ses performances, son intérêt intellectuel, etc.

## La bibliothèque Gloss

Les bibliothèques sont des morceaux de programmes que d'autres personnes ont écrit pour nous, pour éviter qu'on reparte de zéro à chaque fois, qu'on ré-invente la roue.
Par exemple, il existe une bibliothèque qui dessine des fenêtres pour nous. Si nous devions le faire à chaque fois, on devrait dessiner pixel par pixel sa barre, ses boutons, ses bords, etc. avec beaucoup de rectangles, de cercles, d'icônes, alors que la bibliothèque peut le faire à notre place.

C'est un des principes chers à l'informatique et donc la programmation : l'automatisation : l'être humain dit une seule fois à la machine comment faire, et la machine le répète autant de fois que nécessaire.
D'ailleurs, le mot "informatique", qui a été inventé par [Philippe Dreyfus](https://fr.wikipedia.org/wiki/Philippe_Dreyfus) est la contraction d'"information automatique". Le terme anglais, "computing", signifie plutôt "l'activité de calculer".

Nous allons utiliser la bibliothèque Gloss, qui nous permet d'afficher des graphismes et de gérer l'interaction avec l'utilisateur, le joueur, d'une façon très simple et élégante.

# Ce que nous allons faire

## Plan

Nous allons, dans cet ordre de progression, écrire des programmes informatiques pour :
 * afficher des images immobiles.
 * afficher des images animées.
 * réaliser un jeu, c'est-à-dire afficher des images animées en fonction de l'interaction avec le joueur et d'un monde virtuel en mouvement.
Cela va prendre plusieurs semaines.

## Objectifs

Ce cours n'est pas destiné à une simple mémorisation de programmes informatiques.
Il s'agit d'être créatif, d'expérimenter, de réaliser un programme qui nous plait.
Après la théorie, beaucoup de temps sera réservé à la pratique, afin d'essayer les nouveaux concepts étudiés avec vos idées.

## Comment profiter de ce cours

Ce cours apprend principalement à utiliser Gloss mais aussi plus généralement Haskell. Il introduit parfois des termes informatiques et des informations culturelles. Ce n'est pas grave si vous ne parvenez pas à tout retenir, ni si vous ne comprenez pas du premier coup. Réfléchissez un peu, relisez, puis passez à la suite, et revenez-y plus tard. Les concepts importants s'apprendront facilement à force d'être exploités à de nombreuses reprises. Les concepts sont toujours assortis d'exemples et de travaux pratiques à la fin.
Certains paragraphes et certaines parties sont longs, mais ils ne développent qu'une idée de façon à ce qu'elle soit comprise.

# C'est parti !

Commencons sans plus tarder à programmer.
