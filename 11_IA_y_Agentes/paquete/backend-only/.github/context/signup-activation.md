# Signup and Activation Contract

## 1. Purpose and Scope

This document defines the canonical backend contract for account signup and activation in the Quiniela project.

It covers:
- public account registration;
- payment proof submission;
- administrative proof review;
- user activation and rejection;
- login eligibility rules;
- backend security and operational constraints for the flow.

It does not define UI layout or frontend component structure. The frontend may present these states, but it does not define them.

## 2. Source of Truth and Precedence

- The backend is the source of truth for account state, activation, authorization, and transitions.
- Product decisions documented in `01_Producto_y_Reglas` take precedence over implementation preferences.
- More recent sprint decisions take precedence over older documents if there is a conflict.
- If implementation details conflict with this document, this document wins unless a newer sprint decision explicitly overrides it.

## 3. Ownership and Document Status

- Owner: backend domain and activation flow.
- Status: canonical project contract.
- Scope: product rules plus backend API contract.

## 4. Business Rules

### 4.1 Official user states

The only official user states are:

- `PENDIENTE`: the account was created, but activation has not yet been approved.
- `ACTIVO`: the account was approved by an admin and can participate normally.
- `RECHAZADO`: the admin denied activation.
- `BLOQUEADO`: the admin revoked access after the account had already been active.

### 4.2 Important separations

- Payment review is not a user state.
- Review state belongs to the activation request or proof, not to the user account itself.
- If a proof is rejected and the product later allows a retry, that retry must be modeled in the activation request flow, not by inventing a new user state.
- The user does not become `ACTIVO` until an admin explicitly approves the proof.

### 4.3 Activation rule

- Registration does not activate the account.
- A user is only eligible to log in for normal operation after admin approval.
- Only `ACTIVO` users can compete and generate valid picks.

## 5. State Machines

### 5.1 User state machine

Allowed transitions:

- `PENDIENTE` -> `ACTIVO`
- `PENDIENTE` -> `RECHAZADO`
- `ACTIVO` -> `BLOQUEADO`

Implicit rule:
- a new registration starts at `PENDIENTE`

Forbidden transitions:

- `ACTIVO` -> `PENDIENTE`
- `RECHAZADO` -> `ACTIVO` unless a new explicit product decision is documented
- `BLOQUEADO` -> `ACTIVO` unless a new explicit product decision is documented
- any transition not listed above

### 5.2 Payment proof / activation request state

This flow is separate from the user state.

Recommended internal states:

- `EN_REVISION`: proof was submitted and is waiting for admin review
- `APROBADO`: proof was approved and the user became active
- `RECHAZADO`: proof was rejected

If the product later decides to support retry after rejection, that behavior should be expressed here, not in the user state machine.

## 6. Canonical Flow

1. The user registers.
2. The backend creates the account in `PENDIENTE`.
3. The user pays outside the system.
4. The user uploads the payment proof.
5. The backend stores the proof metadata and a private reference.
6. The backend marks the activation request as pending review.
7. An admin reviews the proof.
8. The admin approves or rejects the request.
9. If approved, the user becomes `ACTIVO`.
10. If rejected, the user becomes `RECHAZADO`.
11. Only `ACTIVO` users may log in and use competitive features.

## 7. API Contract

### 7.1 `POST /users/register`

Creates the initial account.

Implementation note:

- the current repository already exposes this registration flow as `POST /users`;
- `POST /users/register` is the logical name of the operation in the contract, but this sprint does not require a separate compatibility alias to understand or implement the slice.

#### Request body

```json
{
  "nombre": "Diego Lopez",
  "email": "diego@email.com",
  "password": "strong-password",
  "acceptedTerms": true
}
```

#### Allowed fields

- `nombre`
- `email`
- `password`
- `acceptedTerms` must be `true`

#### Disallowed fields

- `role`
- `activo`
- `id`
- `estado`
- `createdAt`
- `updatedAt`
- any other system-owned field

