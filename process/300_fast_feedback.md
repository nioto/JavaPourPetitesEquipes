## Retour Rapide

## Le Fichier de Build détient la Vérité

Le script de build fournit un retour moins immédiat que l'IDE parce qu'il doit être déclenché.

Toutefois, le retour par le script de build a deux avantages majeurs:

* Il est reproductible à travers toutes les machines
* Avec l'aide d'un serveur d'intégration continue, vous pouvez vous assurez qu'il ne sera pas ignoré

Parce qu'un retour plus lent est acceptable depuis un script de build, un plus large éventail d'analyses statiques et dynamiques peut y être exécuter. Celà incluera généralement les résultats du retour instantané.


## Tests Unitaires Localement Exécutables

Après le compilateur et l'analyse statique, les plus rapides niveaux de retours sont les tests unitaires.

Au moins deux jeux devraient être maintenus qui seraient localement exécutables sur la machine de tout développeur.

Parce qu'ils sont générallement exécutés juste après la compilation ou avant le commit/push de code, Martin Fowler fait références à eux par :

* Le jeu de compilation
* Le jeu de commit

Dans Maven, ils sont naturellement référencés par les phases `test` et `integration-test`.

Le critère pour qu'un test soit placé dans le jeu de compilation devrait, néanmoins, être plus que **juste** sa vitesse d'exécution.

Ils doivent être très rapides (millisecondes ou moins par test) mais doit aussi être hautement déterministe et reproductible. Celà garantit que le jeu fournisse un retour propre - la seule raison qui ferait qu'un test doive échouer après un changement de code serait que le changement a causé une régression.

Bien que celà semble simple, en pratique celà exige beaucoup de rigueur pour s'assurer que les tests n'interfèrent pas les uns avec les autres ou soient affectés par des facteurs externes.

Les tests dans le jeu de commit peuvent être plus long et aussi légèrement moins répétables. Ils devraient **viser** le 100% répétable mais ils peuvent effectuer des tâches qui ajoutent des risques occasionnels d'échec, tels que la connectivité réseau ou l'écriture sur disque.

Bien que beaucoup de tests dans ce jeu ne font pas plus que lancer du code dans la même JVM que les tests eux-mêmes, certains tests devraient lancer la création de l'arifact (war, ear, jar) et effectuer au moins un certain nombre de tests dessus.

Bien que le jeu de commit est surement dépendant de ressources externes tels que des containers, des bases de données, des queues, etc., il devrait est exécutable via une seule commande.

Installer et démarrer les ressources dépendantes devraient être gérer par les scripts de builds et de tests - votre projet ne devrait pas contenir une page de notes sur comment mettre en place une machine de développement. 

Couramment, le plugin Cargo de Maven est utilisé pour télécharger et configurer des containers pour tester.
