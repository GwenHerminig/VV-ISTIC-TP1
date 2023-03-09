# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
HAMONO Hermine
Roy Raphaël

### Question 1
https://www.millenium.org/news/400432.html

Il existe un bug dans le jeu league of legends ou une interaction spécifique entre deux personnages du jeu entraîne un comportement problématique. En effet, un sort est censé disparaître à la mort de Aurélion Sol mais si celui-ci ne meurt pas dans la même dimension (possible grâce à un sort du Mordekaiser) le sort reste sur la map sans possibilité de l'enlever ruinant l'experience des joueurs. Ceci est un bug locale et en ayant pris en compte cette situation dans les tests unitaires il aurait été possible d'éviter ce problème.

### Question 2 
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues
Ce bug consiste en l'appel de la fonction retainAll de la classe CollectionUtils, servant à enlever tous les éléments d'une liste qui ne sont pas contenus dans une collection donnée. Cette fonction est censée rendre une exception de type NullPointerException quand l'un des paramètres (la liste ou la collection) était égale à null, cependant l'exception n'est pas détectée ni rendue, témoignant d'un bug dans le code.
Ce bug est local car il ne concerne que la méthode retainAll.
La solution de ce bug a été d'ajouter une conditionnelle "requireNonNull" aux paramètres pour traiter le cas nonNull automatiquement.
Pour la suite, les développeurs ont ajouté des éléments dans la classe de test : https://github.com/apache/commons-collections/commit/7eb78290c8d7d1fa379536700de0bd4a81320bb0#diff-27c19e081e1e90e79b36daef451dcb7b44c295b56d0575e4f63648d7f3d158dc

### Question 3
L'experience concrète que Netflix effectue avec leur service Chaos Monkey est l'arrêt aléatoire de systèmes dans l'application (ex: les machines virtuelles) pour observer le comportement de l'application.
Pour pouvoir effectuer ce type de tests il est nécessaire de savoir dans quelles circonstances on peut dire que notre test est réussis. De plus il faut garantir que le test soit aléatoire et qu'il ne se concentre pas sur les cas faciles ou ceux dont on est sûr du fonctionnement.

Du côté de Netflix ils essaient de regarder ce qu'il se passe quand : 
    - Certaines machines virtuelles sont KO
    - Ils ajoutent du délai entre les requêtes
    - Certaines requêtes ne fonctionnent pas
    - Un service entier ne fonctionne pas
    - Une région d'Amazone n'est plus disponible
    
Le but de l'experience est de comparer un groupe test et un groupe avec "problèmes" puis de voir si le comportement attendu est le même des deux côtés. Après avoir fais l'experience, Netflix a plus de confiance en son application quand celui-ci est en présence des variable mentionnée plus haut.

Netflix n'est pas la seule entreprise à effectuer du Chaos Engineering. De plus l'informatique n'est pas le seul domaine ou on peut voir ce type de réflections, par exemple dans un hôpital il est vital de pouvoir être sûr de la continuitée de l'apport d'électricité il y a donc périodiquement des tests pour vérifier le fonctionnement des générateur de secours en cas de panne. Dans le domaine de l'informatique, je peux citer ARKEA à qui il arrive de tester ses services avec des serveurs en moins.


### Question 4
Il y a plusieurs avantages à avoir une implémentation formelle, par exemple de pouvoir varier l'appareil sur lequel le code est utilisé (sous réserve que celui-ci prenne en charge WebAssembly). Cela sert également à assurer que les différentes implémentations utilisant WebAssembly sont compatibles entre elles. Enfin, cela permet de faciliter la compilation et la validation du code.
De plus, l'implémentation formelle étant presque tout le temps déterministe cela permet d'éviter des problèmes comme la division par zéro par exemple.
Le fait d'éviter la majorité des cas critiques pourrait mener à penser que les tests ne sont pas aussi utiles quand dans le cas d'implémentation moins formelles. Cependant, même si on est censé éviter un grand nombre de bugs il peut en rester. De plus, il faut tester que tout les cas ont bien été pris en compte par la définition formelle car il est toujours facile de faire une erreur d'interprétation. WebAssembly l'a bien compris et à mis en place un interpreteur de réferences utilisé pour tester les différentes implémentation et spécifications.

### Question 5
D'après l'auteur la qualité principale de la spécification mécanique est l'analyse formelle et la vérification qui sont plus simple à mettre en place. Cela à permis à l'auteur d'améliorer les spécifications formelles déjà en place. Dans l'article l'auteur a présenté des preuves et à montrer la validité d'implémentations de l'interpréteur et de la vérification de types. Il a fait passer tout les tests accessibles dans WebAssembly pour tester la conformité du langage. Il y a aussi eu des tests différentiels sur l'interpréteur. Il faut cependant continuer à tester, car ce n'est pas parcequ'une spécification est valide qu'elle effectue le comportement attendu dans tout les cas.
