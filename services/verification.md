# Verification

_Verification_ is something that verifies user action e.g. you may confirm an email belongs to a client or sign a contract via SMS.

At least the following properties must exist on the aggregate:

- `id`
- `subject`
- `confirmed` (default: `false`)
- `code` (numeric field with up to 8 digits)
- `userInfo`

All the rest is up to your implementation.

    Please, note that "property" is not the same as "field" in the database.

Subject consists of two parts:

- `identity`,
- and `type`.

Implement support for two subject types `email_confirmation` and `mobile_confirmation` with `identity` being email address

```
{
  "identity": "john.doe@abc.xyz",
  "type": "email_confirmation"
}
```

and mobile phone respectively

```
{
  "identity": "+37120000001",
  "type": "mobile_confirmation"
}
```

Save client's IP address and user agent as `userInfo`.

Numeric code is never exposed via REST API.

## Business rules

1. Each verification is only valid for 5 minutes until confirmed or expired.
2. It must not be possible to create a duplicated "pending" verification for the same subject.
3. Verification can only be confirmed if `clientInfo` matches.
4. When invalid confirmation code is supplied 5 times verification automatically expires.
5. Verification confirmation is idempotent [1].

## Configuration

Verification code length (up to 8 digits) and expiration ttl (default 5 minutes) must be configured via enviroment variables.

## Task

You need to implement two public API REST endpoints.

### 1. Creating new verification

```bash
curl -i -X POST -H 'Content-Type: application/json' http://<public-app>:<port>/verifications -d '{
  "subject": {
    "identity": "+37120000001",
    "type": "mobile_confirmation"
  }
}'

HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "b249b759-11a7-4b8b-a38d-e53d193d4e90"
}
```

#### Status codes

| Code | Description                                  |
| -----| -------------------------------------------- |
| 201  | Verification created.                        |
| 400  | Malformed JSON passed.                       |
| 409  | Duplicated verification.                     |
| 422  | Validation failed: invalid subject supplied. |

### 2. Confirming verification

```bash
curl -i -X PUT -H 'Content-Type: application/json' http://<public-app>:<port>/verifications/b249b759-11a7-4b8b-a38d-e53d193d4e90/confirm -d '{
  "code": "727468"
}'

HTTP/1.1 204 No Content
```

#### Status codes

| Code | Description                               |
| -----| ----------------------------------------- |
| 204  | Verification confirmed.                   |
| 400  | Malformed JSON passed.                    |
| 403  | No permission to confirm verification.    |
| 404  | Verification not found.                   |
| 410  | Verification expired.                     |
| 422  | Validation failed: invalid code supplied. |

## Storage

Choose RDBMS you like.

## Events

Any mutation of aggreage must emit a corresponding domain event [2] to the message broker. Make sure following domain events are emitted:

- `VerificationCreated`
- `VerificationConfirmed`
- `VerificationConfirmationFailed`

with at least bellow properties:

- `id`
- `code`
- `subject`
- `occurredOn`

## References

 1. https://restfulapi.net/idempotent-rest-apis/
 2. https://martinfowler.com/eaaDev/DomainEvent.html
