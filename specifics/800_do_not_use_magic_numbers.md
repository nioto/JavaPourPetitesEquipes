## N'Utilisez pas de Nombres Magiques

### Sommaire

Les nombres magiques devraient être remplacée par des constantes correctement nommés suivant leur signification.

#### Détail 

Mettre des chaines de caractères ou des nombres directement dans le code source provoque deux problèmes:

1. Il est peu probable que la **signification** de la valeur soit claire
2. Si la valeur change, des modifications sont nécessaires partout où ces valeurs ont été dupliquées

Les valeurs devraient être placées dans des constantes correctement nommées et des Enums.

**Mauvais**
```java
public void fnord(int i) {
  if (i == 1) {
    performSideEffect();
  }
}
```

**Mieux**
```java
public void fnord(int i) {
  if (i == VALID) {
    performSideEffect();
  }
}
```


**T'as pas tout compris**
```java
public void fnord(int i) {
  if (i == ONE) {
    performSideEffect();
  }
}
```

Si les constantes ainsi extraites ont un rapport avec un concept identifiable, créez plutôt un Enum:

**Bon**
```java
public void fnord(FnordStatus status) {
  if (status == FnordStatus.VALID) {
    performSideEffect();
  }
}
```

Des normes de codage rendent des déclarations telles "0 et 1 sont des exceptions à la règle". C'est, cependant , une simplification excessive.

Parfois 0 et 1 auront un sens local très clair en étant utilisés dans du code de bas niveau, par ex:

```java
  if (list.size() == 0) {...}
```

Mais 0 et 1 peuvent aussi est des valeurs spécifiques au domaine qui devraient être placées dans des constantes comme les autres valeurs.

Du code Java côté serveur peut souvent être ré-écrit d'une façon plus claire sans utilisé de valeurs litérales, par ex:

```java
  if (list.isEmpty()) {...}
```
