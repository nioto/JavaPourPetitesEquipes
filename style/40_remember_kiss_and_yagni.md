## Se Souvenir de KISS et YAGNI

### Sommaire

Conserver votre design aussi simple que possible.

Ne créer que les fonctionnalités dont vous avez besoin maintenant - pas ce que vous pensez avoir besoin dans le futur.


### Détails

Les acronymes KISS (Keep It Simple, Stupid) and YAGNI (You Ain't Going To Need It) fournissent de bons conseils qu'il est bon de se souvenir quand on code.

KISS conseille de garder le code et le design aussi simple que possible.

Peu de personne ne serait pas d'accord, mais malheureusement il n'est pas évident de définir *simple*.

Etant donné deux solutions à un problème laquelle est la plus simple?

* Celle avec le moins de lignes de code?
* Celle avec le moins de classes?
* Celle qui nécessite le moins de bibliothèques tierces?
* Celle qui utilise le moins de conditions? //* The one with fewer branch statements?*************//
* Celle où la logique est la plus explicite?
* Celle qui est consistante avec une solution utilisée ailleurs?

Toutes ces définitions sont raisonnable pour définir *simple*. Aucune d'entre elles n'est la définition qu'il est toujours judicieux de suivre.

Reconnaitre que simple n'est pas facile et conserver les choses simples requiert beaucoup de travail.

Si nous pouvions mesurer la complexité de notre logiciel, nous constaterions qu'il y a une valeur minimale que chaque morceau du logiciel doit contenir.

Si le logiciel était plus simple, alors il serait moins fonctionnel. 

Les vrais programmes auront toujours cette *compléxité inhérente* avec un petit plus. Cette compléxité en plus est la *compléxité accidentelle* que nous ajoutons car nous ne sommes pas parfaits.

Distinguer la compléxité accidentelle de la compléxité inhérente est bien sûr difficile.

Heureusement YAGNI nous donne des conseils utiles sur comment garder les choses simples sans avoir à distinguer les compléxité accidentelle et inhérente.

Plus un système fait de chosees, plus grande sera sa compléxité. Si nous faisons un système qui fait moins, il sera plus simple - il aura moins de *compléxité inhérente* ainsi que moins de *compléxité accidentelle*.

Votre but est, par conséquent, de créer le nombre de fonctionnalités minimal qui résoud le problème que vous avez **maintenant**.

* N'implémentez de choses dont vous pensez avoir besoin plus tard. Implémentez les dans le futur si vous en avez besoin.
* N'essayez pas de rendre les choses "flexibles" ou "configurables". Faites les faire ce qu'elles doivent faire - paramétrez-les que si vous en avez besoin.

Si vous créez plus que la quantité minimume de fonctionnalités, vous aurez plus de code à debugger, comprendre et maintenir à partir de là jusqu'à ce que quelqu'un est le courage de le supprimer.

