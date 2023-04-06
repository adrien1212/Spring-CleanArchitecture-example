# Implémentation de l'architecture

Nous avons vu le fonctionnement de Spring Security, nous allons maintenant mettre en oeuvre les trois différente partie de l'authentification :
- Le manager `AuthenticationManager`
- Le provider `DAOAuthenticationProvider`
- Le security filter chain

Pour ce faire, nous allons développée une classe `SecurityConfig` que nous allons annoter avec `@Configuration` et `@EnabledWebSecurity`

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    /* manager */
    /* provider */
    /* security filter chain */
}
```

## Créer un AuhenticationManager
L'implémentation par défaut, si nous ne précisons pas le `AuthentificationManager` Spring utilisera le `ProviderManager`. Ainsi le code permettant de fournir un  `AuthentificationManager` à notre programme reste simple.

```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
    return authenticationConfiguration.getAuthenticationManager();
}
```

## Créer un DAOAuthenticationProvider
Le `DAOAuthenticationProvider` à une dépendance vers un `PasswordEncore` et vers un `UserDetailService` que nous devons lui founir

```java
@Bean
public AuthenticationProvider authenticationProvider(UserDetailsService userDetailsService , PasswordEncoder passwordEncoder) {
    DaoAuthenticationProvider daoAuthenticationProvider = new DaoAuthenticationProvider();
    daoAuthenticationProvider.setPasswordEncoder(passwordEncoder);
    daoAuthenticationProvider.setUserDetailsService(userDetailsService);
    return daoAuthenticationProvider;
}

@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```
Etant donné que nous avons besoin d'un `passwwordEncoder` nous devons créer un Bean spécifique. Concernant l'`userDetailService` il sera injecté via l'annotation que nous positionnerons sur la classe.

## Créer une Security Filter Chain
Il ne nous reste plus qu'à définir la *Security Filter Chain*. 

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

  @Bean
  public SecurityFilterChain filterChain(HttpSecurity httpSecurity) throws Exception {
    httpSecurity.csrf().disable()
                    .authorizeHttpRequests()
                    .antMatchers("/customer/auth/*").permitAll()
                    .anyRequest().authenticated();

        // Construction de la chaine
        return httpSecurity.build();
  }
}
```

### Autoriser des requêtes sans authentification
Pour que l'utilisateur est la possibilité de créer un compte ou de se connecter nous devons autoriser les requête vers ces deux end-points :
- `.antMatchers("/customer/auth/*").permitAll()` permet de dire que toutes requêtes provevant de `/customer/auth/*` n'ont pas besoin d'être authentifiées :
  - `/customer/auth/login`
  - `/customer/auth/register`

### Interdire des requête aux utilisateurs non connectés
Néanmois, nous souhaitons que les autres pages soient accessibles seulement aux utilisateur connectés :
- `.anyRequest().authenticated()` spécifie que seuls les utilisateurs authentifiés peuvent avoir accès à toutes autres ressources

## Conclusion
En soit, cette classe nous permet de personnaliser comment Spring va gérer l'authentification. En effet, Spring fournit le mécanisme mais nous sommes libre de :
- Préciser le fonctionnement de la `SecurityFilterChain`
- Préciser l'`AuthenticationManager` à utiliser
- Fournir un ou plusieurs `AuthenticationProvider`