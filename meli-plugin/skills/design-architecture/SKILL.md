---
description: Designs the Java/Spring Boot architecture for the analyzed problem. Evaluates complexity, proposes the appropriate style and iteratively discusses GoF patterns before generating the final design.
---

Design the solution architecture using the analysis produced by `problem-analysis` available in the conversation. To generate complex design artifacts, call the `architect` agent.

## Step 0 — Complexity evaluation (ALWAYS FIRST)

Evaluate the problem and propose the appropriate level. **Wait for user confirmation before continuing.**

| Level | Signals | Package structure |
|-------|---------|----------------------|
| **Simple** | Basic CRUD, 1-2 entities, minimal rules | `controller/` `service/` `repository/` `model/` |
| **Medium** | 2-4 entities, non-trivial validations, moderate logic | `controller/` `service/` `repository/` `domain/` `dto/` |
| **Complex** | Multiple use cases, rich domain, isolation required | `domain/` `application/` `infrastructure/` `shared/` (Hexagonal) |

## Iteration 1 — Entities and creational patterns
Propose entities according to the agreed level and detect opportunities for:
- **Builder**: objects with more than 4-5 fields or complex construction
- **Factory Method**: creation with business logic or validations
- **Value Objects**: if there is Primitive Obsession (`Email`, `Money`, `OrderStatus`)

**Wait for feedback before continuing.**

## Iteration 2 — Services, ports and behavioral patterns
According to the level: direct `@Service` (Simple/Medium) or input/output ports (Complex).
Propose if applicable:
- **Strategy**: interchangeable business rules
- **Template Method**: flow with variable steps
- **Decorator**: cross-cutting behavior (logging, auditing)
- **Observer/Events**: side effects after operations
- **Facade**: orchestration of multiple subsystems

**Wait for feedback before continuing.**

## Iteration 3 — Data access
Repository Pattern at all levels:
- Simple/Medium: direct `JpaRepository`
- Complex: output port + JPA↔domain Adapter
- Propose **CQRS** only if there is a clear read/write separation

**Wait for feedback before continuing.**

## Iteration 4 — HTTP layer
Controllers, DTOs with Fail Fast validations (`@Valid`, `@NotNull`, `@Size`), global `@ControllerAdvice`, HTTP status → domain exception mapping. Propose **Facade** if the controller orchestrates more than 2-3 services.

**Wait for feedback before continuing.**

## Final output (only when everything is agreed upon)
1. Complete package tree with all `.java` files
2. Class list with responsibility and associated pattern
3. Justification of key architectural decisions
4. Mermaid diagrams saved to a `design/` folder at the project root:

   - **`design/class-diagram.md`** — UML class diagram with all classes (controller, service, repository, entity, DTOs, exceptions, `ErrorResponse` record), their fields, methods and relationships (`-->`, `..>`, `-->`).
   - **`design/component-diagram.md`** — Flowchart showing the layered architecture: Client → HTTP layer (Controller + GlobalExceptionHandler) → Application layer (Service) → Persistence layer (Repository) → Domain model (Entity + enums) → DTOs → DB tables.
   - **`design/er-diagram.md`** — ER diagram with all tables, columns (name, type, nullable, PK/FK), and relationships. Include the `@ElementCollection` table if the entity has one.
   - **`design/infrastructure-diagram.md`** — Deployment diagram: JVM process, Spring Boot with embedded Tomcat, H2 in-memory DB (runtime vs. test schema), exposed ports, and API clients (Swagger UI, REST client, H2 console).
   - **`design/sequence-diagram.md`** — One sequence diagram per endpoint covering the full happy path and the main error path (e.g., duplicate on POST, not-found on GET/DELETE). Show all layers: Client → Controller → Service → Repository → DB.

Suggest continuing with `/hackerrank-java:implement-solution`.
