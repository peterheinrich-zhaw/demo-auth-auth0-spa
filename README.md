# Demo: Federated Authentication – SPA + Spring Boot API with Auth0

This project demonstrates federated authentication for a
Single Page Application (SPA) with a Spring Boot REST API backend.

The application uses token-based (JWT) authentication and is fully stateless.

---

## Architecture Overview

- Frontend: SPA (e.g. React, Vue, Angular)
- Backend: Spring Boot REST API
- Authentication handled by: SPA
- Identity Provider: Auth0 (OIDC)
- Auth state: JWT access tokens

---

## OAuth / OIDC Flow Used

Authorization Code Flow with PKCE

This is the recommended and modern flow for SPAs:

- No client secret required
- Designed for public clients
- Protects against authorization code interception

The legacy Implicit Flow is deprecated and not used.

---

## High-Level Authentication Flow

### User Authentication (SPA ↔ Auth0)

1. User opens the SPA
2. SPA redirects the browser to Auth0 /authorize with PKCE parameters
3. User authenticates (database or federated IdP)
4. Auth0 redirects back to the SPA with an authorization code
5. SPA exchanges the code + verifier for tokens

### API Access (SPA → Spring Boot)

6. SPA calls the API with:

 Authorization: Bearer <access_token>

7. Spring Boot validates the JWT
8. Spring Security creates a SecurityContext from token claims

---

## Tokens and State

- Access Token (JWT): Used to call the API
- ID Token: Used by the SPA for user identity
- Refresh Token (optional): Used to renew access tokens

No HTTP session is created on the backend.

---

## Auth0 Configuration

### SPA Application

- Application type: Single Page Application

#### Allowed Callback URLs

http://localhost:3000/callback

#### Allowed Logout URLs

http://localhost:3000

#### Allowed Web Origins

http://localhost:3000

---

### API Configuration (Auth0 API)

- Application type: API
- Identifier (Audience):

https://api.demo.local

This audience must match what the SPA requests and what Spring validates.

---

## Running Locally

This setup runs fully on localhost with two servers:

| Component | URL |
|----------|-----|
| SPA | http://localhost:3000 |
| REST API | http://localhost:8080 |

No deployment or tunneling is required.

---

## CORS Considerations

CORS is the most common local development issue.

The Spring Boot API must allow:

- Origin: http://localhost:3000
- Headers: Authorization, Content-Type
- HTTP methods: GET, POST, PUT, DELETE

This is required for authenticated API calls from the SPA.

---

## Spring Security Role

- Acts as an OAuth2 Resource Server
- Does not initiate logins
- Validates JWTs using Auth0 public keys
- Enforces authorization based on token claims

---

## Best Practices Demonstrated

- Stateless backend security
- Token-based authentication with JWTs
- Secure SPA authentication using PKCE
- Separation of authentication and resource server concerns

---

## Intended Audience

This project is intended for:

- Learning modern SPA authentication patterns
- Understanding JWT-based security
- Comparing token-based vs session-based auth
- Demonstrating federated login in SPAs

---