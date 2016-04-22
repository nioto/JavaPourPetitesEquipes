## Eviter les Checked Exceptions 

### Sommaire

Ne déclarez pas de checked exceptions à moins qu'il n'y ait un plan d'action clair qui doit être appliqué lorsque l'une d'elle est levée. 

### Détails

Les exceptions sont pour circonstances exceptionnelles - concevez votre code de telle sorte qu'elles ne soient pas levées dans des scénarions qui sont supposés arrivés.

Cela signifie qu'elles ne doivent pas être utilisées pour le flux de contrôle normal.

Les checked exceptions gonflent et compliquent le code. Vous devriez les éviter dans vos API, excepté lorsqu'il y a une action bien définie que l'appelant peut prendre pour récupérer à partir du scénario d'erreur. 

Cela est étonnament rare.

Si vous travaillez avec une bibliothèque qui utilisent des checked exceptions, vous pouvez toujours les encapsuler par une exception Runtime.

Quand vous le faites, assurez-vous de conserver la Stacktrace.

```java
try {
  myObject.methodThrowingException();
} catch (SomeCheckedException e) {
  throw new RuntimeException(e);
}
```

Si vous avez attraper une `Exception` ou un `Throwable`, et n'êtes pas sûr de son type exact, vous pouvez éviter de l'encapsuler inutilement en utilisant `Throwables.propagate` de Guava.

```java
try {
  myObject.methodThrowingException();
} catch (Exception e) {
  throw Throwables.propagate(e);
}
```

Cela encapsulera les checked exceptions et laissera les unchecked exceptions telles quelles.

