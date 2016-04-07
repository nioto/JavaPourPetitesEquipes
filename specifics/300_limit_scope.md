## Limiter les Variables à la plus petite Portée

### Sommaire

Les variables devraient être déclarées le plus tard possible afin qu'elles est la plus petite portée possible.

### Détails

*Mauvais*

```java
public void foo(String value) {
    String calculatedValue;
    if (someCondition()) {
        calculatedValue = calculateStr(value);
        doSomethingWithValue(calculatedValue);
    }
}
```

*Meilleur*

```java
public void foo(String value) {
    if (someCondition()) {
        String calculatedValue = calculateStr(value);
        doSomethingWithValue(calculatedValue);
    }
}
```

*Encore Meilleur*

```java
public void foo(String value) {
    if (someCondition()) {
        doSomethingWithValue(calculateStr(value));
    }
}
```

Parfois assigner à des variables temporaires bien-nommées rend le code plus lisible que des appels de méthodes imbriquées car celà permet au lecteur de mieux suivre le déroulement du code.

En règle générale, si vous êtes incapable de nommer une variable qui ne fait guère plus que de stocker le résultat d'une méthode, la variable doit être éliminée.

