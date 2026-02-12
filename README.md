# Elephant

A VM0 agent that never forgets. Automatically archives all your VM0 agent run logs into `.memory/YYYY-MM-DD/<runId>.log`, organized by date.

## Skill

Elephant ships a built-in skill at `elephant/SKILLS.md` that other agents can use to search through archived history. To add it to another agent:

```yaml
# vm0.yaml
agents:
  my-agent:
    framework: claude-code
    skills:
      - https://github.com/e7h4n/elephant/tree/main/elephant
```

This gives your agent the ability to grep through `.memory/` to look up past run logs when it needs historical context.

## Setup

### 1. Get VM0 Token

```bash
vm0 auth setup-token
```

Copy the token and store it as a secret:

```bash
vm0 secret set VM0_TOKEN --body "<your-token>"
```

### 2. Deploy

```bash
vm0 compose https://github.com/e7h4n/elephant
```

### 3. Initialize History

On first run, backfill all historical logs:

```bash
vm0 run elephant-memory --artifact-name artifact \
  "Rebuild all memory: fetch ALL runs using vm0 run ls --all --limit 100, paginate with --until to get every run. For each run, save logs to .memory/YYYY-MM-DD/<runId>.log. Skip runs that already have a log file."
```

### 4. Schedule Daily Archival

```bash
vm0 schedule setup elephant-memory \
  -f daily \
  -t 00:00 \
  -z <your-timezone> \
  -p "Archive all runs from the past 24 hours. Save logs to .memory/YYYY-MM-DD/<runId>.log" \
  --artifact-name artifact \
  -e
```

Find your IANA timezone [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) (e.g. `America/New_York`, `Europe/London`).
