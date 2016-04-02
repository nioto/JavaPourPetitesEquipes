# Toujours Utiliser un StringBuffer pour Concaténer

Ce conseil est doublement erroné.

D'abord il recommande l'usage de `StringBuffer` qui est synchronisé plutôt que `StringBuilder`.

Ensuite c'est une simplification excessive ou une méprise du conseil plus raisonnable et nuancé de ne pas concaténer de chaînes de caractères dans une boucle.

Éviter la concaténation dans une boucle est raisonnable. Utiliser un `StringBuilder` est susceptible d'être plus efficace si la boucle s'exécute un nombre raisonnable de fois car celà évitera des allocations de chaînes de caractères. 

La différence de performance sera négligeable dans la plupart des cas, mais le code qui en résulte ne sera pas moins lisible - dans ce cas c'est une ptimisation prématurée sans coût.

Voyons ce qui arrive quand on applique cette règle quand aucune boucle n'est présente :

```java
  public String buffer(String s, int i) {
    StringBuilder sb = new StringBuilder();
    sb.append("Foo");
    sb.append(s);
    sb.append(i);
    return sb.toString();
  }

  public String concat(String s, int i) {
    return "Foo" + s + i;
  }
```

La version `concat` est beaucoup plus claire.

Si la version `concat` est la plus claire des deux fonctions, quelle est la plus efficace?

Le compilateur d'Eclipse génère le bytecode suivant pour `concat`:

```
    NEW java/lang/StringBuilder
    DUP
    LDC "Foo"
    INVOKESPECIAL java/lang/StringBuilder.<init> (Ljava/lang/String;)V
    ALOAD 1
    INVOKEVIRTUAL java/lang/StringBuilder.append /
       (Ljava/lang/String;)Ljava/lang/StringBuilder;
    ILOAD 2
    INVOKEVIRTUAL java/lang/StringBuilder.append (I)Ljava/lang/StringBuilder;
    INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;
    ARETURN
```

Un `StringBuilder` est créé par le compilateur dans les coulisses pour gérer la concaténation donc notre code plus simple et plus clair produit un bytecode identique à la version plus verbeuse.

La présence de boucles dans le code peut prévenir le compilateur d'accomplir cette optimisation, mais le code sans branches sera optimisé à chaque fois. A noter que certains compilateurs peuvent ne pas supporter cette optimisation mais il est peut probable que vous en utilisiez un. 
