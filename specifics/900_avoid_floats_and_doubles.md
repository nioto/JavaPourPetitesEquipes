## Evitez les Floats et les Doubles

### Sommaire

Evitez d'utiliser les floats et les doubles (les types primitifs et leur wrappers).

### Détail

Les floats et les doubles introduisent un champs de mines avec les probèmes d'arrondis et de comparaisons. Bien qu'ils soient un choix raisonnable pour certains domaines où vous ne tenez pas compte des erreurs d'arrondis, les types Integer et `BigDecimal` sont de meilleur candidat pour du code professionnel côté serveur.

Le problème essentiel est que les nombres à virgule flotante ne sont pas capables de représenter certains nombres ( par ex `0.1`)

Cela entraîne des résultats inattendus qui auraient pu être détectés par de simples tests.

```java
    double balance = 2.00;
    double transationCost = 0.10;
    int numberTransactions = 6;

    System.out.printf("After %s transactions balance is %s"
                    , numberTransactions
                    , balance - (transationCost * numberTransactions));
    // Gives After 6 transactions balance is 1.4 :-)
```

Mais

```java
    double balance = 2.00;
    double transationCost = 0.10;
    int numberTransactions = 7;

    System.out.printf("After %s transactions balance is %s"
                     , numberTransactions
                     , balance - (transationCost * numberTransactions));
    // Gives After 7 transactions balance is 1.2999999999999998 :-(
```

La solution la plus simple dans ce cas aurait été de remplacer les floats par valeurs entières ( càd conserver la somme en unités de centimes plutôt qu'en euros), mais le code peut aussi être ré-écrit en utilisant `BigDecimal`.

```java
    BigDecimal balance = new BigDecimal("2.00");
    BigDecimal transationCost = new BigDecimal("0.10");
    
    BigDecimal numberTransactions = BigDecimal.valueOf(7);

    System.out.printf("After %s transactions balance is %s"
                     , numberTransactions
                     , balance.subtract(transationCost.multiply(numberTransactions)));

   // Gives After 7 transactions balance is 1.30 :-)
```

On remarquera que `BigDecimal` peut être construit en utilisant un float, mais celà nous ramènera au problème précédent.


```java
    BigDecimal balance = new BigDecimal("2.00");
    BigDecimal transationCost = new BigDecimal(0.10); // <- float used to construct
    
    BigDecimal numberTransactions = BigDecimal.valueOf(7);

    System.out.printf("After %s transactions balance is %s"
                     , numberTransactions
                     , balance.subtract(transationCost.multiply(numberTransactions)));

   // Gives After 7 transactions balance is 1.2999999999999999611421941381195210851728916168212890625
```

