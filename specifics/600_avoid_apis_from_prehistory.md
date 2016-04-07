## Eviter des APIs de la Pré-Histoire

### Sommaire

N'utiliser pas `Vector`, `StringBuffer` et autre parties archaique du JDK.

### Détails

Java est présent depuis 20 ans. Dans le but de maintenir la compatibilité ascendante, il a amassé toutes sortes d'API qui n'ont plus de sens aujourd'hui. Certaines sont marquées par l'annotation @Deprecated, d'autres non. 

Malheureusement, beaucoup sont encore utilisées dans des cours universitaires et des exemples en ligne. Les nouveaux développeurs Java peuvent ne pas être au courant qu'elles ont été remplacées - quelques-uns à connaitre:

* `java.util.Vector` - utiliser `ArrayList` plutôt
* `java.lang.StringBuffer` - utiliser `StringBuilder` plutôt
* `java.util.Stack` - utiliser une `Dequeue` (e.g. `ArrayDeqeue`)
* `java.util.Hashtable` - utiliser une `Map` (e.g. `HashMap`)
* `java.util.Enumeration` - utiliser un `Iterator` ou un `Iterable`

Chacun de ces remplacements (excepter `Enumeration`) diffèrent de l'original en n'étant pas synchronisés. Si vous pensez avoir besoin d'une collection synchronisée isolez-vous et réfléchissez à nouveau.

