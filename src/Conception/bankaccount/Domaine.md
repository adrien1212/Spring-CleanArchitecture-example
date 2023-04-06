# Le domaine

> Domain encapsulate Enterprise wide business rules. An entity can be an object with methods, or it can be a set of data structures and functions.

```java
public class Account {
	private String name;

	private AccountType accountType;

	private Balance balance;
}
```

```java
public class Balance {
	private double amount;
}
```

```java
public enum AccountType {
	CHEQUE, EPARGNE
}
```