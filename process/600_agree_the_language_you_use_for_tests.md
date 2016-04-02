## Convenir du Langage que vous Utilisez pour les Lests

### La Pyramide des Tests

Malheureusement, le langage utilisé pour les tests est surchargé, avec différentes communités se référant a des choses différentes utilisant les mêmes noms.

La pyramide des tests est un diagramme largement reconnu de la façon dont les tests doivent être abordés.

Il montre un grand nombre de tests unitaires en bas, avec un plus petit nombre de tests unitaires au dessu et encore un plus petit nombre de tests système à sa pointe. Souvent, des nuages de tests manuels sont ajoutés au sommet.


![La pyramide des tests](../generated/images/svg/pyramid.png)

Ce diagramme a sans doute été dessiné des milliers de fois. Bien que les tests unitaires apparaissent à la base de caque version, le vocabulaire aux autres niveaux peut varier considérament.

Bien que les mêmes mots soit utilisés leur sens peut différer.

Bien que les gens peuvent aquiescer quand vous discutez "tests unitaires", "tests d'intégration", "tests système", "tests de bout à bout", "tests de service", il n'y a pas de garantie qu'ils pensent la même chose que vous.

En fonction de votre interlocuteur, un "test unitaire" peut être n'importe quoi depuis un document word pleine d'instructions, "tout test écrit par un développeur", via diverses définitions formelles (mais en aucun cas faisant autorité) qui apparaissent dans les livres.

Le nombre de sens possibles pour "tests d'intégration" est encore plus grand.

### Les Tests Unitaires

Une définition assez complète d'un test unitaire est largement utilisé dans la communité Java. Nous préconisons que vous et votre équipe utilise cette définition.

Pour être un test unitaire, un test doit être :

* Rapide (millisecondes ou moins)
* Isolé (ne tester qu'une chose)
* Répétable (capable d'être exécuté des millions de fois sur toute machine avec le même résultat)
* Auto vérifiant (ça passe ou àa échoue)
* En temps et en heure (écrit en premier)


Note: Bien qu'écrire vos test en premier est souvent ujne bonn idée, un test qui vérifie les autres critères est quand même un test unitaire indépendamment de quand il a été écrit.



When we talk about "unit" testing, what constitutes a *unit* isn't necessarily that obvious.

A somewhat circular definition is that a *unit* is the smallest thing makes sense to test independently. 

It will often be a single class, but this is not necessarily the case. It may make sense to treat a group of classes as a unit (particularly if most of them are non-public) or occasionally even a method.

If we accept that a *unit* is a small thing, and that we'll know it when we see it, then we can see that the criteria for being a unit test largely matches the criteria we put forward for the compile suite. 

The only difference is that the compile suite does not care about isolation.

If we choose to write a test that tests two (or more) *units* in tandem, it still belongs in the compile suite if it meets the other criteria.

### System Tests

System tests are also fairly well-defined. They are tests that exercise the overall system - i.e all your code and all the code it interacts with in a realistic environment.

### Integration Tests

Integration tests are harder to define. They occupy the large space of everything that doesn't fit the unit or system tests definitions.

The two following diagrams show how this terminology fits into our world of test suites. 

This document will use the terminology *unit test*, *Integration test* and *System test* as shown in these diagrams. 

For clarity, it will sometimes state exactly what is being tested when discussing integration tests - e.g "test via the REST API of the war file running in Tomcat". 

Although it is tedious, this long-hand terminology is clear. It is recommended that you use it when discussing testing across teams. Within your own team it is likely you will develop a shorter language you all understand.

![Properties of different test types](../generated/images/svg/test_types.png)

This maps to our suites as shown below:

![Test suites](../generated/images/svg/test_types_maven.png)
