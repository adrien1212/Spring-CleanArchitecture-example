# UserDetailService

> UserDetailsService is used by DaoAuthenticationProvider for retrieving a username, a password, and other attributes for authenticating with a username and password 

Pour assurer l'authentification il faut que notre classe `RegisterCustomerServiceImpl` implémente en plus l'interface `UserDetailsService`. Cela va également impliqué de redéfinir et d'ajouter la méthode `loadUserByUsername()`.

**Note**  
Dans notre cas le critère d'uniticité est l'adresse email

```java
public interface RegisterCustomerService extends UserDetailsService {
	void register(RegisterCustomerRequestModel dto) throws ...;

	void update(RegisterCustomerRequestModel dto, Long id) throws ...;
}

```

```java
@Service
public class RegisterCustomerServiceImpl implements RegisterCustomerService {

	...

	@Override
	public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        // Récupération de l'utilisateur en BDD
		Customer customer = customerRegisterGateway.findByEmail(email);

		// Conversion d'un customer en UserDetail
		User user = new User(customer.getEmail(), customer.getPassword(),
				Collections.singletonList(new SimpleGrantedAuthority(customer.getRole().name())));

		return user;
	}
}
```

1- Nous allons récupérer l'utilisateur en base de données en fonction de son adresse mail
2- Puis convertir l'objet `Customer` retourné en un `UserDetail` qui est la représentation d'un utilisateur en Spring Security