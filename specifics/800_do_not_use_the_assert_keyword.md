## N'Utilisez pas le Mot-Clé Assert

### Sommaire

N'utilisez pas le mot-clé assert

### Détails

Les assertions écrites avec le mot-clé assert ne sont prises en compte que lorsqu'un flag est actif dans la JVM. Ce n'est pas ce que vous voulez.

Pour du code en production, utilisez plutôt des bibliothèques telles que les préconditions de Guava qui sont assurées d'être déclenchées.

Si vous avez le mot-clé assert dans votre code de tests, demandez-vous comment se fait-il que vous n'ayez pas remarqué que vos tests ne peuvent échoués. Après votre période de réflexion, remplacez les assert par des appels à une bibliothèque d'assertion telle que AsserJ.