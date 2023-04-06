# Domaine

```Java
public class Customer {

	private String email;
	private String password;
	private String firstName;
	private String lastName;
	private String username;
	private String phoneNumber;
	private Role role;

}
```

```Java
public enum Role {
	USER("User"), ADMIN("Admin");

	private final String value;

	private Role(String value) {
		this.value = value;
	}

	public String getValue() {
		return value;
	}
}
```