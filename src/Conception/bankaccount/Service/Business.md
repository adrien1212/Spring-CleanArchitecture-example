# Service Métier

Il fait le lien avec les trois parties :
- Le `Controller` d'où il reçoit une requête
- Le `Domaine` qu'il doit exploiter / modifier
- La base de données où sont stocker les informations

```Java
public interface AccountService {

	void create(AccountRequestModel requestModel) throws AccountNameAlreadyExistException;

	void update(AccountRequestModel requestModel) throws AccountDoestExistException;

	void delete(AccountRequestModel requestModel) throws AccountDoestExistException;
	AccountResponseModel getOneAccount(AccountRequestModel requestModel);

	List<AccountResponseModel> getAllAccounts(AccountRequestModel requestModel);

	List<AccountResponseModel> getAllAccountType(AccountRequestModel requestModel);
}

```

Sur l'implémentation nous devons spécifier l'annoation `@Service`

```java
@Service
public class AccountServiceImpl implements AccountService {
    ...
}
```

## Créer un compte bancaire
Pour créer un nouveau compte bancaire à l'utilisateur nous devons :
- vérifier que le compte n'existe pas
  - donc vérifier en base de données si nous avons quelque chose
- vérifier que le type de compte souhaité existe
  - vérifier que le type saisi est soit `Cheque` soit `Epargne`
- Créer le compte

```java
@Override
public void create(AccountRequestModel requestModel) throws AccountNameAlreadyExistException {
    // Récupérer en base de données un compte bancaire via l'identifiant du client et le nom du compte
    Account accountAlreadyExist = accountGateway.findByCustomerIdAndByName(requestModel.getIdCustomer(),
            requestModel.getName());

    if (accountAlreadyExist != null) {
        throw new AccountNameAlreadyExistException(
                "Account with name " + requestModel.getName() + " already exist");
    }

    // Vérifier si le type de compte est valide
    if (!requestModel.getAccountType().equals(AccountType.CHEQUE.name())
            && !requestModel.getAccountType().equals(AccountType.EPARGNE.name())) {
        throw new IllegalArgumentException("The account type " + requestModel.getAccountType() + " doesn't exist");
    }

    // Créer le compte bancaire en appelant la gateway et en transmettant les paramètres
    accountGateway.create(requestModel.getIdCustomer(), requestModel.getName(),
            AccountType.valueOf(requestModel.getAccountType()));
}
```

**Note**
Afin de simplifier la création des microservices par ordre de difficulté et étant donné que le microservice pour la création d'un nouvelle utilisateur n'a pas encore été développé nous ne vérifions pas ici si l'utilisateur existe (`customerID`).