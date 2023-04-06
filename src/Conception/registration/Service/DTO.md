# DTO

Nous décisons ici de créer deux DTO pour les requêtes :
- Un pour la création d'un compte utilisateur
- Un pour la demande de connexion

```Java
public class RegisterCustomerRequestDTO {
	private String email;
	private String password;
	private String matchedPassword;
	private String firstName;
	private String lastName;
	private String phoneNumber;

}
```

```Java
public class LoginRequestDTO {
	private String email;
	private String password;
}
```

On remarque bien la différence entre les DTOs et le Domaine. Dans le premier DTO, nous avons deux fois le mot de passe. L'utilisateur va être amené à le saisir deux fois mais dans le Domaine nous ne le spécifions qu'une seule fois (comme en base de donnée). En effet, dans la partie service métier nous allons vérifier que les deux mot de passes sont égaux.