Il est possible d'avoir plusieurs AuthenticationProvider afin de gérer différents type d'authentification :
- Par mot de passe
- OAuth2
- LDAP
- custom 

Ainsi, dans cette partie nous allons voir comment configurer notre application pour qu'elle accepte à la fois l'authentification par :
- mot de passe en retrouvant l'utilisateur en base de données
- en mémoire, nous allons enregistrer un utilisateur en mémoire directement
- en customisant notre propre provider

# AuthenticationManager

Nous devons donc configurer notre `AuthenticationManager` pour qu'il accepte différent type d'authentification. Pour ce faire nous allons nous aider de la classe `AuthenticationManagerBuilder`

```java
class SecurityConfig {
    @Autowired
    private CustomUserDetailsService userDetailsService;

    @Autowired
    private CustomAuthenticationProvider customAuthProvider;

    @Bean
    public AuthenticationManager authManager(HttpSecurity http) throws Exception {
        AuthenticationManagerBuilder authenticationManagerBuilder = http.getSharedObject(AuthenticationManagerBuilder.class);
        authenticationManagerBuilder.userDetailsService(userDetailsService);
        authenticationManagerBuilder.inMemoryAuthentication()
                .withUser("memuser")
                .password(passwordEncoder().encode("pass"))
                .roles("USER");
        authenticationManagerBuilder.authenticationProvider(customAuthProvider);
        return authenticationManagerBuilder.build();
}
}
```

- `authenticationManagerBuilder.userDetailsService(userDetailsService);` permet d'ajouter l'authentification par user/password via la configuration d'un userDetailsService qui ira chercher en base de données les informations. Derrière cette méthode se cache la création de l'objet `DaoAuthenticationProvider`
- `authenticationManagerBuilder.inMemoryAuthentication()` permet d'ajouter un utilisateur en mémoire directement
- `authenticationManagerBuilder.authenticationProvider(customAuthProvider)` permet de créer notre propre provider

# CustomAuthProvider
Classe simpliste, nous enregistrons les informations (user/password) directement dans le code afin de simplifier 

```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {
    @Override
    public Authentication authenticate(Authentication auth)
            throws AuthenticationException {
        String username = auth.getName();
        String password = auth.getCredentials()
                .toString();

        if ("externaluser".equals(username) && "pass".equals(password)) {
            return new UsernamePasswordAuthenticationToken
                    (username, password, Collections.emptyList());
        } else {
            throw new
                    BadCredentialsException("External system authentication failed");
        }
    }

    @Override
    public boolean supports(Class<?> auth) {
        return auth.equals(UsernamePasswordAuthenticationToken.class);
    }
}
```

# Conséquence
Maintenant nous avons trois moyens de nous authentifier
- Via les informations enregistrées en base
- En saisissant `memuser/pass` pour l'authentification en mémoire
- En saisissant `externaluser/pass` pour l'authentification custom