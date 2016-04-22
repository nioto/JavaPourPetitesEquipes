## Utilisez Final Librement

### Sommaire

Envisagez de rendre final toute variable ou paramètre qui ne doit pas changer.

### Détails

Rendre les paramètres et les variables déjà assignées  final rend la méthode plus facile à comprendre car celà restreint le nombre de choses qui peuvent changer lors de son exécution.

Il est raisonnable de rendre les paramètres et les variables qui ne sont assignées qu'une fois final dans toutes les méhodes, mais cela doit pondérer avec le bruit introduit en insérant le mot clé `final` partout.

Pour des méthodes courtes, que le bénéfice l'emporte sur le coût est discutable, mais pour les méthodes longues et lourdes le bénéfice est bien supérieur.

Chaque équipe devrait se mettre d'accord sur la politique de mettre les variables final.

Au minimum, tout devrait être rendu final dans une méthode longue. Cela peut aussi être étendu à des méthodes plus courtes à la discrétion de l'équipe. Une politique du tout a l'avantage d'être facile à automatiser/comprendre. Une politique nuancée plus difficile à transmettre.

Lorsque vous travaillez avec du code éxistant, rendre les paramètres et les variables final est un premier pas à la compréhension de la méthode avant sa ré-écriture. Les méthodes qui se sont avérées difficiles à découper en plus petits blocs seront plus faciles à comprendre quand les variables assignées qu'une fois sont rendues final.