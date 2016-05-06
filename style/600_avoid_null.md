## Eviter Null Aussi Souvent que Possible

### Sommaire

Null est une erreur à un milliard de dollars, assurez-vous de savoir comment éviter de l'utiliser dans votre code.

Essayez de limiter le nombre de fois où vous ou vos clients doivent écrire ceci:


```java
  if ( != null ) {
    ...
  }
```
### Détails

Bien qu'il soit certain que les bibliothèques et les frameworks avec lesquels vous intéragissez retournons null, vous devriez essayer de faire en sorte de celà soit isolé dans du code tiers.

Le coeur de votre application devrait supposer qu'il n'a pas à se soucier des valeurs null.


Les stratégies pour éviter null comprennent:

* Le modèle Null Object - quand vous avez quelque chose que vous pensez être facultatif
* Le modèle Type-safe Null Object  (càd Option, Facultatif et Peut-être) - quand vous avvez besoin d'exprimer qu'une interface peut ne pas retourner quelque chose
* Conception par contrat


### Le modèle Null Object

Le modèle null object est l'approche classique en OO d'éviter null. Vous devriez l'utiliser quand vous pensez avoir un dépendance que vous pensez optionnelle.

Le modèle est très simple, il suffit de fournir une implémentation de l'interface qui ne fait "rien" ou à un effet neutre. Elle peut être sans risque référencée par ses clients, mais inutile de vérifier si elle est null.


### Type-Safe Nulls (Optional)

Le modèle Type-safe null est connu dans la plupart des langages de programmation fonctionnelle où il est connu sous les noms Peut-être, Option ou Facultatif. Java 8 a enfin introduit un type Optional, mais des implémentations sont disponibles pour les versions précédentes via Guava ou d'autres bibliothèques.

C'est un modèle simple. Un Optional est simplement une boîte qui peut contenir soit une soit zéro valeurs. Vous pouvez vérifier si la boite est vide (en utilisant `isPresent`) et récupérer sa valeur via une méthode get.

Optional devrait être utilisé quand une méthode publique peut ne pas retourner une valeur dans le cours normal d'un programme.

Si vous appelez get sur un Optional vide, celà lèvera l'exception `NoSuchElementException`.

Il n'est pas flagrant de voir ce qu'apporte une valeur Optional sur l'usage de null. Si vous avez besoin de vérifier si un Optional a une valeur avant d'appeler `get`, en quoi celà est différent de vérifier si la valeur est null pour éviter un `NullPointerException`?

Il y plusieurs différences importantes.

Premièrement, si votre méthode déclares qu'elle retourne un type `Optional<Person>` alors vous pouvez voir instantanément depuis cette signature qu'elle peut ne pas retourner de valeur. Si elle ne retournait que le type `Person`, vous ne sauriez qu'elle peut retourner null qu'en parcourant la documentation, les tests ou la documentation.

De même importance, si vous savez que l'usage de `Optional`est utilisé lorsque quelque chose peut ne pas être présent, alors vous savez instantanément que si vous retournez `Person` alors la valeur ne sera jamais null.

Enfin, la meilleure façon d'utiliser Optionals est de ne pas appeler la méthode get ou d'explicitement vérifier qu'il contient une valeur. Les valeurs qui sont contenues (ou pas) dans un Optional peut être de façon sure mappées, consommées et filtrées par diverses méhodes de la classe.

Dans des cas simples, une possibilité est donnée pour les Optional vides d'être accédés par l'appel de la méthode `orElse` qui prend une valeur par défaut à renvoyer si l'Optional est vide.

Comme indiqué, la meilleure façon d'utiliser les Optional est pour le type retour des méthodes. Ils ne devraient pas être utilisés comme champs (utilisez le Null Pattern à la place) ou passés en paramètres de méthodes publiques (à la place, fournissez des versions qui ne nécessitent pas de paramètres).

Une objection qui revient quelques fois de la part des programmeurs Java qui découvre Optional, est qu'il est possible pour un Optional d'être null. Bien que celà soit vrai, retourner un Optional null pour une méthode est une chose perverse à faire et devrait être considéré comme une erreur de coding.

Des règles d'analyses statistiques existent pour vérifier si le code retourne des Optionals null.

### Conception par Contrat

Nous aimerions que tout le code que nous controlons puisse ignorer l'éxistence de null ( à moins qu'il ne s'interface avec des bibliothèques tierces qui nous forcent à le prendre en compte)

`Objects.requireNonNull` peut être utilisé pour ajouter une assertion à l'éxécution afin de s'assurer que null ne soit pas passer en paramètre à une méthode.

Parce que votre code ne devrait jamais supposer que null ne sera jamais présent, il y une plus-value en documentant ce comportement par des tests; les assertions ajoute de la valeur car elles assurent que l'erreur apparaitra proche du point où a été faite la faute.

Nous pouvons aussi tester ce contrat à la compilation.

JSR-305 fournit des annotations qui peuvent être utilisées pour déclarer où null est acceptable.

Bien que cette spécification soit en sommeil, et il n'y pas de signe qu'elle sera incorporée dans Java dans un futur proche, les annotations sont disponibles dans maven via :-

```xml
<dependency>
    <groupId>com.google.code.findbugs</groupId>
    <artifactId>jsr305</artifactId>
    <version>3.0.1</version>
</dependency>
```

Elles sont supportées par plusieurs outils d'analyse statique tels que ;-

* [Findbugs](http://findbugs.sourceforge.net/)
* [Error Prone](http://errorprone.info/)

Ces derniers peuvent être configurés pour casser le build quand null est passé en tant que paramètre alors qu'on ne l'y attend pas.

Annoter toutes les classes, méthodes ou paramètres avec `@Notnull` deviendrait rapidement fastidieux et le problème serait de savoir si le gain vaut la quantité de bruit introduit.

Heureusement, il est possible de rendre `@Notnull` le comportement par défaut en annontant le package dans le fichier package-info.java comme ceci:

```java
@javax.annotation.ParametersAreNonnullByDefault
package com.example.somepackage ;
```

Malheureusement, les sous packages n'héritent pas des annotations de leur parent, donc un fichier package-info.java doit être créé pour chaque package.

Une fois que le comportement par défaut est de n'avoir que nes paramètres non null, tout paramètre qui doit supporter null peut être annoté par `@Nullable`.

