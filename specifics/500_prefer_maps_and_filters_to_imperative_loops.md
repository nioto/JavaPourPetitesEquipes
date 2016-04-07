## Préférer les Maps et les Filters aux Boucles

### Commaire

Les boucles dissimule la logique de l'application dans du code verbeux - préférer les maps et filters puisqu'il sépare la logique de l'implémentation.

### Détails

La plupart du code basé sur des boucles peut être réu-écrit dans un style plus décoratif en utilisant des filters et des maps.

Java 8 a rendu celà simple en introduisant les lambdas et l'API Stream, mais le même style peut être appliqué avec Java 7 en utilisant des classes internes anonymes et des bibliothèques tierces telles que Guava.

Les filters et les maps mettent en valeur ce que le code est censé réaliser. Celà est moins clair dans l'implémentation impérative.

**Mauvais**
```java
  public List<String> selectValues(List<Integer> someIntegers) {
    List<String> filteredStrings = new ArrayList<String>();
    for (Integer value : someIntegers) {
      if (value > 20) {
        filteredStrings.add(value.toString());
      }
    }
    return filteredStrings;
  }
```

**Mieux (Java 8)**
```java
  public List<String> selectValues(List<Integer> someIntegers) {
    return someIntegers.stream()
        .filter(i -> i > 20)
        .map(i -> i.toString())
        .collect(Collectors.toList());
  }
```

**Mieux (Java 7 en utilisant Guava)**
```java
  public List<String> selectValues(List<Integer> someIntegers) {
    return FluentIterable
    .from(someIntegers)
    .filter(greaterThan(20))
    .transform(Functions.toStringFunction())
    .toList();
  }

  private static Predicate<Integer> greaterThan(final int limit) {
    return new Predicate<Integer>() {
      @Override
      public boolean apply(Integer input) {
        return input > limit;
      }
    };
  }
```

Noter que, bien que la version Java 7 nécessite plus de lignes de code (sous la forme de code verbeux et affreux pour la classe interne anonyme), la logique de la méthode `selectvalues` est plus claire. Si la logique nécessite que le Predicate ou la Function de mapping soit utilisée à plusieurs endroits alors il sera simple de la placer dans une endroit partagé. Cela sera plus dur à effectuer avec la version impérative.

Noter également que la méthode qui crée le Predicate a été déclarée statique. C'est une bonne idée de faire de la sorte, quand cela est possible, à chaque fois qu'une classe anonyme est nécessaire afin d'éviter d'avoir une instance qui perdure et évite sa classe parent d'être libérée par le garbage collector.
Bien que le Predicate soit de courte durée dans notre cas, utiliser static dogmatiquement dans tous les cas évite de trop réfléchir.
