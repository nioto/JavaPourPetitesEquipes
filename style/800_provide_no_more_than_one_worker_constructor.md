## Ne Fournisser qu'un Seul Constructeur Fonctionnel

### Sommaire

Bien qu'une classe puisse fournir plusieurs constructeurs, un seul devrait affecter les champs et initialiser la classe.

### Détails

Avoir un seul endroit où les champs sont assignés durant la construction rend plus facile la compréhension des états que cette classe peut avoir à la construction.

Les classes ne devraient pas avoir plusieurs constructions qui renseignent les champs.

**Mauvais**
```java
public class Foo {
  private final String a;
  private final Integer b;
  private final Float c;

  public Foo(String value) {
    this.a = Objects.requireNonNull(value);
    this.b = 42;
    this.c = 1.0f;
  }

  public Foo(Integer value) {
    this.a = "";
    this.b = Objects.requireNonNull(value);
    this.c = 1.0f;
  }

  public Foo(Float value) {
    this.a = "";
    this.b = 42;
    this.c = Objects.requireNonNull(value);
  }
}
```
La duplication des valeurs dans le code précédent peut être supprimé mais celà resterait confus car l'initialisation de la classe est répartie en trois endroits.

Si d'autres champs venaient à être ajoutés, il serait facile d'oublier de les initialiser dans les constructeurs déjà présents.

Heureusement, nous avons rendu les champs final donc celà donnerait une erreur à la compilation? Si la classe était mutable, nous aurioins un bug à investiguer à l'éxécution.


**Mieux**
```java
public class Foo {
  private final String a;
  private final Integer b;
  private final Float c;

  private Foo(String a, Integer b, Float c) {
    this.a = Objects.requireNonNull(a);
    this.b = Objects.requireNonNull(b);
    this.c = Objects.requireNonNull(c);
  }

  public Foo(String value) {
    this(value, 42, 1.0f);
  }

  public Foo(Integer value) {
    this("", value, 1.0f);
  }

  public Foo(Float value) {
    this("", 42, value);
  }
}
```

Les champs sont maintenant renseignés à un seul endroit, résultant en moins de duplication.

Nous pouvons aussi voir en un coup d'oeil que `Foo` ne peut être construite avec des valeurs null. Dans la version précédente, celà n'aurait pu être vu qu'en parcourant trois endroits différents.

En suivant ce modèle, il est difficile d'oublier de reseigner un champs même s'il est non-final.