#### Expected behavior

- create the user in `PENDIENTE`
- set `role = USER`
- require `acceptedTerms = true` and persist legal acceptance metadata in backend
- do not issue a session cookie
- do not auto-login
- do not allow the client to override account state or role
- leave the user ready for activation proof upload

#### Successful response

```json
{
  "message": "Cuenta creada. Pendiente de activacion.",
  "usuario": {
    "id": "usr_123",
    "nombre": "Diego Lopez",
    "email": "diego@email.com",
    "estado": "PENDIENTE"
  }
}
```

### 7.2 `POST /users/activation-proof`

Submits the payment proof for admin review.

Implementation note:

- the current repository already exposes this proof submission flow as `POST /payments/proofs`;
- `POST /users/activation-proof` is the logical name of the operation in the contract, but the implementation surface stays in `payments`.

#### Request

```json
{
  "storageKey": "payments/proofs/proof-001.png",
  "fileName": "proof-001.png",
  "mimeType": "image/png",
  "fileSizeBytes": 48123,
  "fileHash": "sha256-proof-001"
}
```

#### Rules

- only `PENDIENTE` users may submit a proof
- the backend stores metadata and a private reference, not the binary
- the user remains `PENDIENTE`
- no session is issued
- the activation request becomes pending admin review
- the storage provider or upload mechanism is not defined here
- allowed file types: PDF, PNG, JPEG
- maximum file size: 5 MiB

#### Successful response

```json
{
  "message": "Comprobante recibido. Queda en revision administrativa.",
  "solicitud": {
    "id": "act_456",
    "estadoRevision": "EN_REVISION"
  }
}
```

### 7.3 `GET /payments/proofs`

Lists activation proofs for admin review.

#### Rules

- admin-only review queue
- includes user, timestamp, proof metadata, and review state
- may be filtered by review state if needed by the admin surface
- does not expose a separate participant "my proofs" surface in this sprint

#### Successful response

```json
{
  "items": [
    {
      "id": "act_456",
      "usuario": {
        "id": "usr_123",
        "nombre": "Diego Lopez",
        "email": "diego@email.com"
      },
      "estadoRevision": "EN_REVISION",
      "createdAt": "2026-04-18T12:00:00Z"
    }
  ]
}
```

### 7.4 `PATCH /payments/proofs/:paymentProofId/review`

Reviews a proof and resolves the activation outcome.

#### Request body

```json
{
  "resolution": "APROBADO",
  "reviewReason": "El comprobante no es legible."
}
```

#### Rules

- admin only
- the proof must exist
- the proof must be in a reviewable state
- the request must carry an explicit resolution of `APROBADO` or `RECHAZADO`
- if the resolution is `RECHAZADO`, the reason is required
- the user becomes `ACTIVO` when approved
- the user becomes `RECHAZADO` when rejected
- the action must be audited with actor and timestamp

#### Successful response when approved

```json
{
  "message": "Cuenta activada correctamente.",
  "usuario": {
    "id": "usr_123",
    "estado": "ACTIVO"
  }
}
```

#### Successful response when rejected

```json
{
  "message": "Solicitud rechazada.",
  "usuario": {
    "id": "usr_123",
    "estado": "RECHAZADO"
  }
}
```

### 7.6 `POST /users/login`

Authenticates only active accounts.

#### Request body

```json
{
  "email": "diego@email.com",
  "password": "strong-password"
}
```

#### Rules

- only `ACTIVO` users can log in
- `PENDIENTE`, `RECHAZADO`, and `BLOQUEADO` must not receive a session
- error messages should be generic to avoid account enumeration

#### Successful response

```json
{
  "message": "Inicio de sesion correcto.",
  "usuario": {
    "id": "usr_123",
    "nombre": "Diego Lopez",
    "estado": "ACTIVO"
  }
}
```

## 8. Security and OWASP Alignment

This flow should follow the following security principles:

