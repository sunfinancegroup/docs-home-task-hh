# Home task from Hedgehog

Thank you for applying for an engineering position at _Sun Finance_.

## Preamble

The goal of this task is to test tactical and strategic Domain-Driven Design [1] skills of the candidate in close to real word (small) project.

## Intro

You will build the event driven _Verification_ system with public and private facing RESTish API [2] in Domain-Driven design manner.

Tooling:

- use the programming language of your choice e.g. _Java_, _Kotlin_, _PHP_;
- use the framework of your choice e.g. _Spring Boot_, _Ktor_, _Symfony_;
- use the storage of your choice e.g. _PostgreSQL_, _MySQL_, _MongoDB_, _SQLite_;
- use the message broker of your choice e.g. _RabbitMQ_, _Kafka_, _NATS_;
- use _Docker_.

Result evaluation:

- we expect to checkout your code from _Git_;
- running `docker-compose up` should start the project with schema and fixtures loaded;
- we should be able to access containers from local machine in order to evaluate the result;
- add README with your notes and setup instruction if required.

Hints:

- take your time, learn what you don't know;
- follow supplied references if your want to expand on some topic;
- if you have any questions don't hesitate to contact us (contact person will be provided to you);
- don't try to be perfect.

Additional services you will use:

- https://github.com/mailhog/MailHog - for Email verifications
- https://gotify.net - for SMS verifications

## Specification

The project will include three aggregates [3]: _Template_, _Notification_ and _Verification_ itself.

Think of it as 3 different services. However, you may share common interfaces, value objects or infrastructure code.

### Template

This part of the system acts like Open Host Service [4]. [Template](services/template.md) specification.

### Verification

Upsteam public facing part of the system. [Verification](services/verification.md) specification.

### Notification

Downstream consumer part of the system. [Notification](services/notification.md) specification.

## Optional

It is completely fine two stop right away, when above specification is implemented.

However, if you're feeling advantageous below are additional points to make your work perfect:

- implement unit tests for application [5] and domain services;
- implement outbox pattern [6];
- configure API Gateway [7] e.g. _Kong_ or _KrakenD_.

## References

 1. https://martinfowler.com/bliki/DomainDrivenDesign.html
 2. [Stick to Level 2 maturity model](https://developers.redhat.com/blog/2017/09/13/know-how-restful-your-api-is-an-overview-of-the-richardson-maturity-model)
 3. https://martinfowler.com/bliki/DDD_Aggregate.html
 4. http://ddd.fed.wiki.org/view/open-host-service
 5. https://martinfowler.com/eaaCatalog/serviceLayer.html
 6. https://microservices.io/patterns/data/transactional-outbox.html
 7. https://microservices.io/patterns/apigateway.html
