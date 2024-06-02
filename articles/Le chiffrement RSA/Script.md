## Disclaimer :

Petit disclaimer avant de commencer cet article, je ne suis ni cryptologue, ni mathématicien, ni expert en cybersecurité, bref, il est possible que je fasse des erreurs, je suis juste un curieux qui s’intéresse a un sujet passionnant. Ne basez donc pas les travaux de vos recherches sur cet article, allez plutôt voir la description dans laquelle j'aurai normalement mis quelques sources fiable si vous souhaitez en apprendre plus sur le sujet. 

Je vous invite également a aller voir la section commentaire, que ce soit pour me corriger ou pour aller voir les possibles corrections qui ont été faites.

Ce disclaimer étant fait, on peut attaquer la vidéo sur un sujet, qui, vous allez le voir, est plus que passionnant.

## Cryptographie ? :

Afin de poser quelques bases, commençons par faire un petit retour en arrière sur la cryptographie :

Déjà, le concept de la cryptographie c'est de rendre inintelligible le contenu d'un message pour toute personne non légitime, en d'autres termes, c'est le concept de rendre incompréhensible un message pour toute personne non conviée a le lire.


> [!warning] Attention
> La cryptographie n'est pas à confondre avec la sténographie, qui est le concept de dissimuler un message dans un autre. Ici le rôle de la cryptographie est de rendre le sens du message en question inintelligible sans pour autant chercher à le dissimuler.


Les premiers cas de cryptographie sont assez anciens. Un des exemples les plus connus, bien qu'il ne s'agisse pas du premier cas de cryptographie enregistré, est le chiffre de César¹, qui consiste à décaler les lettres de l'alphabet pour chiffrer le contenu d'un message. Bien qu'il s'agisse d'un type de chiffrement très simple et peu sécurisé, nous allons nous y attarder un peu, car il permet d'expliquer de manière simple des concepts mathématiques nécessaires pour la suite. 

> [!info]- ¹ Info
> Nom donné en rapport au fait que l'empereur romain Jules César utilisait ce type de chiffrement pour chiffrer ses messages.


## Chiffre de César

Pour chiffrer un message avec le chiffre de César, on suit le raisonnement suivant :

- Définir, pour chaque lettre, un nombre en fonction de sa position dans l'alphabet.

- Définir une fonction mathématique (ici une addition) afin de décaler les lettres.

- Décaler chaque lettre sur la base de la fonction mathématique, par ex. le **A** devient un **D** pour une fonction :
$$
+3
$$
- Pour déchiffrer le message, utiliser la fonction inverse, ici :
$$
-3
$$

> [!info] Remarque
>Ici, la fonction mathématique = clé de chiffrement & de déchiffrement, et cela porte un nom : le chiffrement symétrique, de part le fait que l'on utilise la même clé dans les deux sens.

Seulement, si l'on envoie un message chiffré avec une méthode de chiffrement symétrique, comme le chiffre de César, il est nécessaire d'envoyer la clé avec notre message, sinon notre interlocuteur ne sera pas capable de déchiffrer notre message. Cela veut donc dire que si quelqu'un non convié à lire le message intercepte la clé, il est capable de déchiffrer le message.

Se présente donc le problème de raisonnement logique suivant : 

>[!quote] :
*Si l'on peut transmettre la clé via un canal suffisamment sécurisé pour qu'elle ne soit pas interceptée, pourquoi ne pas directement envoyer le message en clair via ce canal sécurisé ?*


