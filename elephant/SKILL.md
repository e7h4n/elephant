---
name: elephant
description: Search through archived agent run logs in .memory/ to recall what was done before and learn from past problem-solving approaches.
---

# Searching Past History

elephant automatically archives all past agent run logs by date into the `.memory` directory in the working directory. The structure is:

```
.memory/
  2025-01-15/
    <runId>.log
    <runId>.log
  2025-01-16/
    <runId>.log
  ...
```

Each `.log` file contains the complete log of a single agent run, named by its run ID.

When you need to look back at history (e.g. debugging an issue, recalling what was done before, finding context from a previous run), search or grep within the `.memory` directory:

```bash
# Search all logs for a keyword
grep -r "keyword" .memory/

# Search within a specific date
grep -r "keyword" .memory/2025-01-15/

# List all runs for a given day
ls .memory/2025-01-15/
```