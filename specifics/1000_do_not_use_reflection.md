## N'Utilisez pas la Réflection

### Sommaire

N'utilisez pas la réflection dans votre code (càd rien qui ne provienne du package `java.lang.reflect`).

### Détails

La réflection est un outil puissant; il permet à Java de faire des choses qui autrement serait impossible ou nécessiterait une importante quantité de code bateau.

Mais, bein qu'elle soit parfois utile quand on crée un framework ou une bibliothèque, il est peu problable qu'elle soit la bonne solution pour résoudre le genre de problèmes que nous rencontrons dans un code classique.

Alors pourquoi vouloir éviter un outil aussi puissant fournit par Java?

La réflection a trois inconvénients:

#### Perte du processus de sécurité lors de la compilation

La réflection transfert les erreurs de compilation vers l'éxécution. - c'est une Mauvaise Chose &trade;

Le compilateur est la première forme de défense contre les défauts et le système de types est l'outil le plus efficace dont nous disposons pour documenter notre code. Nous ne devrions pas jeter à la corbeille ces deux choses de façon désinvolte.

#### Perte de la Sécurité dans le Refactoring

Le refactoring et les outils d'analyse de code sont aveugles face à la réflection.

Bien qu'ils puissent essayer de la prendre en compte, les possibilités apportées par la réflection sur le comportement d'un programme font qu'ils ne peuvent garantir de façon rigoureuse qu'ils ont compris le programme. En présence de réflection, les refactorings qui autrement seraient sûrs, peuvent changer le comportement du programme et de l'outil d'analyse.

#### Compréhension du Code plus Difficile

Dans la même manière que la réflection rend difficile la compréhension du code par des outils, elle la rend également plus difficile pour des humains.

La réflection amènes des surprises.


*Cette méthode n'est jamais appelée, je peux sans danger la supprimer.* Oh. J'ai oublié la réflection.

*Je peux sans danger changer le comportement de cette méthode privée comme je connais d'où elle est appelée.* Oh. J'ai oublié la réflection.

