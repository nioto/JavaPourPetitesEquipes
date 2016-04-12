## Attention aux Casts et aux Generics Warnings

### Sommaire

Les casts diminue le bénéfice du système de type de Java, ce qui rend le code moins lisible et moins sûr.

Evitez les casts quand c'est possible.

Si vous pensez en écrire un, faites une pause et demandez-vous pourquoi vous en avez besoin.

Qu'est-ce que vous devriez changez dans votre code pour ne pas devoir écrire ce cast?

Pourquoi ne pas faire ce changement?

### Détail

Le système de type de Java est là pour nous aider - il détecte les bugs à la compilation et documente notre code, ce qui le rend plus facile à comprendre et parcourir.

Quand nous ajoutons un cast dans notre code, nous perdons ces deux bénéfices.

Les casts ont été introduits dans le code pour 3 principales raisons:

1. Nous sommes arrivé aux limites du système de type de Java et le programmeur doit prendre le controle
2. Le design globale du code est mauvais
3. Le code utilise des types génériques Raw

Nous allons voir cela par ordre inverse.


#### Code avec des Types Raw

Si le code contient des types génériques Raw (que le code date d'avant Java 5 ou que le programmeur ne soit pas familier avec Java) alors celà créera le besoin d'avoir des casts.

Par exemple:

```java
List list = numberList();
for (Object each : list) {
  Integer i = (Integer) each;
  // do things with integers
}
```

Le compilateur ne sera pas très content que nous avons omis de déclarer pleinement le type de `List` que nous utilisons et générera (en fonction de comment il a été configuré) une erreur ou un warning sur la ligne ou `list` est déclarée:

```
List is a raw type. References to generic type List<E> should be parameterized
```

De même, pour du code tel que :

```java
List l = new ArrayList<Number>();
List<String> ls = l;
```

Le compilateur va émettre: 

```
Type safety: The expression of type List needs unchecked conversion to conform to `List<String>`
```

Assurez-vous que tous les warnings de type soient traités, soit en imposant au compilateur de ne pas lister les warnings ou en le configurant pour les traiter en erreurs.

Dans ce cas, supprimer et le cast et le warning devient simple :

```java
List<Integer> list = numberList();
for (Integer each : list) {
   // do things with each
}
```

#### Mauvais Design

Parfois, supprimer le cast ou traiter un warning n'est pas simple. Passons au problème deux - mauvais design. 

Par exemple:

```java
List<Widget> widgets = getWidgets();
List results = process(widgets);
    
for (Object each : results) {
  if (each instanceof String) {
    // handle failure using data from string
  } else {
    EnhancedWidget widget = (EnhancedWidget) each;
    widget.doSomething();
  }
}
```

En principe, les objets placés dans une collection devraient être d'un type unique ou de différents types mais partageant une classe commune ou une interface.

Ici, des types non apparentés sont placés dans une même list avec une String utilisée pour communiquer une sorte d'information indiquant comment "procéder" dans le cas où un Widget n'a pas pu être traité.

La correction classique -en orienté objet- pour ce code est d'introduire une interface `ProcessResult` avec deux implémentations.

```java
interface ProcessResult {
 void doSomething(); 
}

class Success implements ProcessResult {
  
  private final EnhancedWidget result;
  
  @Override
  public void doSomething() {
    result.doSomething();
  }
  
}

class Failure implements ProcessResult {
  
  private final String result;
  
  @Override
  public void doSomething() {
    // do something with result string
  }
  
}
  
```

Le code original peut être corrigé en:

```java
List<Widget> widgets = getWidgets();
List<ProcessResult> results = process(widgets);
    
for (ProcessResult each : results) {
    each.doSomething();
  }
}
```

Ou, de façon plus concise en Java 8:

```java
 List<ProcessResult> results = process(widgets);
 results.stream().forEach(ProcessResult::doSomething); 
```

Il peut être parfois judicieux d'utiliser un type d'union disjointe soit `Either`.

Cette technique peut être particulièrement utile dans un premier pas vers une ré-écriture de code hérité qui mixe des collections de types bruts, mais peut aussi être une approche possible lorsqu'on traite avec des cas d'erreurs.

Malheureusement, Java ne fournit pas un type `Either` de base mais il est simple à implémenter, par exemple :

```java
public class Either<L,R> {
  private final L left;
  private final R right;
  
  private Either(L left, R right) {
    this.left = left;
    this.right = right;
  }

  public static <L, R> Either<L, R> left(final L left) {
    return new Either<L, R>(left,null);
  }

  public static <L, R> Either<L, R> right(final R right) {
    return new Either<L, R>(null,right);
  }

  boolean isLeft() {
    return left != null;
  }

  L left() {
    return left;
  }

  R right() {
    return right;
  }
  
}
```

Des bibliothèques telles que Fugue d'Atlassian fournissent des implémentations bien pluys riches en fonctionnalité.

En utilisant la form simpliste de `Either` avec Java 7, le code serait ré-écrit de la sorte:

```java
List<Widget> widgets = getWidgets();
List<Either<ProcessResult,String>> results = process(widgets);
    
for (Either<ProcessResult,String> each : results) {
  if (each.isLeft()) {
    // handle failure using data from string
  } else {  
    each.right().doSomething();
  }
}
```

Alors que la plupart des programmeurs Java préféreront la version OO précédente, cette version a deux avantages:

1. Elle ne nécessite pas de changement dans la *structure* du code original - tout ce que nous avons fait est rendre les types plus explicites.
2. elle nécessite moins de code

Ce pattern peut aider à rapidement apprivoiser une base de code existant qui serait difficile à comprendre.

#### Les Limites du Système de Type

Parfois nous atteignons les limites du système de types de Java et nous avons besoin de faire des cast.

Avant de procéder ainsi, nous devons être certain que le cast est sûr et qu'il n'y a pas demeilleure solution à notre problème.

De même, parfois nous avons besoin de supprimer un Generics warning, cela peut être fait en annotant avec `@SuppressWarnings` par ex. 

```java
@SuppressWarnings("unchecked")
<T> T read(final Class<T> type, String xml) {
  return (T) fromXml(xml);
}

Object fromXml(final String xml) {
  return ... // de-serialise from string
}

```

Ici, le compilateur n'a pas de moyen de connaître quel type a été sérialisé en String. Heureusement le programmeur lui sait ou une erreur Runtime sera levée.

