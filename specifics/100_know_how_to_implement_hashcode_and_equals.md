## Sachez Comment implémenter Hashcode et Equals

### Sommaire

Implémenter `hashCode` et `equals` n'est pas simple. Ne les implémentez pas à moins que ce soit une nécessité. Si vous les implémentez, assurez vous que vous savez ce que vous faites.

### Détails

Il est bien connu que si vous ré-écrivez `override` vous devez aussi ré-écrire la méthode `hashCode` (voire Effective Java article 9). 

Si des objets identiques n'ont pas le même `hashCode` ils se comporteront de façon suprenante s'ils sont placés dans une collection basée sur du hashage comme `HashMap`.

Par "surprenante", nous entendons que votre programme va se comporter de façon incorrecte d'une manière très difficile à déboguer.

Malheureusement, implémenter `equals` est étonnament difficile à faire correctement. Effective Java article 8 consacre 12 pages à discuter de ce sujet.

Le contrat pour `equals` est défini avce précision dans le Javadoc de `java.lang.Object`. Nous ne le répéterons pas ici ou ne répéterons la discution ce celà signifie, celà peut être trouvé dans Effective Java et dans de nombreux pages sur internet. Au lieu de celà, nous allons voir les statégies pour l'implémenter.

Quelque soit la stratégie choisie, il est important que vous écriviez d'abord les tests pour votre implémentation.

Il est facile pour la méthode `equals` de créer des bugs difficiles à diagnostiquer si le code change (càd si des attributs sont ajoutés ou leur type change). Ecrire des tests pour les méthodes `equals` était habituellement une procédure douloureuse et chronophage, mais aujourd'hui des bibliothèques existent pour les rendent triviaux sur les cas classiques (voire FAQ sur les Tests)


### Ne pas faire

C'est la stratégie la plus simple et celle que vous devriez adopter par défaut dans l'intérêt de conserver son socle petit.
La plupart des classes ne nécessitent pas une méhode `equals`. A moins d'un cas particulier, il n'y a guère de sens à comparer deux objets, ainsi rester avec l'implémentation héritée de `Object`.

Une zone grise irritante existe lorsque le code en production n'a pas besoin de faire de comparaison d'égalité mais que le code des tests a ce besoin. Le dilemme est de savoir s'il faut implémenter ces méthodes purement pour le bien des tests ou de compliquer le code des tests avec des contrôles d'égalité. 

Il y a, bien sûr, pas de bonne réponse ici; nous vous conseillons d'abord d'essayer 
##############################################
There is, of course, no right answer here; we would suggest first trying the compare-it-in-the test approach before falling back to providing a custom equals method. 
##############################################

Les contrôles d'égalité personnels peuvent être proprement partagés en implémentant une assertion personnalisée en utilisant par une bibliothèque telle que AspectJ ou Hamcrest.
##############################
The custom equality checks can be cleanly shared by implementing a custom assertion using a library such as AssertJ or Hamcrest.
##############################

Effective Java suggère timidement d'avoir votre classe qui lève une erreur si la méhode `equals` est appelée de façon inopinée.

```java
@Override public boolean equals(Object o) {
  throw new AssertionError(); // Method is never called
}
```

Celà semble une bonne idée, mais, malheureusement, celà déboussolera la plupart des outils d'analyse statique. Dans l'ensemble, il crée plus de problèmes qu'il n'en résout. 


### Generation Automatique par 'IDE

