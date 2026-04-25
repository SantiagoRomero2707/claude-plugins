---
description: Analyzes the statement of a HackerRank problem or technical test. Paste the statement as an argument or in the message.
---

Analyze the problem statement and produce a structured document. The input can arrive in three forms:

- **Text** in $ARGUMENTS: use it directly as the statement.
- **Image path** in $ARGUMENTS: read the file and extract the visible text.
- **Image in context** (passed from `list-problems`): extract all visible text. If there are multiple screenshots of the same problem, read them in the order they were passed and consolidate the complete text before analyzing — treat it as a single continuous statement.

Once you have the statement in text, call the `problem-analyzer` agent passing it the full text as context.

## Output structure

### 1. Problem summary
Description in 2-3 sentences of the system and its purpose.

### 2. Domain entities
List of entities with attributes and types:
- `Product { id: Long, name: String, price: BigDecimal, stock: Integer }`

### 3. Required endpoints
| Method | Path | Request Body | Response Body | Status codes |
|--------|------|-------------|---------------|--------------|

### 4. Business rules
Numbered list of each explicit or implicit rule from the statement.

### 5. Input validations
What to validate in each request (required fields, ranges, formats, types).

### 6. Error cases
What errors the API returns, under what condition and what HTTP status corresponds.

### 7. Edge cases
Empty lists, non-existent IDs, null values, duplicates, operations on deleted resources, concurrency.

### 8. Technical constraints
Pagination, authentication, date formats, size limits, sorting.

### 9. Ambiguities
Unclear points in the statement with the most reasonable interpretation proposed.

## Instructions
- Read the complete statement before producing any output.
- Do not assume specific technologies — only analyze the domain.
- Be exhaustive with edge cases; they are the ones that penalize the most in interviews.
- When finished, ask if there is anything to adjust before continuing with `/hackerrank-java:design-architecture`.
