# Base de données

## Entité JPA
Nous n'avons qu'une seule entité JPA qu'est l'utilisateur. De même que pour les DTOs on remarque que l'entité JPA n'est pas exactement la même que l'entité du Domaine. En effet, il peut être intéressant de connaitre la date de création/de mise à jour du compte en base sans forcément l'exploiter dans notre application.

```Java
@Entity
@Table(name = "customer")
public class CustomerJpaEntity {
	@SequenceGenerator(name = "users_sequence", sequenceName = "users_sequence", allocationSize = 1)
	@Id
	@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "users_sequence")
	private int id;

	@NonNull
	@Column(name = "first_name")
	private String firstName;

	@NonNull
	@Column(name = "last_name")
	private String lastName;

	@NonNull
	@Column(name = "email", unique = true)
	private String email;

	@NonNull
	@Column(name = "password")
	private String password;

	@NonNull
	@Column(name = "mobile", unique = true)
	private String mobile;

	@NonNull
	@Enumerated(EnumType.STRING)
	private Role role;

	@Column(name = "created_at", updatable = false)
	@NonNull
	private LocalDateTime createdAt;

	@Column(name = "updated_at")
	@NonNull
	private LocalDateTime updatedAt;

}
```

## Le Repository
Il implémente l'interface `JPARepository` est ajoute des méthodes spécifiques. 

```Java
@Repository
public interface CustomerJpaRepository extends JpaRepository<CustomerJpaEntity, Long> {
	Optional<CustomerJpaEntity> findByEmail(String email);

	Optional<CustomerJpaEntity> findByMobile(String mobile);

	Optional<CustomerJpaEntity> findById(Long idCustomer);
}
```

**Note**  
Nous avons créer la méthode `findById` car nous souhaitons retourner un `Optional`. Néanmoins, JPA fourni la méthode `getReferenceById` qui permet de récupérer une entité de type `Entity` (i.g. `CustomerJpaEntity`).