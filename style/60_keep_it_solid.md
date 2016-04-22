## Rester SOLID

### Sommaire

L'acronyme SOLID fournit l'orientation à suivre pour votre conception.

* **S**ingle Responsibility Principle - Principe de la responsabilité unique
* **O**pen Closed Principle - Principe Ouvert-Fermé 
* **L**iskov Substitution Principle - Principe de substitution de Liskov
* **I**nterface Segregation Principle - Principe de ségrégation des interfaces
* **D**ependency Inversion Principle - Principe d'inversion de dépendance

### Détails

#### Principe de la Responsabilité Unique

Séparez vos problèmes - une classe devrait faire une chose et une seule. Formulé différamment, une classe doit avoir une unique raison de changer.


#### Principe Ouvert / Fermé

Vous devez être capable d'étendre son compartement, sans avoir à modifier le code existant.

*".. vous devez concevoir des modules qui ne changent jamais. Quand les besoins changent,vous étendez le comportement de tels modules en ajoutant du code, pas en changeant l'ancien code qui fonctionne."*

*— Robert Martin*

Une indication que vous pourriez ne pas suivre ce principe est la présence d'instruction `switch`ou de `if/else` dans votre code.

#### Principe de Substitution de Liskov

Les classes dérivées doivent être substituables par leurs classes de base.

Une indication que vous violez ce principe est la présence d'instructions `instanceof` dans votre code.

#### Principe de Ségrégation des Interfaces

Le principe de ségrégation des interfaces stipule que les clients ne devraient pas être forcés d'implémenter des interfaces qu'ils n'utilisent pas; préférer de petites interfaces, taillées sur mesure, qu'une grosse fourre-tout.

Une indication que vous pourriez enfreindre ce principe est la présence de méthodes vides ou de méthodes qui lèvent l'exception `OperationNotSupportedException` dans votre code.

#### Principe d'inversion de dépendance


Les modules de haut-niveau ne devraient pas dépendre de modules de bas niveau. Les deux doivent dépendre d'abstractions.

Les abstractions ne devraient pas dépendre des détails. Les détails doivent dépendre des abstractions.

Dans la pratique, cela signifie que vous devez suivre l'un des deux modèles:

1. Packager les interfaces dont dépend un composant 'haut niveau' avec ce composant
2. Packager l'interface dont dépend un composant séparément du client et de son implémentation

Cette première approche est l'inversion de dépendances classiques (au contraire de l'approche traditionnelle où le composant haut-niveau dépend des couches basses).

La seconde approche est connue sous le nom de "Separated Interface Pattern" (Patron d'Interface Séparée. Il est un peu plus lourd, mais aussi plur flexible dans la mesure où il ne fait pas d'hypothèse sur qui devrait posséder l'interface.

L'indication que vous violez ce principe est la présente d'une dépendance circulaire dans des packages de votre code.

