![[{FB486C91-55BB-4F7F-84C2-B312B5B4ABA2}.png]]


### What is JWT?

- JSON Web Token (JWT) is an open standard (RFC 7519) for securely transmitting information between parties as a JSON object.
- The token is **self-contained**, meaning it contains all the information needed to verify its authenticity.

---

### Structure of a JWT: 
JWT consists of three parts, separated by dots (`.`):

1. **Header**:
    - Contains metadata about the token, including the type (JWT) and the signing algorithm (e.g., HMAC, RSA).
    - Example:
        ```json
        {
          "alg": "HS256",
          "typ": "JWT"
        }
        ```
        
2. **Payload**:
    - Contains the claims, which are statements about an entity (e.g., user) and additional metadata.
    - Claims can be:
        - **Registered Claims**: Predefined claims (e.g., `iss`, `exp`, `sub`, `aud`).
        - **Public Claims**: Custom claims defined by the user.
        - **Private Claims**: Custom claims shared between parties.
    - Example:
        ```json
        {
          "sub": "1234567890",
          "name": "John Doe",
          "admin": true
        }
        ```
        
3. **Signature**:
    - Ensures the integrity of the token and that it hasn't been tampered with.
    - Created by encoding the header and payload and signing them with a secret or private key.
    - Example:
        ```
        HMACSHA256(
          base64UrlEncode(header) + "." +
          base64UrlEncode(payload),
          secret
        )
        ```
        

A JWT looks like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

---

### How JWT Works:

1. **Authentication**:
    - User logs in and is authenticated by the server.
    - Server generates a JWT containing the user’s information and signs it.
    - The JWT is sent to the client.
    
1. **Authorization**:
    - Client includes the JWT in subsequent requests (e.g., as a Bearer Token in the Authorization header).
    - Server validates the token and grants or denies access based on the claims.

---

### Advantages of JWT:

1. **Stateless Authentication**:
    - No need to store session information on the server.
2. **Compact**:
    - Small in size, easily transmitted via URLs, HTTP headers, or cookies.
3. **Secure**:
    - Ensures data integrity with digital signatures.
4. **Cross-Domain Compatibility**:
    - Ideal for single sign-on (SSO) and cross-domain applications.

---

### Challenges with JWT:

1. **Revocation**:
    - JWTs cannot be invalidated until they expire unless additional mechanisms (e.g., a revocation list) are used.
2. **Size**:
    - Larger than session tokens due to included payload.
3. **Expiration**:
    - Short expiration times require frequent regeneration, while long times increase security risks.

---

### Best Practices:

1. **Use HTTPS**:
    - Always transmit JWTs over secure channels.
2. **Set Expiration (`exp`)**:
    - Limit token validity to reduce risk if compromised.
3. **Secure the Secret**:
    - Protect signing keys to prevent tampering.
4. **Avoid Sensitive Data**:
    - Do not include sensitive information in the payload.

---

### Common Use Cases:

1. **Single Sign-On (SSO)**:
    - Allows users to log in once and access multiple applications.
2. **RESTful APIs**:
    - Secure communication between client and server.
3. **Microservices**:
    - Authenticate and authorize requests across distributed systems.