---
description: Audits the implemented Java code against design principles and anti-patterns. Reviews SOLID, KISS, DRY, YAGNI, Law of Demeter, Fail Fast and detects STUPID, Anemic Domain Model, God Class, Primitive Obsession and Magic Numbers.
---

Audit the code available in $ARGUMENTS or in the working directory. To execute the complete review, call the `code-reviewer` agent with the code as context.

## Report structure

### ✅ Section 1 — Principles met
List principles the code respects with concrete evidence.

### ❌ Section 2 — SOLID violations
For each violation: principle (S/O/L/I/D), affected class/method, description and corrected Java code.
- **S**: multiple reasons to change, methods that do too much
- **O**: `if/switch` on types instead of polymorphism
- **L**: subclasses that break the parent's contract
- **I**: fat interfaces with methods not all implementations use
- **D**: `new ConcreteService()` in constructors, hardcoded dependencies

### ❌ Section 3 — KISS / DRY / YAGNI violations
- **KISS**: methods > 20 lines, unjustified abstractions
- **DRY**: duplicated logic (show both occurrences)
- **YAGNI**: code that does not correspond to any requirement in the statement

### ❌ Section 4 — Clean Architecture violations
Dependencies in the wrong direction, framework annotations in pure domain, business logic in controllers or repositories.

### ❌ Section 5 — STUPID anti-patterns
| Anti-pattern | What to look for |
|---|---|
| **S — Singleton** | Mutable `static` instances, global state |
| **T — Tight Coupling** | `new ServiceImpl()` inside another service |
| **U — Untestable** | `static` methods with logic, `new Date()` without abstraction |
| **P — Premature Optimization** | Manual cache without evidence of a problem |
| **I — Indescriptive Naming** | `data`, `obj`, `doStuff()`, single-letter variables |
| **D — Duplication** | Copy-paste between classes, repeated mapping logic |

### ❌ Section 6 — Additional anti-patterns
- **Anemic Domain Model**: entities without behavior, domain logic in services
- **God Class**: >150-200 lines of logic, >4-5 dependencies
- **Primitive Obsession**: `String email`, `double price` where a proper type is needed
- **Magic Numbers/Strings**: `if (status == 3)`, `"ACTIVE"` hardcoded

### ⚠️ Section 7 — Additional principles
- **Law of Demeter**: chains `obj.getA().getB().doX()` → delegate with a method at the root
- **Fail Fast**: validations in inner layers instead of the controller/DTO boundary

### 📋 Section 8 — Executive summary
| Severity | Count | Description |
|---|---|---|
| 🔴 Critical | N | Breaks design or makes code untestable |
| 🟡 Important | N | Degrades quality without blocking functionality |
| 🟢 Minor | N | Style or naming improvements |

**Verdict**: `APPROVED` / `APPROVED WITH OBSERVATIONS` / `REQUIRES CHANGES`

If the verdict is approved, suggest continuing with `/hackerrank-java:generate-tests`.
