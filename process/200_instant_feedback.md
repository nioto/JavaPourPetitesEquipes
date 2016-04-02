## Retour Instantané

Un IDE moderne tel qu'Eclipse ou IntelliJ vous fournira un retour instantané en même temps que vous écrivez, utilisant son compilateur et des outils d'analyses statiques configurables.

Vous pouvez augmenter la quantité de retour instantané que vous recevez en faisant bon usage du système de typage de Java et en configurant les outils d'analyses statiques.

Bien que les retours de votre IDE sont rapides et commodes, il a quelques inconvénients.

* Il peut être différent d'une machine à l'autre en fonction de la configuration de l'IDE
* Il est souvent non binaire (càd réussite/échec)
* Il peut être ignoré / négligé
* Vouloir qu'il soit rapide limite ses possibilités

Pour ces raisons vous devriez purement et simplement éviter d'avoir un processus centré sur l'IDE. Le code ne devrait pas être considéré complet tant que les tests unitaires n'ont pas été exécutés via un fichier de build.
