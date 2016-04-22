## Utilisez une Mise en Page Consistente du Code Dans Chaque Projet

### Sommaire

Convenez et appliquez un style de code standard dans votre base de code.

### Détails

La façon dont le code Java est mis en forme et mis en place est en grande partie une question de préférence personnelle.

Certains styles (tel que l'omission d'accolade dans les conditions if) peut engendrer certains types de bugs.

D'autres peuvent demander plus de travail pour garder le code conform (tel qu'aligner les champs en colonnes) mais, à première vue, aucune ne se distingue par sa supériorité.

Malgré celà, les programmeurs ont tendance à avoir des opinions bien arrêtées sur le sujet.

Chaque codebase devrait, toutefois, avoir un style unique de mis en forme accepté qui est appliqué de manière cohérente et comprise par toute personne travaillant sur ce codebase.

Celà évite les guerres de commit dans lesquelles chaque membre de l'équipe re-formatte le source selon sa préférence personnelle. Celà rend également le code plus facile à compréhendre dans la mesure où il y a un coût cognitif pour le lecteur si la mise en forme change d'un fichier  l'autre.

Bien qu'il y ait un bénéfice dans la cohérence, il y a aussi un coût.

A moins qu'il n'y ait déjà un large accord entre les équipes à propos de comment les choses doivent être mise en forme, essayer de faire respecter un ensemble officiel de règles est suscreptible de créer plus d'amertume que de bien.

Une référence globale pour la mise en forme devrait être mise en place, mais les équipes devraient être autorisées à en dévier s'ils conservent un style cohérent dans le code qu'ils maintiennent.

### Règles de Mises en Forme Suggérées

Si vous n'avez pas défini votre propres règles nous vous conseillons de suivre celles de Google [Google Java Style](https://google.github.io/styleguide/javaguide.html).

Ces règles de mis en forme sont bien pensées, bien documentées et pas trop rigides.

Nous ne les décrirons pas en détail ici, mais du code mis en forme selon ces règles ressemble à quelque chose comme ça:-

```java
class Example {
  int[] myArray = {1, 2, 3, 4, 5, 6};
  int theInt = 1;
  String someString = "Hello";
  double aDouble = 3.0;

  void foo(int a, int b, int c, int d, int e, int f) {
    if (f == 5) {
      System.out.println("fnord");
    } else {
      System.out.println(someString);
    }

    switch (a) {
      case 0:
        Other.doFoo();
        break;
      default:
        Other.doBaz();
    }
  }

  void bar(List<Integer> v) {
    for (int i = 0; i < 10; i++) {
      v.add(new Integer(i));
    }
  }
}
```

Toutefois, nous vous suggérons de ne pas suivre leur règle sur comment écrire le Javadoc et de suivre les notres.

#### Points Remarquables à Propos de ce Style

##### des Espaces pas des Tabulations

Les tabulations s'affichent différement selon la configuration de l'éditeur. Celà resultere à la remise en forme à chaque fois que différents programmeurs adapterons le code selon la configuration de leur éditeur. Les espaces évitent ce problème.

Dans certains langages (càd JavaScript avant la montée des minifieurs de code) les tabulations ont/avaient l'avantage de réduire la taille du code comparé à l'usage de plusiseurs espaces. L'augmentation de la taille du fichier source n'a pas d'importance en Java.

##### One True Brace Style (Toujours utiliser des accolades)

Il y a plusieurs arguments à propos des supposés avantages de ce style, mais nous vous suggérons de l'adopter de part son acceptatin par la communauté Java.

Bien qu'une simple test `if else` peut être plus concis écrit sans accolades nous vous suggérons de toujours en mettre. Celà réduit la probabilité d'avoir une instruction placée en dehors alors que ce n'était pas le but.

