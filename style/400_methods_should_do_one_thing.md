## Les Méthodes ne Devraient Faire qu'Une Chose

### Sommaire

Les méthodes ne devraient faire qu'une chose.

#### Détails

Un guide utile au fait que les fonctions ne doivent faire qu'une chose est donnée dans "Clean Code" par Robert C Martin.

"une autre façon de savoir si une fonction fait plus qu'“une chose” est de voir si vous pouvez en extraire une autre fonction avec un nom qui n'ait pas une ré-écriture de son implémentation."

"another way to know that a function is doing more than “one thing” is if you can extract another function from it with a name that is not merely a restatement of its implementation."

*Mauvais*

```java
  public void updateFooStatusAndRepository(Foo foo) {
     if ( foo.hasFjord() ) {
        this.repository(foo.getIdentifier(), this.collaborator.calculate(foo));
     }

     if (importantBusinessLogic()) {
       foo.setStatus(FNAGLED);
       this.collaborator.collectFnagledState(foo);
     }
  }
```

*Mieux*

```java
  public void registerFoo(Foo foo) {
     handleFjords(foo);
     updateFnagledState(foo);
  }

  private void handleFjords(Foo foo) {
      if ( foo.hasFjord() ) {
        this.repository(foo.getIdentifier(), this.collaborator.calculate(foo));
     }
  }

  private void updateFnagledState(Foo foo) {
    if (importantBusinessLogic()) {
       foo.setStatus(FNAGLED);
       this.collaborator.collectFnagledState(foo);
     }
  }
```

*Vous êtes allé trop loin*

```java
  public void registerFoo(Foo foo) {
     handleFjords(foo);
     updateFnagledState(foo);
  }

  private void handleFjords(Foo foo) {
      if ( foo.hasFjord() ) {
        this.repository(foo.getIdentifier(), this.collaborator.calculate(foo));
     }
  }

  private void updateFnagledState(Foo foo) {
    if (importantBusinessLogic()) {
       updateFooStatus(foo);
       this.collaborator.collectFnagledState(foo);
     }
  }

  private void updateFooStatus(Foo foo) {
    foo.setStatus(FNAGLED);
  }
```
