## Optimiser la Lisibilité par la Performance

### Sommaire

Pas d'optimisation prématurée.

Concentrez-vous pour construire quelque chose de simple et de compréhensible.

### Détails

Beaucoup de programmeurs s'inquiètent des performances pour chaque méthode qu'ils écrivent, évitent d'écrire du code qu'ils supposent inefficace et écrivent dans un style qui tente de réduire l'allocation d'objets, les appels de méthodes et d'autres facteurs qu'ils pensent avoir un coût.

Souvent cela réduit la lisibilité et augmente la compléxité, la plupart des mirco-optimisations ne fournissent pas de réels bénéfices.

Dans le context dans lequel nous travaillons, la performance devrait être l'une des préoccupations à considérer en dernier. Au lieu de cela, l'attention devrait être de faire un code simple et aussi lisible que possible.

Si un problème de performance se pose, le profilaging devrait être utilisé pour identifier où les problèmes se situent.

Cela ne veut pas dire que la performance devrait être écartée complètement, mais doit passer après avoir fourni un code lisible et simple jusqu'à ce qu'il soit prouvé qu'il y a un réel bénéfice à optimiser. Quand le code peut être écrit d'une façon plus efficace sans **aucune** augmentation de la compléxité ou compromis sur sa lisibilité alors le (présumé) code plus efficace devrait être écrit.