La plupart des IDE fournit une méthode de génération automatique des méthodes `hashCode` et `equals`? C'est une approche facile d'accès, mais le code généré est (en fonction de l'IDE et de sa configuration) souvent laid et complexe comme ceux générées par Eclipse montré ci-dessous: 

```java
  @Override
  public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + ((field1 == null) ? 0 : field1.hashCode());
    result = prime * result + ((field2 == null) ? 0 : field2.hashCode());
    return result;
  }
```

```java
  @Override
  public boolean equals(Object obj) {
    if (this == obj)
      return true;
    if (obj == null)
      return false;
    if (getClass() != obj.getClass())
      return false;
   MyClass  other = (MyClass) obj;
    if (field1 == null) {
      if (other.field1 != null)
        return false;
    } else if (!field1.equals(other.field1))
      return false;
    if (field2 == null) {
      if (other.field2 != null)
        return false;
    } else if (!field2.equals(other.field2))
      return false;
    return true;
  }
```

A moins que votre IDE peut être configuré pour produire des méthodes claires (comme discuté ci-dessous) nous ne recommandons pas cette approche. Il est facile d'introduire des bugs dans ce code au fil du temps.


### Hand Roll Clean Methods

Java 7 a introduit la classe `java.util.Objects` qui rend l'implémentation de `hashCode` à la portée de tous. Guava fournit la classe similaire `com.google.common.base.Objects` qui peut être utilisé pour les versions de Java antérieures.

```java
  @Override
  public int hashCode() {
    return Objects.hash(field1, field2);
  }
```

La classe `Objects` a également simplifié l'implémentation d'`equals` en faisant les tests de nullité dans la méthode `Objects.equals`.


```java
  @Override
  public boolean equals(Object obj) {
    if (this == obj) // <- performance optimisation
      return true;
    if (obj == null)
      return false;
    if (getClass() != obj.getClass()) // <- see note on inheritance
      return false;
    MyClass other = (MyClass) obj;
    return Objects.equals(field1, other.field1) &&
        Objects.equals(field2, other.field2);
  }
```

La première instruction `if` n'est pas nécessaire et peut être omise sans risque; elle peut, cependant, améliorer la performance. 

D'habitude, nous ne recommandons pas que de telles micro-optimisations soient faites à moins qu'il n'a été prouvé qu'elles fournissent un certain avantage. Dans le cas de la méthode `equals`, nous suggérons que l'optimisation soit présente. Elle se justifiera pour certaines de vos classes et il est utile d'avoir un modèle identique pour toutes les méthodes.

L'exemple précédent utilise la méthode `getClass` pour vérifier que les objets sont de même type. Une alternative est d'utiliser `instanceof` comme suit:


```java
  @Override
  public boolean equals(Object obj) {
    if (this == obj) 
      return true;
    if (obj == null)
      return false;
    if (!(obj instanceof MyClass)) // <- compare with instanceof 
      return false;
    MyClass other = (MyClass) obj;
    return Objects.equals(field1, other.field1) &&
        Objects.equals(field2, other.field2);
  }
```

Celà resulte en une différence de comportement - comparer les instances  de `MyClass` avec ses sous-classes retournera `true` avec `instanceof` et `false` avec `getClass`. 

Dans Effective Java, Josh Block soutient l'usage de `instanceof` car l'implémentation avec `getClass` enfreint la stricte interprétation du principe de substitution de Liskov.

Toutefois, si `instanceof` est utilisé, il est facile pour la propriété de symétrie du contrat de `equals` d'être enfreinte si une sous-classe la redéfinie, i.e.: 

```java
MyClass a = new MyClass();
ExtendsMyClassWithCustomEqual b = new ExtendsMyClassWithCustomEqual();

a.equals(b) // true
b.equals(a) // false, a violation of symmetry
``` 

Si vous vous trouvez dans une situation où vous vous demandez si certaines sous-classes peuvent être égales à leur parent, alors nous vous suggérons fortement de revoir votre design.

Devoir réfléchir à maintenir le contrat de `equals` dans une hiérarchie de classes est douloureuse et vous ne devriez pas vous mettre, ou votre équipe, dans une une telle situation.

Dans la plupart de cas, si vous pensez devoir implémenter `hashCode` et `equals` dans une classe, nous vous suggérons fortement la déclarer `final` poue éviter tout problème dans sa hiérarchie.

Si vous croyez être dans un cas où les classes doivent être traitées comme équivalentes à leur parent, utilisez `instanceof` mais assurez-vous que la classe parente définisse sa méthode `equals` `final`.

Evitez les relations plus complexes que celles-ci.


### Commons EqualsBuilder and HashCodeBuilder

Les hashcode et equals Builders de la bibliothèque Apache Commons furent, un temps, populaires. Nous ne recommandons pas leur usage dans du code neuf car les fonctionnalités qu'ils fournissaient sont fournies par `java.util.Objects` sans ajout de bibliothèques tierces, ou par leur équivalent dans Guava.

Ces classes utilisent la réflection et fournissaient un code court.

```java
public boolean equals(Object obj) {
  return EqualsBuilder.reflectionEquals(this, obj);
}
```

```java
public int hashCode() {
  return HashCodeBuilder.reflectionHashCode(this);
}
```

La concision de ces implémentations est attractive, mais leur performance est faible comparée aux autres implémentations discutées. Les prendre en première implémentation peut être une approche raisonnable, mais en général nous suggérons de les éviter.

### Générateurs de Code 

Un nombre de projets existent qui peuvent générer des objets valeur à la compilation. Deux des plus connus sont :

* [Google auto](https://github.com/google/auto/tree/master/value)
* [Project Lombok](https://projectlombok.org/)

Mais il en existe beaucoup d'autres.

#### Google Auto

Google *Auto* va prendre une classe abstraite annotée avec `@AutoValue` et créer une sous-classe avec le code nécessaire à son fonctionnement. Ce code incluera les méthodes `equals` et `hashCode`;

```java
import com.google.auto.value.AutoValue;


@AutoValue
abstract class Animal {
  static Animal create(String name, int numberOfLegs) {
    return new AutoValue_Animal(name, numberOfLegs);
  }

  Animal() {}

  abstract String name();
  abstract int numberOfLegs();
}
```

Cela demande clairement beaucoup moins d'effort que d'écrire la classe `Animal` à la main, mais il y qualeques inconvénients.

Certains problèmes de la génération de code sont disutées dans le paragraphe "Envisager les générateurs de code avec précaution", qui les classera entre *friction* et *surprise*.

Ici, Google auto introduit de la *friction* dans le sens où le code ci-dessus ne compilera pas dans un IDE tant quel generateur ne se sera pas exécuté pour produire la classe `AutoValue_Animal`.

Il y a aussi des *surprises*.

Parce qu'elle représente objet valeur, la classe Animal devrait normalement être implémentée en tant que classe `final` - mais on nous impose de la rendre abstraite. L'équipe derrière *Auto* préconise de le rendre package-private pour éviter d'autres classes de l'étendent.

Contrairement à du code Java classique, l'ordre dans lequel des accesseurs sont déclarés est important car cet oredre est utilisé par le générateur pour définir l'ordre des paramètres du constructeur. Les ré-ordonner peut, par conséquent, avoir l'effet inattendu d'introduire un bug.


#### Lombok

Lombok peut aussi (entre autre) générer des implémentations complètes d'objet valeur.

Il a une approche différent de Google Auto.

En partant d'une classe annotée de la sorte :

```java
@Value 
public class ValueExample {
  String name;
  @NonFinal int age;
  double score;
}
```

Il va la transformer à la compilation pour produire l'implémentation suivante :

```java
public final class ValueExample {
  private final String name;
  private int age;
  private final double score;

  public ValueExample(String name, int age, double score) {
    this.name = name;
    this.age = age;
    this.score = score;
   }
   
  public String getName() {
    return this.name;
  }
   
  public int getAge() {
    return this.age;
  }
   
  public double getScore() {
    return this.score;
  }
   
  public boolean equals(Object o) {
   // valid implementation of equality based on all fields
  }

  public int hashCode() {
   // valid hashcode implementation based on all fields
  }
```

Alors que Google *Value* demande à l'utilisateur de fournir une API publique valide pour sa classe, *Lombok* crée l'API publique basée via la description de ses éléments internes. La description est syntaxiquement valide en Java mais a une signification différente lorsqu'elle sera interprétée par Lombok.

*Lombok* cause des *frictions*. il n'est pas pratique d'utiliser *Lombok* sans un IDE qui le prend en compte - un code utilisant l'API générée ne sera pas valide. Un plugin doit être installé dans l'IDE.

Bien qu'il introduise moins de friction une fois le plugin installé dans l'IDE, le comportement de *Lombok* est beaucoup plus surprenant. Il est facile d'expliquer ce que *Auto* fait - il génère une classe à la compilation qui implémente une interface que nous avons définie. Il est beaucoup difficile de prédire ou expliquer ce que *Lombok* fait.

Bien que *Lombok* nécessite d'écrire moins de code que des solutions telles que *Auto*, il dévie plus du code Java normal.

Si vous pensez utiliser une générateur de code pour des classes d'objet valeur, nous vous recommandons l'approche *Auto* plutôt que *Lombok*.

Ajoutons que *Lombok* fournit un plan de secours (voire "Préférer des décisions réversibles") en l'outil delombok qui permet de produire les sources des classes générées. Ces sources peuvent être utilisées pour templacer les originaux contenant les annotations.

Supprimer *Auto* est aussi simple - les classes générées peuvent être ajoutées au dépôt de sources. Les sources des classes abstraites ou leur implémentation peuvent être fusionnées.
