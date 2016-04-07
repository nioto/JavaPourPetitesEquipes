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

Normally, objects placed into a collection should be of a single type or of multiple types related by a common superclass or interface.   

Here, unrelated types have been placed into the same list with a String used to communicate some sort of information about how "processing" of a widget has failed.

The classic OO fix for this code would be to introduce a `ProcessResult` interface with two concrete implementations.

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

The original code can then be fixed as follows:

```java
List<Widget> widgets = getWidgets();
List<ProcessResult> results = process(widgets);
    
for (ProcessResult each : results) {
    each.doSomething();
  }
}
```

Or, more concisely in Java 8:

```java
 List<ProcessResult> results = process(widgets);
 results.stream().forEach(ProcessResult::doSomething); 
```

It may also sometimes make sense to use a disjoint union type aka `Either`.

This technique can be particularly useful as an interim step when reworking legacy code that uses mixed type raw collections, but can also be a sensible approach when dealing with error conditions.

Unfortunately, Java does not provide an `Either` type out of the box but at its simplest it looks something like:

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

Libraries such as Atlassian's Fugue provide implementations with much richer functionality.

Using the simplistic form of `Either` with Java 7 the code could be re-written as:

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

While most Java programmers will prefer the earlier OO version, this version has two advantages:

1. It requires no change to the *structure* of the original code - all we have really done is make the types document what is happening
2. It requires less code

This pattern can help quickly tame a legacy code base that is difficult to comprehend.

#### Limits of the Type System

Sometimes we do reach the limits of Java's type system and need to cast. 

Before we do this, we must make certain that the cast is safe and there is no better solution to our problem. 

Similarly, we may need to sometimes suppress a Generics warning, this can be done by annotating with `@SuppressWarnings` e.g. 

```java
@SuppressWarnings("unchecked")
<T> T read(final Class<T> type, String xml) {
  return (T) fromXml(xml);
}

Object fromXml(final String xml) {
  return ... // de-serialise from string
}

```

Here, the compiler has no way of knowing what type has been serialized to the String. Hopefully the programmer does or else a runtime error will occur.

