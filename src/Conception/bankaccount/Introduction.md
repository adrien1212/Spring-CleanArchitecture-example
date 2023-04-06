# Introduction

Ce module est dédié à la gestion d'un compte bancaire. Nous distinguons la notion de *compte bancaire* et *d'utilisateur*, un utilisateurs peut posséder plusieurs compte bancaire.

## Analyse du besoin
Dans notre application un compte bancaire se traduit par les actions suivantes :
- Créer le compte bancaire pour un utilisateur
- Mettre à jour le compte
- Supprimer le compte bancaire
- Consulter son compte bancaire
- Afficher tous ses comptes bancaires


Nous rajoutons également quelques règles concernant les comptes bancaires :
- Un compte bancaire peut être du type `CHEQUE` ou `EPARGNE`
- Un compte bancaire porte un nom (e.g. *compte principale*)
- Un compte bancaire à une `Balance`
