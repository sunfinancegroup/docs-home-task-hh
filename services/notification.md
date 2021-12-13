# Notification

At least the following properties must exist on _Notification_ aggregate:

- `id`
- `recipient`
- `channel`
- `body`
- `dispatched` (default: false)

All the rest is up to your implementation.

    Please, note that "property" is not the same as "field" in the database.

## Task

Implement consumer which will do the following:

- consume `VerificationCreated` event from message broker;
- based on event received render relevant [_Template_](./template.md) via HTTP using `http://<private-app>:<port>/templates/render` endpoint;
- persist notification in storage.

Dispatch notification via appropriate channel:

- send email via _SMTP_ using [MailHog](https://github.com/mailhog/MailHog);
- send SMS via API using [Gotify](https://gotify.net/);
- update notification as `dispatched` on success.

### Storage

Choose whatever storage you like: RDBMS, document-oriented DB, memory or filesystem.

### Optional

- Emit `NotificationCreated` and `NotificationDispatched` domain events.
- Add support for retry mechanism if notification dispatching failed.

