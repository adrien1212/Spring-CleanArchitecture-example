# Introduction
> L’authentification est un processus permettant au système de s’assurer de la légitimité de la demande d’accès faite par une entité […] afin de l'autoriser à accéder à ressources du système.

Dans ce chapitre, nous nous interessons donc à l'enregistrement et la connexion d'un utilisateur. En soit, gérer l'authentification

## Analyse du besoin
Des pages sont accéssible sans être connectés. C'est le cas de la page de *login* et de *registration*.
D'autres pages, demande à l'utilisateur d'être connecté, comme la page *restricted*.

Nous devons donc mettre en place un système permettant d'accéder à des ressources non protégées et à une ressource protégée.