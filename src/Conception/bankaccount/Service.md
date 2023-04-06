# Service

> This layer contains application specific business rules. It encapsulates and implements all of the use cases of the system. These use cases orchestrate the flow of data to and from the entities, and direct those entities to use their enterprise wide business rules to achieve the goals of the use case.

Le service métier, ici la gestion d'un compte bancaire est :
- représenté par une interface `AccountService`
- cette interface est appelée par notre client au travers du Contrôleur `AccountController`
- Les informations transmises entre le Contrôleur et le service métier sont encapsulées dans un DTO. 
- On récupère les informations pertinantes contenu dans le DTO et on applique l'action demandée (e.g créer un nouvel utilisateur)
- Certaine action comme *créer un nouvel utilisateur* necessite de faire appel à la base de donnée que nous appelerons au travers d'un Adaptateur `AccountGateway`