> [!note]-  Code python du chiffre de César
> >
> ```python
> def chiffre_de_cesar(message, decalage):
> alphabet = 'abcdefghijklmnopqrstuvwxyz'
>message_chiffre = ''
>for lettre in message:
>	if lettre in alphabet:
>		position = alphabet.index(lettre)
>		nouvelle_position = (position + decalage) % 26
>		lettre_chiffree = alphabet[nouvelle_position]
>		message_chiffre += lettre_chiffree
>	else:
>		message_chiffre += lettre
>	return message_chiffre
>
>message = input("Entrez le message à chiffrer : ")
>decalage = int(input("Entrez le décalage pour le chiffrement de César : "))
>
>message_minuscule = message.lower()
>message_chiffre = chiffre_de_cesar(message_minuscule, decalage)
>
>print("Message chiffré :", message_chiffre)


## Chiffrement asymétrique, la solution magique ?

Pour palier à ce problème, on a inventé un type de chiffrement différent : le chiffrement asymétrique, le concept est le suivant :

- Alice veut pouvoir recevoir des messages de la part de Bob de manière sécurisée.
- Alice génère donc 2 clés, une clé privée, qu'elle garde pour elle, et une clé publique, qu'elle envoie a Bob.
- Bob veut envoyer un message a Alice, il écrit son message, le chiffre avec la clé publique, puis l’envoi a Alice.
- Alice reçoit le message puis le déchiffre avec la clé privée.
- Marie, qui espionnait la conversation depuis tout ce temps, a réussi à intercepter le message chiffré qu'a envoyé Bob et la clé publique qu'il a utilisée pour le chiffrer.
- Malgré toutes ses tentatives, Marie n'arrive pas à déchiffrer le message de Bob, cela est dû au fait que la clé publique, également appelée clé de chiffrement, ne permet pas de déchiffrer les messages qu'elle a chiffrés.

Et c'est à ce moment la que vous vous demandez peut être comment c'est possible ?

Après tout c'est vrai que dans les mathématiques que l'on a apprit à l'école, nous avons toujours plus ou moins inconsciemment compris qu'une fonction mathématique possédait toujours son inverse : on peut inverser une addition avec une soustraction, même chose pour la multiplication qui s'inverse avec une division.

Et, de manière plus générale, dans un problème à 3 valeurs, comme une addition, que l'on peut noter ainsi :
$$
x + y = z
$$
À partir du moment où l'on connaît 2 des 3 valeurs, on peut en trouver la troisième.

**Ex (courant) :**
$$
1+2=?
$$
Dans cet exemple, plus que courant, il suffit d’additionner les 2 valeurs pour trouver le résultat, qui est notre troisième valeur.

**Ex (moins courant) :**
$$
1+?=3
$$
Dans cet exemple, légèrement moins courant, ou peut tout de même trouver l'inconnue, il suffit de soustraire 1 à 3 et l'on obtient 2.

**Marie se trouve dans cette situation :**
$$
M+c=C
$$
Ici, $c$ représente la clé de chiffrement, $C$ le message chiffré et $M$ le message en clair.

Marie possède donc bien deux valeurs sur trois : $c$ et $C$, respectivement la clé de chiffrement et le message chiffré. Pourtant il n'est pas possible de déchiffrer le message, même avec ces deux éléments.

Plongeons nous alors un petit peu dans le monde des mathématiques pour expliquer le pourquoi du comment.

> [!info]
> À noter que je vais détailler ici la génération de clés pour le protocole RSA, qui est le protocole de chiffrement asymétrique le plus connu et le plus utilisé, cependant retenez bien qu'il ne s'agit pas de l'unique protocole de chiffrement asymétrique. 

Pour commencer il nous faut deux valeurs $p$ et $q$, qui doivent respectivement être des nombres premiers, pour ceux qui auraient oublié, les nombres premiers ont cette particularité de ne se diviser que par 1 et eux même, par exemple le chiffre 7 n'est divisible que par 7, donc lui même, et par 1.

Ensuite il nous faut trouver la valeur $n$, qui est égal à $p$ multiplié par $q$. Faisons un exemple : définissons $p$ par $11$ et $q$ par $13$. Si on multiplie donc ces deux valeurs on obtient $143$.

Ensuite, il nous faut trouver une variante de $n$, a savoir $\phi(n)$, qui est égal a $(p-1)\times(q-1)$, donc dans notre exemple : $$(11-1)\times(13-1)=10\times12=120$$

Ça va nous permettre de trouver $e$, qui est égal a n'importe quel nombre premier à condition qu'il soit différent de $\phi(n)$, on peut le noter ainsi : 
$$
e \in \mathbb{P}\ne \phi(n)
$$
Pour notre exemple, $7$ fonctionne bien. 

Enfin il nous manque $d$, qui va nous permettre de générer notre clé privée, qui est égal à l'inverse modulaire de $e \bmod \phi(n$), que l'on peut noter 
$$d \times e \equiv 1 (\bmod \phi(n))$$

Je vous vois venir, Modulo c'est un dérivé de la division euclidienne, qui a la particularité de ne travailler qu'avec des entiers, ce qui a pour conséquence de nous donner un reste. 
Par exemple si on divise $45$ par $12$, on obtient un reste de $9$. Eh bien modulo, c'est le calcul de ce reste, en termes plus concret, c'est le calcul du reste d'une division euclidienne.

Pour finir il nous manque notre valeur $M$, qui est égal au message qu'on souhaite chiffrer, par exemple, "Hello world". Problème, on ne sait pas encore calculer des lettres, il faut donc convertir ces lettres en nombres, on peut par exemple utiliser la table ASCII pour convertir "H" en $104$. 

Voilà maintenant on a toutes nos valeurs pour générer notre clé publique via la formule suivante :
$$
C = M^e \bmod n
$$
où $C$ représente notre message une fois chiffré.

et notre clé privée : 
$$
M = C^d \bmod n
$$

En utilisant notre exemple cela donne, pour notre clé publique, la formule suivante : 
$$
104⁷ \bmod 143
$$
Ce qui donne pour résultat $91$.

Bravo, vous venez de chiffrer un message !


Pour ceux appréciant les schémas, en voici un qui résume la génération de nos deux clés :

![[Schéma génération clé RSA.canvas]]


## Le protocole RSA, incassable ?

Alors comment se fait il que l'on ne puisse pas récupérer le message en clair en étant en possession de la clé publique ? Ne peut on pas simplement inverser la fonction de chiffrement ?

Eh bien... Non, parce que la fonction $\text{modulo}$ est une fonction que l'on appelle "à sens unique", il n'est pas possible de l'inverser. L'unique moyen de trouver la valeur originale d'une fonction $\text{modulo}$ à partir de son résultat, c'est de tester toute les valeurs comprises entre $0$ et $C$ en entrée. Pour illustrer avec notre précédent exemple $C = 91$. On testera donc toutes les valeurs comprises entre $0$ et $91$, jusqu'à ce qu'on obtienne $91$ comme résultat. Ce qui, entre nous, est long, fastidieux et chiant.

Surtout quand on sait qu'il existe un solution plus simple et rapide : générer la clé privée à partir de la clé publique. Mais comment fait-on ? Eh bien il nous faut trouver $p$ et $q$ afin de pouvoir calculer $\phi(n)$ nécessaire à la génération de $d$. Et on a de la chance, on sait que lors de la génération d'une clé RSA, $p$ et $q$ doivent obligatoirement être des nombres premiers, donc pour arriver à nos fins il nous suffit de factoriser $n$. Ce qui consiste à tester la divisibilité de $n$ avec tout les nombres premiers, jusqu'à ce que l'on obtienne un entier comme résultat.

Testons avec notre précédent exemple, où $n = 143$, on va donc tenter de le diviser un par un par chaque nombre premier : 

- Avec $2$ on obtient $71.5$ ($143 \div 2$), ce qui n'est pas bon, vu qu'on obtient un résultat avec décimales.
- Avec $3$ on obtient $47.667$ ($143\div3$), ce qui est également incorrect à cause de la décimale.
- Avec $5$ on obtient $28.6$ ($143\div5$), ce qui est incorrect.
- Avec $7$ on obtient $20.428$ ($143\div7$), ce qui est incorrect.
- Avec $11$ on obtient $13$ ($143\div11$), ce qui est correct, on trouve bien un entier (*donc sans décimales*) ce qui signifie que 143 est divisible par 11.

À partir de la on est sur le problème à trois valeurs où l'on possède bien deux des trois valeurs, il nous reste qu'à diviser $n$ par $p$, qu'on vient de trouver en factorisant $n$, ce qui nous donne $13$, on sait donc que $p = 11$ et $q = 13$.

Il ne nous reste donc plus qu'à générer $\phi(n)$ pour trouver $d$, et on peut déchiffrer le message.

Et alors autant vous le dire tout de suite, non, je ne suis pas un génie de la cryptographie et non, je ne viens pas de casser l'un des protocoles de sécurité les plus utilisés au monde.

En fait il y a un petit détail que j'ai oublié de vous préciser : la taille des nombres premiers choisis pour $p$ et $q$. Pour rendre l'explication plus facile, j'ai volontairement choisi de petit nombres, en réalité on utilise de bien plus grands nombres, par exemple : 
$$
548717676914437574112305164192041519101957775986250931657313794583508266879255075895690285746114750436058485590089645905781561558179608685023009243230986418731425867903273456619022344826437309116580206176848695245226376774118542838101343360743651677322228305626431209374915460540470411831909752838531210846501184437491279149292864371943768047289261938181359124235943900909386284927028121907543585752840679965916901535321753641043575407353477562450213838363883206298268116237470209604649822674519996709178102812129429162360147898790649907624801234611690084728508989429558370736413845257924421837382291784016454005639501531934562602811945134062634882757980638639803908834213351495132834965075471591382187879845738212970152922770661324865689093148642149293200507580150839244643081022045612781231367984956098829404799907266049304365835540405225173907140565891852163923377441499247833664550213931081082745226996199013511163436598348973152361662366745306247595915428436851463886958902838498548881745981356926278303753580339216011714730006012326389515424740694058786902833310619346133320551648038208581338274214736266789567386457036617084756434538499505047661085161222908897432939955291482749144502803897837543337503256080250969700215238401
$$
*oui, c'est pas mal grand.*

Et rappelez vous, pour pouvoir retrouver $p$ et $q$ à partir de $n$, il nous faut diviser $n$ par tout les nombres premiers jusqu'à obtenir un entier comme résultat, hors le temps nécessaire pour arriver à diviser $n$ par de tels nombres, même pour un superordinateur, se compte en millions d'années.

Ce qui fait donc du protocole RSA un algorithme de chiffrement particulier, parce que théoriquement faillible, mais pas en pratique, de part la durée nécessaire pour factoriser $n$.


## RSA, infaillible à tout jamais ?

Est ce pour autant que nous ne parviendrons jamais à casser une clé RSA ? Il y a deux situation plausibles : 
- La première, c'est que nos ordinateurs soient devenus suffisamment puissant pour pouvoir réussir à factoriser $n$  dans des temps concevables pour l'être humain. Mais, soyons honnête, il y a peu de chances que cette situation voit le jour. Déjà parce que cela nécessiterait de faire un bond technologique gigantesque pour atteindre de telles puissances de calculs, ensuite, c'est facilement contournable, il suffit d'encore augmenter la taille des nombres premiers $p$ et $q$ pour croître de façon exponentielle le temps nécessaire pour factoriser $n$.
- La deuxième, c'est l'algorithme de Shor, que je ne m’essaierai pas à vous expliquer, parce qu'on arrive aux limites de mes compréhensions mathématiques. Ce que je peux vous dire, c'est qu'il s'agit d'un algorithme reposant sur la puissance de l'informatique quantique pour factoriser $n$ dans des temps relativement restreint. Jusqu'à ce jour, l'algorithme de Shor n'a été mis a exécution qu'une fois, en 2001, par IBM, pour factoriser $15$ en $3$ et $5$, bon autant vous dire que, ça, moi aussi je sais le faire. Mais si une expérience de plus grande envergure voit le jour, elle pourrait alors sérieusement compromettre la sécurité de beaucoup de protocoles utilisant l'algorithme RSA, comme certains réseaux bancaires. Mais ne paniquez pas trop vite, l'attaquant doit tout de même être en possession d'un calculateur quantique pour mettre l'algorithme de Shor à exécution, et ça ne court pas les rues.
