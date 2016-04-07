## Préférer les Boucles For-Each aux Boucles For

### Sommaire

Utiliser les boucles `for each` de préférence aux boucles par index.

### Détails

La boucle `for each` introduite dans Java 5 évite les erreurs potentielles aux extrèmes des boucles par index et est beacoup plus concise que d'utiliser des Iterator.


*Mauvais*
```java
  public List<String> selectValues(List<Integer> someIntegers) {
    List<String> filteredStrings = new ArrayList<String>();
    for (int i = 0; i != someIntegers.size(); i++) {
      if (value > 20) {
        filteredStrings.add(value.toString());
      }
    }
    return filteredStrings;
  }
```
  
*Un peu mieux*
```java
  public List<String> selectValues(List<Integer> someIntegers) {
    List<String> filteredStrings = new ArrayList<String>();
    for (Integer value : someIntegers) {
      if (value > 20) {
        filteredStrings.add(value.toString());
      }
    }
    return filteredStrings;
  }
```
