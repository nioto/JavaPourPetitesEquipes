## Préférer un Code Lisible plutôt que les Commentairess

### Sommaire

Utiliser les commentaires uniquement pour expliquer ce que le code ne peut pas expliquer par lui-même.

Si vous êtes sur le point d'écrire un commentaire, réléchissez d'abord s'il n'y pas un moyen de changer le code afin de le rendre compréhensible sans commentaire.

### Détails

De Clean Code par Robert C Martin.

*"Rien ne peut être plus utile qu'un commentaire bien placé. Rien ne peut encombrer un module plus qu'un commentaire dogmatique futile. Rien de peut être plus dommageable qu'un commentaire obsolète qui propage des mensonges et de la désinformation."*

Les commentaires devraient être utilisés pour expliquer le fonctin d'un code qui ne peut être ré-écrit pour qu'il soit auto-explicatif.


*Mauvais*

```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) &&
(employee.age > 65))
```

*Mieux*

```java
if (employee.isEligibleForFullBenefits())
```

Un commentaire n'est utile que s'il explique quelquechose que le code ne peut expliquer lui-même.

Cela signifie que tout commentaire que vous écrivez deoit fournir le **pourquoi**, pas le quoi ou le comment.

*Mauvais*

```java
// make sure the port is greater or equal to 1024
if (port < 1024) {
  throw new InvalidPortError(port);
}
```
    
*Mieux*

```java
// port numbers below 1024 (the privileged or “well-known ports”)
// require root access, which we don’t have
if (port < 1024) {
  throw new InvalidPortError(port);
}
```

*Encore mieux*

```java
if (requiresRootPrivileges(port) {
  throw new InvalidPortError(port);
}

private boolean requiresRootPrivileges(int port) {
  // port numbers below 1024 (the privileged or "well-known ports") 
  // require root access on unix systems
  return port < 1024; 
}
```

Ici, l'intention fonctionnelle a été perçue dans le nom de la méthode, le commentaire a été fourni uniquement pour fournir un context afin que la logique prenne sens.

Le nombre magique aurait dû être remplacé par une constante.


```java
final static const HIGHEST_PRIVILEDGED_PORT = 1023; 

private boolean requiresRootPrivileges(int port) {
  // The privileged or "well-known ports" require root access on unix systems
  return port <= HIGHEST_PRIVILEDGED_PORT; 
}
```

Le commentaire ajoute encore de la valeur - il donne au lecteur non familier avec le sujet  deux mots clés pour faire une recherche sur un internet.