# La Notation Hongroise

L'idée de la notation Hongroise et des systèmes similaires est de refléter le type, la portée et tout autre attribut d'une variable dans son nom.


Par exemple:

* bFlag
* nSize
* m_nSize

Où `b` désigne un Boolean, `n` un entier et `m_` le fait que la variable est un attribut.

C'est une très mauvaise idée.

Une telle notation *peut* être utile si vous lisez du code imprimé sur papier, mais toutes ces information sont fournies dans un IDE moderne.

Nommer les choses est suffisament difficile sans avoir à y ajouter des contraintes.

Ces types de notations sont comme les commentaires. Ils ajoutent du bruit et doivent être maintenus en tandem avec l'information qu'ils dupliquent. Si un effort supplémentaire n'est pas fait pour les maintenir alors ils deviennent trompeurs.

Uncle Bob Martin le dit gentillement:

> "aujourd'hui, HN et d'autres formes de type de codage sont des obstacles. Elles rendent plus difficile de changer le nom ou le type d'une variable, d'une fonction, d'un atribut ou d'une classe. Elles rendent plus difficile la lecture du code. Et elles créent la possibilité que le type de codage trompera le lecteur"

