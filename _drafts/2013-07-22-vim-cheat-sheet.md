---
layout: post
title:  "My vim cheat sheet"
published: true
description: "This is yet another vim cheat sheet, don't forget it's availaible on github if you want to tweak it"
categories: [linux]
tags: [vim, cheat sheet, cheat,sheet]
image:  false
article: yes
---

***********************************************
mouvement
***********************************************
s - phrase
w - mot
W - MOT
t - balise ( 'dat' supprime depuis <xml-style-tag> jusqu'à </xml-style-tag>
p - paragraphe
B - bloc de code ( '{' ou '}' fonctionne seulement pour des blocs de style C)

) fin de a phrase
} fin du paragrap
$ fin de la ligne 
^ debut de la ligne
gg debut de fichier
G fin de fichier
gD va à la definition de la methode
% si sur une ( ou [ va a à la prochaie  ou ] sinon va au precedent
:25 va a la ligne 25

Formes de parenthèse:
	( ou ) - parenthèse ( ... )
	[ ou ] - [ .. ] 
	< ou > - se rapporte à <..>
	{ ou } - { bloc également visé par le bloc b ci-dessus})

marks:
`ma`      - marque la position actuelle avec la lettre 'a'
`\`a`      - revient à la marque a
`'a`      - place le curseur au début de la ligne a
`:marks`    pour voir une liste des balises effectuées.'

Control+(f/F)   avance d'un écran  (Page suivant
Control+(b/B)   recule d'un écran (Page précédente)
Control+u       monte d'un demi-écran
Control+d       descend d'un demi-écran

***********************************************
mode visuel
***********************************************

v       - entre dans le mode visuel.
Shift+v - entre dans le mode visuel, select lignes seulement
Ctrl+v  - entre en mode visuel rectangulaire (mode bloc).

ex:
shift-v 
selectionner 3 ligne
==
=>indente 3 lignes

***********************************************
buffer
***********************************************
:bn buffer next
:bp buffer previous
:bd ferme le buffer
:ls liste les buffers
:bufdo cmd ewecute cmd sur tous les buffers (ex: bufdo w, bufdo bd)
:bn buffer next

***********************************************
windows
***********************************************
:vsplit ou :vsp (coupe en deux separation verticale)
:split ou :sp (coupe en deux separation horizontale)
C-W fleche (navigue entre les fenetre)
Ctrl + w puis + : agrandit le viewport actuel.
Ctrl + w puis - : réduit le viewport actuel.
Ctrl + w puis = : égalise à nouveau la taille des viewports.
Ctrl + w puis r : échange la position des viewports. Fonctionne aussi avec « R » majuscule pour échanger en sens inverse.
Ctrl + w puis q : ferme le viewport actuel

***********************************************
dev
***********************************************
si vim lance dans root du projet
:make 
:make cible 
==> peut etre redefini
:! lance une commande linux (ex: :!echo 'toto')

***********************************************
trucs
***********************************************
ctx            - supprime tout jusqu'au caracrere x et rentre en insertion
dtx            - supprime tout jusqu'a x ne rentre pas en insertion
da(            - supprime le contenu des parentheses
da"            - supprime la chaine de caracteres
ytg            - copie tout jusqu'aà la lettre g
Shift+j (ou J) - Cela ajoutera la ligne précédente à la fin de la ligne couteran
Shift+k (ou K) - Affiche la page de manuel pour le mot sous le curseur
~              - Modifie la casse du caractère sous le curseu
.              - repeète la dernière commande, quelle qu'elle soit
:u             - undo
<C-r>          - redo
Control+A      - Incrémente le nombre sous le curseur
Control+X      - Décrémente le nombre sous le curseur
r              - remplace une lettre
R			   - mode edition en replace

***********************************************
mapping
***********************************************
:map (insert+normal)
:nmap mode normal
:imap (mode interactif)
:iab substition (:iab toto supet=toto)

