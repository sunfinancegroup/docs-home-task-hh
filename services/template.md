# Template

There are two types of templates: plain text and HTML.

Two templates must be already pre-filled in storage:

- [mobile verification](../templates/sms-verification.txt),
- [email verification](../templates/email-verification.html).

## Task

You need to implement a single private API REST endpoint.

### 1. Template rendering

#### Plain template rendering

```bash
curl -i -X POST -H 'Content-Type: application/json' http://<private-app>:<port>/templates/render -d '{
  "slug": "mobile-verfication",
  "variables": {"code": "1234"}
}'

HTTP/1.1 200 OK
Content-Type: text/plain; charset=UTF-8
Content-Length: 32

Your verification code is 1234.
```

#### HTML template rendering

```bash
curl -i -X POST -H 'Content-Type: application/json' http://<private-app>:<port>/templates/render -d '{
  "slug": "email-verfication",
  "variables": {"code": "1234"}
}'

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 289

<!DOCTYPE html>
<html>
<head>
    <title>Email verification</title>
    <style>
        .content {
            margin: auto;
            width: 600px;
        }
    </style>
</head>
<body>
    <div class="content">
        <p>Your verification code is 1234.</p>
    </div>
</body>
</html>
```

#### Status codes

| Code | Description                                              |
| -----| -------------------------------------------------------- |
| 200  | Template rendered.                                       |
| 400  | Malformed JSON passed.                                   |
| 404  | Template not found.                                      |
| 422  | Validation failed: invalid / missing variables supplied. |

### Storage

Choose whatever storage you like: RDBMS, document-oriented DB, memory or filesystem. At least the following properties must exist on the aggregate:

- `id`
- `slug`
- `content`

It is up to you how to design the aggregate, what value objects [1] to implement, etc.

    Please, note that "property" is not the same as "field" in the database.

### Templating engine

Delimeters `{{` and `}}` can be replaced with something else. You can use a third-party templating Engine if you want.

### Optional

Add support for arbitrary variables schema. For example a _welcome_ email template may require variables like `firstName` and `lastName`.

## References

 1. https://martinfowler.com/bliki/ValueObject.html
