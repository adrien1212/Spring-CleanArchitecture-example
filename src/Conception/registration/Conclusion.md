# Conclusion

L'authentification n'est pas une partie simple à comprendre. Nous revonnons ici sur les principaux points mis en application.

## Des requêtes interceptées
Toutes les requêtes sont interceptées par la *Security Filter Chain*. Ainsi, si on souhaite accéder à une ressource protégée (e.g. `/restricted`)
1. La *Security Filter Chain* intercepte la requête
2. Si aucune expception n'est levée, c'est-à-dire que l'utilisateur était bien authentifié
3. Alors la méthode représentant l'appel (ici `restricted()`) est invoquée et retourne la ressource.

## Différentes authentifications
Nous avons :
- Créer notre propre système d'authentification afin d'introduire la notion
- Puis nous avons vu l'authentification *Basic* qui permet de rentrer dans le détail de la *Security Filter Chain*
- Pour au final, créer notre propre filtre afin de réaliser l'authentification JWT

**Authentification notion**
![](Images/SpringSecurityArchi.png)

**Authentification Basic**
![](Images/SpringSecurityArchiBasic.png)

**Authentification JWT**
![](Images/SpringSecurityArchiJWT.png)