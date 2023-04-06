# JWTAuthenticationFilter

## Créer le filtre
```java
@Component
class JwtAuthenticationFilter extends OncePerRequestFilter {
    @Autowired
    private JwtUtil jwtUtil;

    @Override
    protected void doFilterInternal(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse,
            FilterChain filterChain) throws ServletException, IOException {

        // On récupère le token contenu dans le Header
        final String authorizationHeader = httpServletRequest.getHeader("Authorization");
        if (authorizationHeader == null || authorizationHeader.isEmpty() || !authorizationHeader.startsWith("Bearer")) {
            // S'il n'existe pas alors on passe au filtre suivant
            // C'est le cas pour le /login
            filterChain.doFilter(httpServletRequest, httpServletResponse);
            return;
        }

        final String token = authorizationHeader.split(" ")[1].trim();
        if (!jwtUtil.validate(token)) {
            // Si le token est invalide, alors on passe au filtre suivant
            filterChain.doFilter(httpServletRequest, httpServletResponse);
            return;
        }

        // Le token est valide
        String username = jwtUtil.getUsername(token);
        
        // On va créer une Authentication
        UsernamePasswordAuthenticationToken upassToken = new UsernamePasswordAuthenticationToken(username, null,
                new ArrayList<>());
        upassToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(httpServletRequest));

        // On passe l'Authentication au Security Context
        SecurityContextHolder.getContext().setAuthentication(upassToken);

        // On appelle le filtre suivant
        filterChain.doFilter(httpServletRequest, httpServletResponse);
    }
}
```

## Ajouter le filtre à la SecurityFilterChain
On ajoute notre `JwtAuthenticationFilter` avant le filtre `UsernamePasswordAuthenticationFilter`.
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity httpSecurity) throws Exception {
    httpSecurity...

    httpSecurity.addFilterBefore(new JwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);

    return httpSecurity.build();
}
```

## Cas d'une requête quelconque
Dans le cas d'une requête quelconque nous :
1. Vérifions quelle contienne une Authorization
2. Vérifions que le token fourni est valide
3. Si tel est le cas, alors nous créons une `Authentication` que nous ajoutons au `SecurityContext`
4. Nous passons au filtre suivant
5. Une fois tous les filtres exécutés, nous arriverons dans la methode exécutons la méthode appelée, par exemple `/restricted`

## Cas d'une requête de connexion
Dans le cas d'une requête `/auth/login` nous n'avons pas de token donc nous rentrons dans le premier `if` ce qui va exécuter les filtres suivants.

Une fois tous les filtres exécutés, nous arriverons dans la methode `login()` où nous devrons :
- Créer un objet de type `Authentication`
- Appeler le `AuthenticationManager`
- Ajouter l'`AuthenticationManager` au `SecurityContext`
- Et, créer et renvoyer le JWT Token