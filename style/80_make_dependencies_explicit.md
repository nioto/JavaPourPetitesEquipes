## Rendez les Dépendances Explicites et Visibles

### Sommaire

Faite en sorte que les dépendances d'une classe soit clairement visible.

Injectez toujours les dépendances dans une classe en utilisant sont constructeur. N'utilisez pas d'autres méthodes telles que les setters ou des annotations sur les champs.

N'introduisez pas de dépendances en utilisant des chemins caché tels que `Singletons` ou `ThreadLocals`.

### Détails

Le code est plus facile à comprendre si les interfaces et les classes dont chaque objet dépend sont évidents et visibles.

Les dépendances les plus visibles sont celles qui sont injectées dans une méthode via un paramètre.

Celles moins visibles sont celles stockées dans des champs mais, en fonction de comment ces champs sont renseignés, les dépendances peuvent être relativement facile à découvrir

#### Injection dans le Constructeur

L'injection de dépendance du constructeur indique clairement les dépendances de l'objet en un seul endroit et assurent que les objets sont seulement créés dans des états valides. Celà permet aux champs d'être mis en **final** ainsi leur cycle de vie n'est pas ambigü.

Celà est la seule façon dont les dépendances devraient être injectées.

#### Injection par un Setter

L'injection par un setter augmente le nombre d'états possibles qu'un objet peut avoir. Plusieurs de ces états seront invalides.

Si l'injection par un setter est utilisée, une classe peut être construite dans une état semi initialisé. Ce qui constitute un état entièrement initialisé ne peut être déterminé qu'en éxaminant le code.


**Mauvais**
```java
public class Foo() {
  private Bar bar;

  public void doStuff() {
    bar.doBarThings();
  }

  public void setBar(Bar bar) {
    this.bar = bar;
  }
}
```

Ici, une `NullPointerException` sera lancée si `Foo` est construit sans appel à `setBar`.

**Mieux**
```java
public class Foo() {
  private final Bar bar;

  public Foo(Bar bar) {
    this.bar = bar;
  }

  public void doStuff() {
    bar.doBarThings();
  }
}
```

Ici, il est clair que nous devons fournir un `Bar` car sans lui on ne pourrait créer la classe.

#### Annotations des Champs

Bien que les annotations sur les champs semblent commodes, elles rendent les dépendances non visibles dans l'API publique. Elles lient également la construction de votre classe aux frameworks qui les comprennent et nous privent de rendre les champs final.

Les annotations ne devraient pas être utilisées.

Si vous travaillez avec un framework d'injection de dépendances tel que Spring, déplacez la construction de vos objets dans les classes de configuration ou limitez l'usage des annotations aux constructeurs. Ces deux méthodes autorisent vos classes à être instanciées normalement et assure que toutes dépendances sont visibles.

#### Dépendances Cachées

Tout ce qui n'est pas injecté dans une classe en utilisant un constructeur ou en passant pas un paramètre de méthode est une dépendance cachée.

Elles représentent un fléau.

Elles sont récupérées depuis des `Singletons`, `ThreadLocals`, appels de méthodes statiques ou simplement par appel de `new`.


**Mauvais**
```java
public class HiddenDependencies {
  public void doThings() {
    Connection connection = Database.getInstance().getConnection();
    // do things with connection
    ....
  }
}
```

Ici nous devons nous assurer que la classe `Database` est dans un état valide avant d'appeler la méthode `doThings` dans le code précédent, mais nous n'avons aucun moyen de le savoir sans regarder chaque ligne de code.

**Mieux**
```java
public class HiddenDependencies {
  private final Database database;

  public HiddenDependencies(Database database) {
    this.database = database;
  }

  public void doThings() {
    Connection connection = database.getConnection();
    // do things with connection
    ....
  }
}
```

L'injection par le constructeur rend la dépendance clairement visible.

Par définition, les dépendances cachées sont dures à trouver mais pose un autre problème - elles sont aussi dures à remplacer.

#### Seams

Seams est un concept introduit par Michael Feathers dans "Working Effectively with legacy code"

Il le définit ainsi:

> "un endroit où vous pouvez changer le comportement de votre programme sans éditer cet endroit"
("a place where you can alter behavior in your program without editing in that place.")

Dans la version originale de `HiddenDependencies` si nous voulons remplacer `Database` par un Mock ou un Stub nous ne pouvons le faire que si le singleton fournit une méthode pour modifier l'instance retournée.


**Pas une bonne approche**
````java
public class Database implements IDatabase {
  private static IDatabase instance = new Database();

  public static IDatabase getInstance() {
    return instance;
  }

  public static void setInstanceForTesting(IDatabase database) {
    instance = database;
  }

}
````

Cette approche introduit un seam mais ne résout pas nos préoccupations à propos de la visibilité. La dépendance reste cachée.

Si nous utilisons cette approche, notre codebase restera dur à comprendre et nous nous trouverons constamment à lutter contre les dépendances en test.

Avec l'injection par le constructeur, nous gagnons un steam et rendons la dépendance visible. Même si `Database` est n singleton, nous sommes capables d'isoler le code de son implémenation pour les tests.


