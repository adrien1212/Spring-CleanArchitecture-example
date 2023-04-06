# Gateway

Nos services sont déployés sur plusieurs serveur et accessibles via le :
- port `:8081` pour *bankaccount*
- port `:8083` pout *registration*

Néanmoins il serait intéressant de pour acceder à toutes les APIs en utilisant le même port, par exemple :
- `:8080/account/create` au lieu de `:8081/account/create`
- `:8080/auth/login` au lieu de `8083/auth/login`

Ainsi la Gateway est un serveur qu'on va déployé et qui va avoir pour rôle de rediriger le traffic vers le bon port. Elle permet d'offrir un accès simplifié. 