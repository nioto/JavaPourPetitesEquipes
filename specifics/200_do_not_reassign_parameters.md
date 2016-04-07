## Ne pas Ré-Affecter les Paramètres

### Sommaire

Les paramètres des méthodes ne doivent jamais être affectés.

### Détail 

Ré-affecter les paramètres rend le code plus difficile à comprendre et ne produit aucun avantage par rapport à créer une nouvelle variable.

Si la méthode est grande, il peut devenir difficile de tracer le cycle de vin d'un paramètre. Même avec des méthodes courtes, ré-utiliser des paramètres peut causer des problèmes. Comme la variable est utilisée pour représenter deux concepts séparés, il est soucent impossible de le nommer de manière significative.

Si une autre variable du même type que le paramètre est nécessaire, elle devrait être déclarée localement.


**Mauvais**

```java
public String foo(String currentStatus) {
  if ( someLogic() ) {
    currentStatus  = "FOO";
  }
  return currentStatus;
}
```

**Meilleur**

```java
public String foo(final String currentStatus) {
  String desiredStatus = currentStatus;
  if ( someLogic() ) {
    desiredStatus = "FOO";
  }

  return desiredStatus ;
}
```

Les paramètre devraient être déclarés comme *final* ainsi le lecteur pourra immédiatement se rendre compte que sa valeur ne change pas.
