---
description: Lists the problems available in the screenshots folder, shows the status of each one (pending/in_progress/solved) and allows selecting which one to work on. Pass the folder path as an argument or use ./problems/ by default.
---

Manages the queue of technical problems to solve.

## Step 1 — Locate the folder
Use the path in $ARGUMENTS if provided, or `./problems/` by default. If the folder does not exist, inform the user and stop.

## Step 2 — Read progress
Read the `progress.json` file inside the folder. If it does not exist, create it with `{}`.

## Step 3 — Scan the problems
The folder supports two formats, which can coexist:

- **Single screenshot**: direct image file (`.png`, `.jpg`, `.jpeg`, `.webp`)
- **Multiple screenshots**: subfolder with any number of images inside, sorted by name

```
problems/
├── progress.json
├── 01_inventory-api.png          ← 1-screenshot problem
├── 02_payment-service/           ← multiple screenshots problem
│   ├── 01_description.png
│   ├── 02_examples.png
│   └── 03_constraints.png
└── 03_orders-api/
    ├── 01_part1.png
    └── 02_part2.png
```

For each entry (file or subfolder), determine its status from `progress.json`:
- `solved` → ✅
- `in_progress` → 🔄
- absent from the JSON → ⏳ pending

## Step 4 — Display the list
Present the sorted list with numbering, status and screenshot count if more than one:

```
Problems available in ./problems/
─────────────────────────────────────────────────────
[1] ✅  01_inventory-api.png              (1 screenshot)
[2] 🔄  02_payment-service/               (3 screenshots)
[3] ⏳  03_orders-api/                    (2 screenshots)

Solved: 1 / 3  |  In progress: 1  |  Pending: 1
```

## Step 5 — User selection
Ask the user which number they want to work on. Also accepts the file name directly.

## Step 6 — Update progress.json
Mark the selected problem as `in_progress` in `progress.json`.

## Step 7 — Start the analysis
According to the type of the selected problem entry:
- **Single file**: read the image and pass it to `problem-analysis` as context.
- **Subfolder**: read all images inside in alphabetical order and pass them all to `problem-analysis` as context. Indicate how many screenshots make up the problem so the agent can consolidate them before analyzing.
