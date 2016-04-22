## Préférer des Décisions Réversibles 

### Sommaire

Préférer des décisions de design qui soient facilement modifiables.

### Détails

Beaucoup des décisions que vous faites pendant le design du code se révèleront mauvaises.

Si vous pouvez prendre des décisions réversibles en restraignant leurs conséquences et en ajoutant de l'abstraction, alors celà aura une importance moindre.

Par exemple - si vous introduisez une bibliothèque tierce et l'utilisez à nombreux endroits dans votre code, alors le coût d'un retour arrière sera élevé. Par contre, si vous ne l'utilisez qu'en un seul endroit, et fournissez une interface pour l'utiliser, le coût de retour arrière est très faible.

Mais n'oubliez pas KISS (Keep it simple, stupid : garde ça simple, idiot ) et YAGNI (you ain't gonna need it : vous n'en aurez pas besoin) - si vos abstractions compliquent le design alors il serait mieux de les oublier.