- allowlist validation on every input
- canonicalization before validation where needed
- no trust in client-side role or state fields
- strong password hashing
- no auto-login at registration time
- generic error messages to reduce enumeration
- rate limiting on registration, login, and proof upload
- secure cookies only after real authentication
- admin approval must be audited
- file uploads must be validated by type, size, and storage policy

### 8.1 Password storage

Prefer a modern memory-hard password hashing algorithm when possible.

If a legacy algorithm is used, it must be configured with a strong cost factor and its limits must be respected.

### 8.2 Session handling

- issue a session only after successful login
- use secure cookie flags
- do not expose session material to the client body

### 8.3 File upload handling

- store proof files in private storage
- store metadata and reference in the database
- do not trust the original filename
- validate the file size and type before storage
- accept only PDF, PNG, or JPEG files
- reject files larger than 5 MiB

## 9. Storage and Evidence Policy

- The file lives in private object storage or equivalent.
- The database stores metadata and a private reference only.
- Recommended metadata fields:
  - reference key
  - file name
  - mime type
  - file size
  - file hash
  - review state
  - reviewer reference
  - review reason

### 9.1 Legal acceptance tracking

- Legal acceptance tracking is stored in backend, not only in UX.
- Registration must persist the acceptance timestamp and the agreed terms version when `acceptedTerms` is true.
- The client may display the consent UI, but the backend owns the durable record.

## 10. Retry Policy

For Sprint 3, retry after rejection is closed as follows:

- a rejected proof does not reopen automatic retry in the same activation attempt;
- the rejected attempt is considered closed for this sprint;
- any future retry policy must be introduced as an explicit product decision before implementation.

The backend therefore keeps the one-pending-proof-per-user rule as the operational control for this slice.

## 11. Audit

The following actions must be auditable:

- account registration;
- proof submission;
- proof approval;
- proof rejection;
- state transition to `ACTIVO`;
- state transition to `RECHAZADO`;
- state transition to `BLOQUEADO`;
- administrative access to review surfaces.

Audit payload should capture:
- actor type;
- actor user id;
- target entity type;
- target entity id;
- request id if available;
- outcome or resolution;
- minimal business metadata needed for traceability.

## 12. Standard Errors

### Validation error

```json
{
  "error": "validation_error",
  "message": "Los datos enviados no son validos."
}
```

### Conflict

```json
{
  "error": "conflict",
  "message": "No fue posible completar la operacion."
}
```

### Authentication required

```json
{
  "error": "authentication_required",
  "message": "No fue posible completar la operacion."
}
```

### Forbidden

```json
{
  "error": "forbidden",
  "message": "La cuenta no esta habilitada para iniciar sesion."
}
```

### Invalid state

```json
{
  "error": "invalid_state",
  "message": "La solicitud no puede procesarse en este estado."
}
```

### Not found

```json
{
  "error": "not_found",
  "message": "El recurso solicitado no existe."
}
```

### Too many requests

```json
{
  "error": "too_many_requests",
  "message": "Se realizaron demasiados intentos de registro."
}
```

## 13. Implementation Notes

- This document defines business behavior, not repository structure.
- The backend repo should consume this contract as source of truth.
- The frontend may reflect the states, but it must not define them.
- If a future sprint defines a more specific rule, that newer sprint decision takes precedence.

## 14. Criteria of Acceptance

- A new account starts in `PENDIENTE`.
- The backend does not auto-login users on registration.
- A payment proof can be submitted only by `PENDIENTE` users.
- An admin can approve or reject a proof and the user transitions accordingly.
- Only `ACTIVO` users can log in successfully for normal operation.
- The file upload contract stores metadata and a private reference, not the binary in the database.
- The flow leaves an audit trail for every critical administrative decision.
- This sprint does not introduce a new session lifecycle or a new refresh-token policy.
- This sprint does not enable automatic retry after a rejected proof.

## 15. Open Questions

None for QH-93 in this slice.
