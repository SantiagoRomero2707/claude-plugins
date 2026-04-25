---
description: Shows a token consumption summary by phase at the end of the plugin's complete flow. Presents a table with input tokens, output tokens and total for each skill executed in the session.
---

The user wants to see the token consumption summary for all phases executed in this session.

Review the current conversation and identify each agent invocation of the `hackerrank-java` plugin that was executed. For each one, collect the input and output tokens reported in the usage metadata (`usage.input_tokens`, `usage.output_tokens`).

Present the summary table in this format:

```
╔══════════════════════════════╦══════════════╦═══════════════╦═════════════╗
║ Phase                        ║ Input tokens ║ Output tokens ║ Total       ║
╠══════════════════════════════╬══════════════╬═══════════════╬═════════════╣
║ analyze-problem              ║        x,xxx ║         x,xxx ║      x,xxx  ║
║ design-architecture          ║        x,xxx ║         x,xxx ║      x,xxx  ║
║ implement-solution           ║        x,xxx ║         x,xxx ║      x,xxx  ║
║ review-principles            ║        x,xxx ║         x,xxx ║      x,xxx  ║
║ generate-tests               ║        x,xxx ║         x,xxx ║      x,xxx  ║
╠══════════════════════════════╬══════════════╬═══════════════╬═════════════╣
║ SESSION TOTAL                ║        x,xxx ║         x,xxx ║      x,xxx  ║
╚══════════════════════════════╩══════════════╩═══════════════╩═════════════╝
```

Only include phases that were actually executed in this session. If a phase was not executed, omit it from the table.

Below the table, add a brief note about which phase consumed the most tokens and why (usually `implement-solution` due to the volume of code generated).
