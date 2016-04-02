### Retour plus Lent

Les deux jeux de compilation et de commit devraient être exécutés sur un serveur d'intégration continue, normalement déclenché par un commit/push vers le dépôt.

En plus des jeux de compilation et de commit, d'autres jeux devraient être créés.

Ces jeux peuvent nécessiter des ressources indosponibles sur la machine locale et/ou prendre beaucoup de temps pour s'exécuter.

Ils peuvent également se relancer dans un environnement plus réaliste. Si un base de données en mémoire est normalement utilisé dans les tests locaux d'intégration, les mêmes tests devraient être exécutés avec une base de données de production.

Pour un build Maven, ces jeux sont susceptibles d'être implémentés via des profils ou des modules Maven séparés.

Ces jeux seront exécutés le plus fréquemment possible. En pratique, celà signifie via un ordonnanceur car ils consommeront surement beaucoup trop de temps pour être exécutés à chaque commit. Ici, "beaucoup trop de temps" signifie plus qu'ils peuvent prendre plus de temps que le temps entre deux commit/push sur le dépôt associé. 

Des tests chronométrés exécutent parfois les jeux quand le code n'a pas changé - celà fournit des informations utiles pour identifier des tests idiots.
