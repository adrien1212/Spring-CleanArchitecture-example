# Architecture Générale

Nous optons pour une architectecure en microservices où chaque service peut être deployé séparément et dispose de sa propre base de données.

Chaque service reprendra le même fonctionnement :
- Un service représentant le comtpe bancaire d'un client.
- Un service permettant l'authentification d'un client
- Un dernier service gérant la communication vers les deux autres microservices.

![](Images/archi_generale.png)

