# Tabletop Inject Simulation (GitHub Actions)

This repository automatically simulates incident injects for tabletop exercises.

Each inject is read line-by-line from random files (inject-1.txt, inject-2.txt, …) and posted as GitHub Issues.

## How it works
1. **Inject source files:**

- Place one or more files named inject-X.txt in the repository root.

- Each line = one inject.

2. Workflow:

- The GitHub Action (`.github/workflows/scheduled-issues.yml`) runs on a schedule or manually.

  - On the first run, it randomly chooses one inject file.

  - Each subsequent run continues from the next line in that same file.
  - When the file ends, a summary issue is posted and the process resets.

3. State persistence:
- The current inject and line number are stored in a small JSON file:
  - `state.json`

- Example content:

  - `{"current_inject": "inject-3.txt", "index": 2}`
- _Note: This file is committed automatically after each run_

## Running the workflow

1. **Manual start:**

- Go to Actions → Tabletop Incident Injects → Run workflow.

- Choose:
  - `reset=true` → start a new random inject file.
  - `reset=false` → continue the current one.

2. **Automatic schedule:**
- The workflow attempts to run every 6 minutes.
  - _Important: Free account limits (like this one) will probably delay this 6 minutes (sometimes to even 30 minutes), so sometimes would be better to manually launch them one by one._

## Resetting everything
If something goes wrong or you want to restart from scratch:
- Delete state.json manually from the repo.
- Run the workflow with `reset=true`.

### Example output

- Each inject line appears as a new GitHub Issue titled like:

  - `🚨 [inject-3.txt] Line 4 - 2025-11-03 11:20:33 UTC`

- Includes randomized user/host/IP metadata to simulate an alert.


_By Mauricio Díaz_

