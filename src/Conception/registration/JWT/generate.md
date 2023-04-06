# Generer token

Nous devons d'abord créer une classe permettant de générer un token JWT. Pour ce faire. Je m'appuis sur des exemples fournis sur internet.

```java
@Component
public class JWTGenerator {

    private static final int expireInMs = 300 * 1000;

    private final static Key key = Keys.secretKeyFor(SignatureAlgorithm.HS256);

    public String generate(Authentication authentication) {
        String username = authentication.getName();

        return Jwts.builder().setSubject(username).setIssuer("adrien").setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + expireInMs)).signWith(key).compact();
    }

    public boolean validate(String token) {
        if (getUsername(token) != null && isExpired(token)) {
            return true;
        }
        return false;
    }

    public String getUsername(String token) {
        Claims claims = getClaims(token);
        return claims.getSubject();
    }

    public boolean isExpired(String token) {
        Claims claims = getClaims(token);
        return claims.getExpiration().after(new Date(System.currentTimeMillis()));
    }

    private Claims getClaims(String token) {
        return Jwts.parser().setSigningKey(key).parseClaimsJws(token).getBody();
    }
}
```


