# Architecture d'un microservice

Nous allons exploiter l'architecture hexagonale et la Clean Architecture.

## Les objectifs de cette architecture
### Les entités
Nous devons préserver les entités de toute altération. Elles sont le coeur de notre application et ne doivent communiquer qu'entres-elle. En effet, les entités n'ont pas connaissance de comment elles vont être utilisée. Ce sont par définition des objets autonomes.

### Les UseCase
Ils sont utilisés par une ou plusieurs entités pour réaliser une action. Ils permettent la coordination des entités entres-elles afin de fournir un résultat à l'utilisateur. Les UseCase peuvent être lié à des librairies. Par exemple pour enregistrer une nouvel utilisateur l'UseCase devra s'assurer de sa persistance.

Dans notre application, le diagramme BPMN nous permet d'identifier facilement quels seront ces cas d'utilisation

### Les librairies
Le coeur de l'application doit être indépendant des point d'entrée et des points de sortie (les librairies). En effet, si une librairie change ou que ses méthodes changent notre coeur lui ne doit pas être impacté.
Dans ce but, afin de communiquer avec une librairie nous allons utiliser les ports/adaptateurs. Par exemple, nous n'allons pas directement appeler les méthodes d'une librairie JDBC mais passé par un adaptateur qui lui enverra les requêtes.

Nous assurons ainsi l'indépendance du coeur


## Exemple
Nous reviendrons en détail sur l'implémentation de cette architecture. Mais ci-dessous voici un exemple de la construction de chaque microservice

![](Images/springArchi.drawio.png)