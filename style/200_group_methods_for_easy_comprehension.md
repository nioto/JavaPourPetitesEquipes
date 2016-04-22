## Grouper les Méthodes pour une Meilleure Compréhension

### Sommaire

Les méthodes publiques d'une classe devraient apparaitre en haut du fichier, les méthodes privées en bas et les autres méthodes (protected et package default) entre les deux.

En plus d'être triées par visibilité, elles devraient être ordonnées par un flux logique.

### Détail 

Ce plan veut atteindre deux objectifs:

1. Mettre en valeur l'API publique en la séparant du détail de son implémentation
2. Permettre au lecteur de suivre le cours du code avec un minimal de défilement de code

Pour atteindre ce deuxième but, les méthodes doivent être arrangées dans des groupes logiques, avec les méthodes appelantes au dessus de celles qu'elles appellent.

Ces deux objectifs entrent clairement en conflit car grouper les méthodes de l'API publique en haut de fichier interdit de les rapprocher des méthodes qu'elles utilisent. Si cela posse un gros problème, c'est une indication que la classe a trop de responsabilités et devrait être refactorisée en plusieurs classes de moindre taille.

Des questions se poseront sur l'emplacement "correct" d'une méthode si elle est appelée depuis plusieurs endroits ou a des relations récursives. Il n'y a pas, bien sûr, de réponse unique et l'ordre qui suit le second objectif devrait être privilégié.

Les constructeurs et les fabriques statiques devraient être placés en début de classe. Le fait qu'une classe est statique ne devrait pas influencé sont enplacement dans le code.


**Exemple**

```java
public class Layout {

  private int a;

  Layout() {...}
  
  public static Layout create() {...}
  
  public void api1() {
    if (...) {
      doFoo();
    }
  }
  
  public void api2() {
    if(...) {
      doBar();
    }
  }
  
  private void doFoo() {
    while(...) {
      handleA();
      handleB();
    }
    leaf();
  }
  
  private void handleA() {...}
  
  private void handleB() {...}
  
  private static void doBar() {
    if (...) {
      leaf();
    }
  }
  
  private void leaf() {...}  
}

```

Les champs devraient toujours être placé au sommet de la classe avant toute méthode.
