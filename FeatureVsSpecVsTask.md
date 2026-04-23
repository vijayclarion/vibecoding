# Feature plan.md — JWT refresh token rotation - Why are we building it

Answers Why are we building this?
Audience PM, stakeholders, team
Detail level High-level, no code

Feature: User auth refresh token
Goal: Allow users to stay logged in without re-entering credentials
every 15 minutes.

## Problem
Access tokens expire in 15 min for security. Users are being logged
out mid-session, causing complaints (30% of support tickets).

## Proposed solution
Implement refresh token rotation in the ASP.NET Core auth pipeline.
- Issue a short-lived access token (15 min)
- Issue a long-lived refresh token (7 days, stored in HttpOnly cookie)
- On access token expiry, silently issue new pair via /auth/refresh
- Revoke old refresh token on each rotation (detect token reuse)

## Out of scope
- OAuth / social login (separate feature)
- Multi-device session management

## Success criteria
- Zero mid-session logouts for active users
- Refresh cycle invisible to the user
- Token reuse attack detected and session revoked

## Dependencies
- Existing JWT middleware (JwtBearerHandler)
- SQL Server: RefreshTokens table needed

## Effort estimate
~3 days backend, ~0.5 days frontend integration



# Spec.md — JWT refresh token rotation
Answers What must be true exactly?
Audience Architect, senior dev, AI
Detail level Precise, no ambiguity

## Data model
Table: RefreshTokens
  Id         UNIQUEIDENTIFIER PK DEFAULT NEWID()
  UserId     UNIQUEIDENTIFIER FK → AspNetUsers.Id
  Token      NVARCHAR(500) NOT NULL UNIQUE
  ExpiresAt  DATETIME2 NOT NULL
  RevokedAt  DATETIME2 NULL
  ReplacedBy NVARCHAR(500) NULL
  CreatedAt  DATETIME2 NOT NULL DEFAULT GETUTCDATE()

## Endpoints
POST /auth/login
  Request:  { email: string, password: string }
  Response: 200 { accessToken: string, expiresIn: 900 }
           + Set-Cookie: refreshToken=; HttpOnly; Secure; SameSite=Strict
  Errors:   401 { error: "invalid_credentials" }

POST /auth/refresh
  Request:  Cookie: refreshToken= (no body)
  Response: 200 { accessToken: string, expiresIn: 900 }
           + Set-Cookie: refreshToken=; ...
  Errors:   401 { error: "token_reuse_detected" }  ← revokes entire family
            401 { error: "token_expired" }

POST /auth/logout
  Request:  Cookie: refreshToken=
  Response: 204 No Content
           + Set-Cookie: refreshToken=; Max-Age=0

## Interface contract (C#)
public interface IRefreshTokenService
{
    Task<string> CreateAsync(Guid userId);
    Task<(bool valid, Guid userId)> ValidateAsync(string token);
    Task RevokeChainAsync(string token); // on reuse attack
    Task RevokeAsync(string token);      // on logout
}

## Access token spec
- Algorithm: HS256
- Claims: sub (userId), email, roles[], iat, exp
- Lifetime: 900 seconds
- Key: IConfiguration["Jwt:Secret"] min 32 chars

## Acceptance criteria
- [ ] Refresh token is HttpOnly — not readable via JS
- [ ] Each rotation issues a NEW token, immediately revokes old one
- [ ] Token reuse (replaying an old token) revokes entire family
- [ ] Tokens expired in DB are rejected with 401
- [ ] POST /auth/logout clears the cookie and revokes token in DB
- [ ] Access token claims include sub, email, roles


# Tasks.md — JWT refresh token rotation
Answers What do I build next?
Audience Developer (or AI agent) Detail level Actionable, ordered steps

## Task 1 — DB migration
- [ ] Add EF Core migration: AddRefreshTokensTable
- [ ] Columns match spec schema exactly
- [ ] Add index on Token column (unique)
- [ ] Add index on UserId + RevokedAt (for cleanup queries)
Done when: dotnet ef migrations add runs clean, db updated.

## Task 2 — RefreshTokenService
- [ ] Create Services/RefreshTokenService.cs
- [ ] Implement IRefreshTokenService interface from spec
- [ ] CreateAsync: generate cryptographically random 64-byte token
      (Convert.ToBase64String(RandomNumberGenerator.GetBytes(64)))
- [ ] ValidateAsync: check exists + not revoked + not expired
- [ ] RevokeAsync: set RevokedAt = UtcNow
- [ ] RevokeChainAsync: traverse ReplacedBy chain, revoke all
Done when: unit tests for all 4 methods pass.

## Task 3 — AuthController endpoints
- [ ] POST /auth/login: validate creds, issue access token + refresh cookie
- [ ] POST /auth/refresh: validate cookie token, rotate, return new access token
- [ ] POST /auth/logout: revoke token, clear cookie
- [ ] Wire RefreshTokenService via DI in Program.cs
Done when: integration test hits all 3 endpoints, correct status codes.

## Task 4 — Token reuse detection
- [ ] In /auth/refresh: if token is already revoked → call RevokeChainAsync
- [ ] Return 401 { error: "token_reuse_detected" }
- [ ] Log warning with userId and token family id
Done when: test replays old token → 401, all family tokens revoked in DB.

## Task 5 — Middleware / token validation
- [ ] Configure JwtBearerOptions in Program.cs (HS256, validate lifetime)
- [ ] Add [Authorize] to protected controllers
- [ ] Confirm 401 on expired access token (not 403)
Done when: expired access token returns 401, valid token returns 200.

## Task 6 — Cleanup job (optional, v1.1)
- [ ] Hangfire or IHostedService background job
- [ ] Delete rows where ExpiresAt < UtcNow - 30 days
Done when: job runs on schedule, old rows removed.
