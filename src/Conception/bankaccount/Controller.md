# Contrôleur
Il recoit les requête WEB et transmet l'information à la partie métier. Nous y définissons l'ensemble des points d'entrées de notre application.

## API
* Chaque point d'entrée commence par `/account` suivi de
  * `/create` pour créer un compte 
  * `/update/{id}` pour mettre en jour un compte
  * `/delete/{id}` pour supprimer un compte
  * `/customer/{id}` récupérer **tous** les au comptes du client `id`
  * `/customer/{idCustomer}/account/{idAccount}` récupérer un compte spécifique `idAccount` du client `idCustomer`
  * `/customer/{idCustomer}/type/{accountType}` récupérer tous les comptes en fonction du type `CHEQUE` ou `EPARGNE`

Pour ce faire la classe `AccountController` possèdera :
- l'annotation `@RestController`
- l'annotation `@RequestMapping("/account")`

## Créer un compte

1. Prend en paramètre un objet représentant un `AccountRequestModel`
2. Appel du service métier 
   1. Si succès alors on renvoie le status 201
   2. Si erreur alors on renvoie le status 403

```java
@PostMapping("/create")
public ResponseEntity create(@RequestBody AccountRequestModel requestModel) {
    try {
        // Appel de la partie métier
        accountService.create(requestModel);
        return ResponseEntity.status(HttpStatus.CREATED).body("New account " + requestModel.getName() + " created");
    } catch (AccountNameAlreadyExistException e) {
        return ResponseEntity.status(HttpStatus.FORBIDDEN).body(e.getMessage());
    }
}
```