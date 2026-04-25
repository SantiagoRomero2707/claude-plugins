---
description: Implements the Java/Spring Boot solution layer by layer from the agreed design. Discusses each proposal before generating the final complete and compilable code.
---

Implement the solution using the architect's design available in the conversation. To generate the code for each layer, call the `implementer` agent with the accumulated context from the iterations.

## Before starting
Confirm you have the architect's design (package tree, classes, agreed patterns) and the problem analysis. If either is missing, ask the user for it.

## Iteration 1 — Domain layer
Propose entities with **real behavior** (not Anemic Domain Model), Value Objects, domain exceptions and application of Builder/Factory if agreed.
**Wait for feedback and approval before continuing.**

## Iteration 2 — Application layer
Propose use case interfaces (if Complex level), Application Services with business logic, implementation of Strategy/Template Method/Decorator if agreed. No Spring Web annotations in this layer.
**Wait for feedback and approval before continuing.**

## Iteration 3 — Persistence
Propose JPA repositories, JPA↔domain Adapter (if Complex level), H2 configuration for tests.
**Wait for feedback and approval before continuing.**

## Iteration 4 — HTTP layer
Propose `@RestController`, DTOs with Bean Validation, Facade if agreed.

### Exception handling — fit to the problem (YAGNI)
Use `@RestControllerAdvice` with a single `GlobalExceptionHandler`. The standard structure for every error response is an `ErrorResponse` Java record: `record ErrorResponse(int status, String error, String message) {}`.

Only add handlers that can actually be triggered by this specific solution:
- **Domain exceptions** (e.g. resource not found → 404, duplicate → 400): always needed, one handler per exception class thrown by the service.
- **`MethodArgumentNotValidException`** → 400: add only if the DTOs use `@Valid` with Bean Validation annotations. Collect all field errors with a stream and join them.
- **`HttpMessageNotReadableException`** → 400: add only if the endpoint accepts a request body that could arrive malformed.
- **`MethodArgumentTypeMismatchException`** → 400: add only if the endpoint has path variables with a non-String type (e.g. `Long id`).
- **Generic `Exception`** → 500: always add as a last-resort fallback to avoid leaking stack traces.

Do not add handlers for exceptions that cannot occur given the problem's endpoints and constraints.

**Wait for feedback and approval before continuing.**

## Final output (only when everything is agreed upon)
Complete and compilable Java code for all files organized by package, plus:
- `pom.xml` with dependencies (Spring Boot 3.x, Java 17+, H2, Bean Validation)
- Minimal functional `application.properties`

## Principles applied in every line
- **SOLID**: SRP per class, DIP (constructor injection, no `new` for services), ISP (cohesive interfaces)
- **KISS**: most straightforward implementation that works correctly
- **DRY**: no duplicated logic; extract to private methods or domain
- **YAGNI**: only implement what the problem statement asks for
- **Fail Fast**: `@Valid` in controllers, validation in Value Object constructors
- **Law of Demeter**: `order.calculateTotal()` not `order.getItems().stream()...`
- **No Anemic Domain Model**: entities have behavior, not just getters/setters
- **No Magic Numbers/Strings**: named constants or enums for fixed values

Suggest reviewing with `/hackerrank-java:review-principles`.
